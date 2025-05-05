---
title: Gestire i segreti in AEM as a Cloud Service
description: Scopri le best practice per la gestione dei segreti in AEM as a Cloud Service, utilizzando gli strumenti e le tecniche forniti da AEM per proteggere le informazioni sensibili, garantendo la sicurezza e la riservatezza dell’applicazione.
version: Experience Manager as a Cloud Service
topic: Development, Security
feature: OSGI, Cloud Manager
role: Developer
jira: KT-15880
level: Intermediate, Experienced
exl-id: 856b7da4-9ee4-44db-b245-4fdd220e8a4e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# Gestione dei segreti in AEM as a Cloud Service

La gestione dei segreti, come le chiavi API e le password, è fondamentale per mantenere la sicurezza delle applicazioni. Adobe Experience Manager (AEM) as a Cloud Service offre solidi strumenti per gestire i segreti in modo sicuro.

Questa esercitazione illustra le best practice per la gestione dei segreti in AEM. Verranno illustrati gli strumenti e le tecniche forniti da AEM per proteggere le informazioni riservate, garantendo la protezione e la riservatezza dell&#39;applicazione.

Questo tutorial presuppone una conoscenza operativa dello sviluppo Java di AEM, dei servizi OSGi, dei modelli Sling e di Adobe Cloud Manager.

## Servizio OSGi di gestione segreti

In AEM as a Cloud Service, la gestione dei segreti tramite i servizi OSGi offre un approccio scalabile e sicuro. I servizi OSGi possono essere configurati per gestire informazioni riservate, come chiavi API e password, definite tramite configurazioni OSGi e impostate tramite Cloud Manager.

### Implementazione del servizio OSGi

Seguiremo lo sviluppo di un servizio OSGi personalizzato che [espone segreti relativi alle configurazioni OSGi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values).

L&#39;implementazione legge i segreti dalla configurazione OSGi tramite il metodo `@Activate` e li espone tramite il metodo `getSecret(String secretName)`. In alternativa, è possibile creare metodi discreti come `getApiKey()` per ogni segreto, ma questo approccio richiede più manutenzione in quanto i segreti vengono aggiunti o rimossi.

```java
package com.example.core.util.impl;

import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.apache.sling.api.resource.ValueMap;
import org.apache.sling.api.resource.ValueMapDecorator;
import java.util.Map;

@Component(
    service = { SecretsManager.class }
)
public class SecretsManagerImpl implements SecretsManager {
    private static final Logger log = LoggerFactory.getLogger(SecretsManagerImpl.class);
 
    private ValueMap secrets;

    @Override
    public String getSecret(String secretName) {
        return secrets.get(secretName, String.class);
    }

    @Activate
    @Modified
    protected void activate(Map<String, Object> properties) {
        secrets = new ValueMapDecorator(properties);
    }
}
```

In qualità di servizio OSGi, è consigliabile registrarlo e utilizzarlo tramite un’interfaccia Java. Di seguito è riportata una semplice interfaccia che consente ai consumatori di ottenere segreti in base al nome della proprietà OSGi.

```java
package com.example.core.util;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface SecretsManager {
    String getSecret(String secretName);
}
```

## Mappatura dei segreti sulla configurazione OSGi

Per esporre i valori segreti nel servizio OSGi, mapparli sulle configurazioni OSGi utilizzando [valori di configurazione segreti OSGi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values). Definisci il nome della proprietà OSGi come chiave per recuperare il valore segreto dal metodo `SecretsManager.getSecret()`.

Definisci i segreti nel file di configurazione OSGi `/apps/example/osgiconfig/config/com.example.core.util.impl.SecretsManagerImpl.cfg.json` nel progetto AEM Maven. Ogni proprietà rappresenta un segreto esposto in AEM, con il valore impostato tramite Cloud Manager. La chiave è il nome della proprietà OSGi, utilizzato per recuperare il valore segreto dal servizio `SecretsManager`.

