---
title: Hook di accesso personalizzato SAML 2.0
description: Scopri come sviluppare un hook di accesso SAML 2.0 personalizzato per AEM.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# Hook di accesso SAML 2.0

Scopri come sviluppare un hook di accesso SAML 2.0 personalizzato per AEM. Questa esercitazione fornisce istruzioni dettagliate per creare un hook di accesso personalizzato che si integra con un provider di identità SAML 2.0, consentendo agli utenti di eseguire l’autenticazione utilizzando le proprie credenziali SAML.

Se l’IDP non è in grado di inviare i dati del profilo utente e l’iscrizione al gruppo di utenti nell’asserzione SAML, oppure se i dati devono essere trasformati prima della sincronizzazione con AEM, è possibile implementare hook SAML personalizzati per estendere il processo di autenticazione SAML. Gli hook SAML consentono di personalizzare l’assegnazione dell’appartenenza al gruppo, modificare gli attributi del profilo utente e aggiungere regole business personalizzate durante il flusso di autenticazione.

>[!NOTE]
>Gli hook SAML personalizzati sono supportati in **AEM come Cloud Service** e **AEM LTS**. Questa funzione non è disponibile nelle versioni precedenti di AEM.

## Casi d’uso comuni

Gli hook SAML personalizzati sono utili quando è necessario:

+ Assegnazione dinamica dell&#39;appartenenza a gruppi in base a una logica di business personalizzata oltre a quella fornita nelle asserzioni SAML
+ Trasforma o arricchisci i dati del profilo utente prima che vengano sincronizzati con AEM
+ Mappare strutture complesse di attributi SAML alle proprietà utente di AEM
+ Implementare regole di autorizzazione personalizzate o assegnazioni di gruppi condizionali
+ Aggiungi registrazione o controllo personalizzato durante l’autenticazione SAML
+ Integrazione con sistemi esterni durante il processo di autenticazione

## Interfaccia del servizio OSGi `SamlHook`

L&#39;interfaccia `com.adobe.granite.auth.saml.spi.SamlHook` fornisce due metodi hook che vengono richiamati in fasi diverse del processo di autenticazione SAML:

### metodo `postSamlValidationProcess()`

Questo metodo viene chiamato **dopo** la risposta SAML è stata convalidata, ma **prima** dell&#39;avvio del processo di sincronizzazione utente. Questa è la posizione ideale per modificare i dati di asserzione SAML, ad esempio per aggiungere o trasformare attributi.

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### Casi d’uso

+ Aggiungere appartenenze di gruppo aggiuntive all&#39;asserzione
+ Trasforma i valori degli attributi prima della sincronizzazione
+ Arricchire l’asserzione con dati provenienti da origini esterne
+ Convalidare regole business personalizzate

### metodo `postSyncUserProcess()`

Questo metodo viene chiamato **dopo** che il processo di sincronizzazione utenti è stato completato. Questo hook può essere utilizzato per eseguire operazioni aggiuntive dopo la creazione o l’aggiornamento dell’utente AEM.

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### Casi d’uso

+ Aggiornare proprietà aggiuntive del profilo utente non coperte dalla sincronizzazione standard
+ Creazione o aggiornamento di risorse personalizzate relative agli utenti in AEM
+ Attiva flussi di lavoro o notifiche dopo l’autenticazione dell’utente
+ Registra eventi di autenticazione personalizzati

**Important:** To modify user properties in the repository, the hook implementation requires:

+ Un riferimento `SlingRepository` inserito tramite `@Reference`
+ Un [utente del servizio](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) configurato con le autorizzazioni appropriate (configurato in &quot;Apache Sling Service User Mapper Service Amendment&quot;)
+ Gestione corretta delle sessioni con blocchi try-catch-finally

## Implementare un hook SAML personalizzato

I passaggi seguenti descrivono come creare e distribuire un hook SAML personalizzato.

### Creazione dell’implementazione hook SAML

Creare una nuova classe Java nel progetto AEM che implementa l&#39;interfaccia `com.adobe.granite.auth.saml.spi.SamlHook`:

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### Configurare l’hook SAML

L’hook SAML utilizza la configurazione OSGi per specificare a quale IDP deve essere applicato. Crea un file di configurazione OSGi nel progetto in:

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

`idpIdentifier` deve corrispondere al valore `idpIdentifier` configurato nella configurazione di fabbrica OSGi del gestore di autenticazione SAML corrispondente (PID: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`). Questa corrispondenza è critica: l&#39;hook SAML verrà richiamato solo per l&#39;istanza del gestore di autenticazione SAML con lo stesso valore `idpIdentifier`. Il gestore di autenticazione SAML è una configurazione di fabbrica, ovvero è possibile avere più istanze (ad esempio, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`) e ogni hook è associato a un gestore specifico tramite `idpIdentifier`. La proprietà `service.ranking` controlla l&#39;ordine di esecuzione quando sono configurati più hook (vengono eseguiti prima i valori più alti).

### Aggiungi dipendenze Maven

Aggiungi la dipendenza SPI SAML richiesta a `pom.xml` del progetto principale AEM Maven.

**Per i progetti AEM as a Cloud Service**, utilizza la dipendenza API dell&#39;SDK dell&#39;AEM che include le interfacce SAML:

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

L&#39;artifact `aem-sdk-api` contiene tutte le interfacce SAML Adobe Granite necessarie, tra cui `com.adobe.granite.auth.saml.spi.SamlHook`.

### Configurare l&#39;utente del servizio (facoltativo)

Se l&#39;hook SAML deve modificare il contenuto nell&#39;archivio JCR di AEM, ad esempio le proprietà dell&#39;utente (come mostrato nell&#39;esempio `postSyncUserProcess`), è necessario configurare un [utente del servizio](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/developing/advanced/service-users):

1. Creare una mappatura utente del servizio nel progetto in `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json`:

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. Creare uno script Repoinit per definire l&#39;utente e le autorizzazioni del servizio in `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json`:

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

In questo modo l&#39;utente del servizio può leggere e modificare le proprietà dell&#39;utente nel repository.

### Distribuzione all&#39;AEM

Distribuisci l’hook SAML personalizzato in AEM as a Cloud Service:

1. Creare il progetto AEM
1. Eseguire il commit del codice nell’archivio Git di Cloud Manager
1. Distribuire utilizzando una pipeline di distribuzione full stack
1. L’hook SAML viene attivato automaticamente quando un utente si autentica tramite SAML


### Considerazioni importanti

+ **Identificatore IDP corrispondente**: `idpIdentifier` configurato nell&#39;hook SAML deve corrispondere esattamente a `idpIdentifier` nella configurazione del factory del gestore di autenticazione SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)
+ **Nomi di attributo**: verificare che i nomi di attributo a cui si fa riferimento nell&#39;hook (ad esempio, `groupMembership`) corrispondano agli attributi configurati nel gestore di autenticazione SAML
+ **Prestazioni**: alleggerisci le implementazioni hook man mano che vengono eseguite durante ogni autenticazione SAML
+ **Gestione errori**: le implementazioni dell&#39;hook SAML devono generare `com.adobe.granite.auth.saml.spi.SamlHookException` quando si verificano errori critici che non dovrebbero consentire l&#39;autenticazione. Il gestore di autenticazione SAML rileva queste eccezioni e restituisce `AuthenticationInfo.FAIL_AUTH`. Per le operazioni dell&#39;archivio, rilevare sempre `RepositoryException` e registrare gli errori in modo appropriato. Utilizza i blocchi try-catch-finally per garantire la corretta pulizia delle risorse
+ **Verifica**: verifica accuratamente gli hook personalizzati in ambienti inferiori prima della distribuzione in produzione
+ **Hook multipli**: è possibile configurare più implementazioni di hook SAML; tutti gli hook corrispondenti verranno eseguiti. Utilizzare la proprietà `service.ranking` nel componente OSGi per controllare l&#39;ordine di esecuzione (i valori di classificazione più alti vengono eseguiti per primi). Per riutilizzare un hook SAML in più configurazioni factory del gestore di autenticazione SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`), creare più configurazioni hook (configurazioni factory OSGi), ciascuna con un `idpIdentifier` diverso corrispondente al rispettivo gestore di autenticazione SAML
+ **Sicurezza**: convalida e bonifica tutti i dati dalle asserzioni SAML prima di utilizzarli nella logica di business
+ **Repository access**: When modifying user properties in `postSyncUserProcess`, always use a [service user](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) with appropriate permissions rather than administrative sessions
+ **Autorizzazioni utente del servizio**: concedere autorizzazioni minime richieste all&#39;utente [del servizio](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) (ad esempio, solo `jcr:read` e `rep:write` in `/home/users`, non diritti di amministratore completi)
+ **Gestione delle sessioni**: utilizzare sempre i blocchi try-catch-finally per garantire la corretta chiusura delle sessioni dell&#39;archivio, anche in caso di eccezioni
+ **Intervallo di sincronizzazione utenti**: l&#39;hook `postSyncUserProcess` viene eseguito dopo che l&#39;utente è stato sincronizzato con OAK, pertanto l&#39;oggetto utente sarà sicuramente presente nell&#39;archivio a quel punto