```json
{
    "api.key": "$[secret:api_key]",
    "service.password": "$[secret:service_password]"
}
```

In alternativa all’utilizzo di un servizio OSGi per la gestione dei segreti condivisi, puoi includere i segreti direttamente nella configurazione OSGi di servizi specifici che li utilizzano. Questo approccio è utile quando i segreti sono necessari solo per un singolo servizio OSGi e non sono condivisi tra più servizi. In questo caso, i valori segreti sono definiti nel file di configurazione OSGi per il servizio specifico e sono accessibili nel codice Java del servizio tramite il metodo `@Activate`.

## Utilizzo dei segreti

I segreti possono essere utilizzati dal servizio OSGi in vari modi, ad esempio da un modello Sling o da un altro servizio OSGi. Di seguito sono riportati alcuni esempi di come sfruttare i segreti di entrambi.

### Da modello Sling

I modelli Sling spesso forniscono una logica di business per i componenti del sito AEM. Il servizio OSGi `SecretsManager` può essere utilizzato tramite l&#39;annotazione `@OsgiService` e utilizzato all&#39;interno del modello Sling per recuperare il valore segreto.

```java
import com.example.core.util.SecretsManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingHttpServletRequest;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.OsgiService;

@Model(
    adaptables = {SlingHttpServletRequest.class, Resource.class},
    adapters = {ExampleDatabaseModel.class}
)
public class ExampleDatabaseModelImpl implements ExampleDatabaseModel {

    @OsgiService
    SecretsManager secretsManager;

    @Override 
    public String doWork() {
        final String secret = secretsManager.getSecret("api.key");
        // Do work with secret
    }
}
```

### Da servizio OSGi

I servizi OSGi spesso espongono una logica di business riutilizzabile all’interno di AEM, utilizzata dai modelli Sling, dai servizi AEM come Flussi di lavoro o da altri servizi OSGi personalizzati. Il servizio OSGi `SecretsManager` può essere utilizzato tramite l&#39;annotazione `@Reference` e utilizzato all&#39;interno del servizio OSGi per recuperare il valore segreto.

```java
import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component
public class ExampleSecretConsumerImpl implements ExampleSecretConsumer {

    @Reference
    SecretsManager secretsManager;

    public void doWork() {
        final String secret = secretsManager.getSecret("service.password");
        // Do work with the secret
    }
}
```

## Impostazione dei segreti in Cloud Manager

Con il servizio e la configurazione OSGi attivati, l’ultimo passaggio consiste nell’impostare i valori segreti in Cloud Manager.

I valori per i segreti possono essere impostati tramite [API Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#tag/Variables) o, più comunemente, tramite la [interfaccia utente Cloud Manager](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#overview). Per applicare una variabile segreta tramite l’interfaccia utente di Cloud Manager:

![Configurazione segreti Cloud Manager](./assets/secrets/cloudmanager-configuration.png)

1. Accedi a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com).
1. Seleziona il programma e l’ambiente AEM per i quali desideri impostare il segreto.
1. Nella visualizzazione Dettagli ambiente, selezionare la scheda **Configurazione**.
1. Seleziona **Aggiungi**.
1. Nella finestra di dialogo Configurazione ambiente:
   - Immetti il nome della variabile segreta (ad esempio, `api_key`) a cui si fa riferimento nella configurazione OSGi.
   - Immetti il valore segreto.
   - Seleziona il servizio AEM a cui si applica il segreto.
   - Seleziona **Segreto** come tipo.
1. Seleziona **Aggiungi** per mantenere il segreto.
1. Aggiungi tutti i segreti necessari. Al termine, seleziona **Salva** per applicare immediatamente le modifiche all&#39;ambiente AEM.

L’utilizzo delle configurazioni di Cloud Manager per i segreti offre il vantaggio di applicare valori diversi per ambienti o servizi diversi e di ruotare i segreti senza ridistribuire l’applicazione AEM.
