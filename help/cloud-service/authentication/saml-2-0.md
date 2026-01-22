---
title: SAML 2.0 su AEM come Cloud Service
description: Scopri come configurare l'autenticazione SAML 2.0 su AEM come servizio Publish Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 2ed303e316577363f6d1c265ef7f9cd6d81491d8
workflow-type: tm+mt
source-wordcount: '4277'
ht-degree: 1%

---

# Autenticazione SAML 2.0{#saml-2-0-authentication}

Scopri come configurare e autenticare gli utenti finali (non AEM gli autori) in un IDP compatibile con SAML 2.0 di tua scelta.

## Quale SAML per AEM come Cloud Service?

L&#39;integrazione SAML 2.0 con AEM Publish (o Anteprima) consente agli utenti finali di un&#39;esperienza web basata su AEM di autenticarsi presso un IDP (Identity Provider) non Adobe Systems e di accesso AEM come utente nominativo e autorizzato.

|                       | AEM Author | Pubblicazione AEM |
|-----------------------|:----------:|:-----------:|
| Supporto SAML 2.0 | ✘ | ✔ |

+++ Comprendere il flusso SAML 2.0 con AEM

Il flusso tipico di un’integrazione SAML di pubblicazione AEM è il seguente:

1. L’utente invia una richiesta ad AEM Publish, che indica che è necessaria l’autenticazione.
   + L’utente richiede una risorsa protetta da CUG/ACL.
   + L’utente richiede una risorsa soggetta a un requisito di autenticazione.
   + L&#39;utente segue un collegamento all&#39;endpoint di accesso di AEM (ad esempio `/system/sling/login`) che richiede esplicitamente l&#39;azione di accesso.
1. AEM invia una AuthnRequest all’IDP, richiedendo a quest’ultimo di avviare il processo di autenticazione.
1. L&#39;utente si autentica in IDP.
   + L&#39;IDP richiede all&#39;utente le credenziali.
   + L&#39;utente è già autenticato con l&#39;IDP e non deve fornire ulteriori credenziali.
1. IDP genera un&#39;asserzione SAML contenente i dati dell&#39;utente e la firma utilizzando il certificato privato dell&#39;IDP.
1. IDP invia l’asserzione SAML tramite HTTP POST, tramite il browser web dell’utente (RESPECTIVE_PROTECTED_PATH/saml_login), ad AEM Publish.
1. AEM Publish riceve l’asserzione SAML e ne convalida l’integrità e l’autenticità utilizzando il certificato pubblico IDP.
1. AEM Publish gestisce il record utente di AEM in base alla configurazione OSGi SAML 2.0 e al contenuto dell’asserzione SAML.
   + Crea utente
   + Sincronizza gli attributi utente
   + Aggiorna l&#39;iscrizione al gruppo utenti di AEM
1. AEM Publish imposta il cookie AEM `login-token` nella risposta HTTP, utilizzato per autenticare le richieste successive in AEM Publish.
1. AEM Publish reindirizza l&#39;utente all&#39;URL in AEM Publish come specificato dal cookie `saml_request_path`.

+++

## Procedura dettagliata della configurazione

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

Questo video illustra come configurare l’integrazione SAML 2.0 con il servizio AEM as a Cloud Service Publish e utilizzare Okta come IDP.

## Prerequisiti

Per configurare l’autenticazione SAML 2.0 sono necessari i seguenti elementi:

+ Deployment Manager accesso a Cloud Manager
+ AEM amministratore accesso ad AEM come ambiente Cloud Service
+ L&#39;amministratore accesso all&#39;IDP
+ Facoltativamente, accesso a una coppia di chiavi pubblica/privata utilizzata per crittografare i payload SAML
+ AEM Sites pagine (o strutture ad albero), pubblicate in Publish AEM e [protette da gruppi di utenti chiusi (CUG)](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties#permissions)

SAML 2.0 è supportato solo per autenticare gli usi per AEM Publish o Anteprima. Per gestire l&#39;autenticazione di AEM Autore che utilizza e IDP, [integrare l&#39;IDP con Adobe Systems IMS](https://helpx.adobe.com/it/enterprise/using/set-up-identity.html).


## Installare il certificato pubblico IDP nel AEM

Il certificato pubblico dell&#39;IDP viene aggiunto all&#39;archivio attendibilità globale dell&#39;AEM e utilizzato per convalidare che l&#39;asserzione SAML inviata dall&#39;IDP è valida.

+++Flusso di firma asserzione SAML

![SAML 2.0 - IDP Firma asserzione SAML](./assets/saml-2-0/idp-signing-diagram.png)

1. L&#39;utente esegue l&#39;autenticazione all&#39;IDP.
1. IDP genera un&#39;asserzione SAML contenente i dati del utente.
1. IDP firma l&#39;asserzione SAML utilizzando il certificato privato dell&#39;IDP.
1. IDP avvia un POST HTTP lato client per AEM&#39;endpoint SAML del Publish (`.../saml_login`) che include l&#39;asserzione SAML firmata.
1. AEM Publish riceve il POST HTTP contenente l&#39;asserzione SAML firmata, può convalidare la firma utilizzando il certificato pubblico IDP.

+++

![Aggiungere il certificato pubblico IDP all&#39;archivio attendibilità globale](./assets/saml-2-0/global-trust-store.png)

1. Ottenere il file di __certificato__ pubblico dall&#39;IDP. Questo certificato AEM consente di convalidare l&#39;asserzione SAML fornita all&#39;AEM dall&#39;IDP.

   Il certificato è in formato PEM e deve essere simile a:

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Accedi all AEM Autore come amministratore AEM.
1. Passare a __Strumenti > Security > Trust Store__.
1. Crea o apri il Global Trust Store. Se crei un Global Trust Store, store il password un posto sicuro.
1. Espandere __Aggiungi certificato da file CER__.
1. Seleziona __File__ certificati e carica il file del certificato fornito dall&#39;IDP.
1. Lascia __vuoto Map Certificate to User__ .
1. Seleziona __Invia__.
1. Il certificato appena aggiunto viene visualizzato sopra la __sezione Aggiungi certificato da file__ CRT.
1. Prendi nota dell&#39;alias ____, poiché questo valore viene utilizzato nella configurazione[ OSGi di ](#saml-2-0-authentication-handler-osgi-configuration)SAML 2.0 Authentication Handler.
1. Seleziona __Salva e chiudi__.

L&#39;archivio attendibilità globale è configurato con il certificato pubblico dell&#39;IDP in Autore AEM, ma poiché SAML viene utilizzato solo su AEM Publish, l&#39;archivio attendibilità globale deve essere replicato in AEM Publish affinché il certificato pubblico IDP sia accessibile lì.

![Replicare l&#39;archivio attendibilità globale in AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. Passare a __Strumenti > Deployment > Packages__.
1. Crea un pacchetto
   + Nome pacchetto: `Global Trust Store`
   + Versione: `1.0.0`
   + Gruppo: `com.your.company`
1. Modifica il nuovo __pacchetto Archivio__ attendibilità globale.
1. Seleziona la __scheda Filtri__ e aggiungi un filtro per il percorso `/etc/truststore`principale.
1. Seleziona __Fine__ e quindi __Salva__.
1. Selezionare il pulsante __Genera__ per il pacchetto __Archivio attendibilità globale__.
1. Una volta generato, selezionare __Altro__ > __Replica__ per attivare il nodo dell&#39;archivio fonti attendibili globale (`/etc/truststore`) in AEM Publish.

## Crea keystore del servizio di autenticazione{#authentication-service-keystore}

_È necessario creare un keystore per il servizio di autenticazione quando la proprietà di configurazione OSGi [ del gestore di autenticazione `handleLogout`SAML 2.0 è impostata su `true`](#saml-20-authenticationsaml-2-0-authentication) o quando [è richiesta la firma AuthnRequest/la crittografia dell&#39;asserzione SAML](#install-aem-public-private-key-pair)_

1. Accedi ad AEM Author come amministratore di AEM per caricare la chiave privata.
1. Passa a __Strumenti > Protezione > Utenti__, seleziona l&#39;utente __authentication-service__ e seleziona __Proprietà__ dalla barra delle azioni superiore.
1. Selezionare la scheda __Registro chiavi__.
1. Crea o apri il keystore. Se crei un keystore, proteggi la password.
   + Un keystore [pubblico/privato è installato in questo keystore](#install-aem-public-private-key-pair) solo se è richiesta la firma AuthnRequest/la crittografia dell&#39;asserzione SAML.
   + Se questa integrazione SAML supporta la disconnessione ma non l&#39;asserzione di firma AuthnRequest/SAML, è sufficiente un keystore vuoto.
1. Seleziona __Salva e chiudi__.
1. Crea un pacchetto contenente l&#39;utente __authentication-service__ aggiornato.

   _Utilizza la seguente soluzione alternativa temporanea utilizzando i pacchetti :_

   1. Passa a __Strumenti > Distribuzione > Pacchetti__.
   1. Creare un pacchetto
      + Nome pacchetto: `Authentication Service`
      + Versione: `1.0.0`
      + Gruppo: `com.your.company`
   1. Modifica il nuovo __pacchetto dell&#39;archivio__ chiavi del servizio Authentication.
   1. Seleziona la __scheda Filtri__ e aggiungi un filtro per il percorso `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`principale.
      + È `<AUTHENTICATION SERVICE UUID>` possibile trovarlo accedendo a __Strumenti > Security > Users__ e selezionando __authentication-service__ utente. L&#39;UUID è l&#39;ultima parte del URL.
   1. Selezionare __Fine__ , quindi __Salva__.
   1. Selezionare il __pulsante di compilazione__ per il pacchetto dell&#39;archivio __chiavi__ del servizio Authentication.
   1. Una volta generato, seleziona __Altro__ > __Replica__ per attivare l&#39;archivio chiavi del servizio di autenticazione in AEM Publish.

## Installare una coppia di chiavi pubblica/privata di AEM{#install-aem-public-private-key-pair}

_L&#39;installazione della coppia di chiavi pubblica/privata di AEM è facoltativa_

AEM Publish può essere configurato per firmare AuthnRequests (in IDP) e crittografare le asserzioni SAML (in AEM). Ciò si ottiene fornendo una chiave privata per AEM Publish e la chiave pubblica corrispondente all&#39;IDP.

+++ Comprendere il flusso di firma AuthnRequest (facoltativo)

L&#39;AuthnRequest (l&#39;richiesta all&#39;IDP da AEM Publish che avvia il processo di login) può essere firmato da AEM Publish. A tale scopo, AEM Publish firmi AuthnRequest utilizzando la chiave privata, che l&#39;IDP convalidi la firma utilizzando la chiave pubblica. Ciò garantisce all&#39;IDP che AuthnRequest è stato avviato e richiesto da AEM Publish e non da una terza parte malintenzionata.

![SAML 2.0 - Firma SP AuthnRequest](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. L&#39;utente effettua una richiesta HTTP a AEM Publish che si traduce in un&#39;autenticazione SAML richiesta all&#39;IDP.
1. AEM Publish genera il richiesta SAML da inviare all&#39;IDP.
1. AEM Publish firma il richiesta SAML utilizzando la chiave privata di AEM.
1. AEM Publish avvia AuthnRequest, un reindirizzare HTTP lato client all&#39;IDP che contiene il richiesta SAML firmato.
1. IDP riceve AuthnRequest e convalida la firma utilizzando la chiave pubblica di AEM, garantendo AEM Publish avviato AuthnRequest.
1. AEM Publish convalida quindi l&#39;integrità e l&#39;autenticità dell&#39;asserzione SAML decrittografata utilizzando il certificato pubblico IDP.

+++

+++ Comprendere il flusso di crittografia delle asserzioni SAML (facoltativo)

Tutte le comunicazioni HTTP tra IDP e AEM Publish devono avvenire tramite HTTPS e quindi sicure per impostazione predefinita. Tuttavia, se necessario, le asserzioni SAML possono essere crittografate nel caso in cui sia richiesta una maggiore riservatezza oltre a quella fornita da HTTPS. A tale scopo, l&#39;IDP crittografa i dati dell&#39;asserzione SAML utilizzando la chiave privata e AEM Publish decrittografa l&#39;asserzione SAML utilizzando la chiave privata.

![SAML 2.0 - Crittografia asserzione SP SAML](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. L&#39;utente esegue l&#39;autenticazione all&#39;IDP.
1. IDP genera un&#39;asserzione SAML contenente i dati dell&#39;utente e la firma utilizzando il certificato privato dell&#39;IDP.
1. IDP crittografa quindi l&#39;asserzione SAML con la chiave pubblica di AEM, che richiede la decrittografia della AEM chiave privata.
1. L&#39;asserzione SAML crittografata viene inviata, tramite l&#39;browser web del utente a AEM Publish.
1. AEM Publish riceve l&#39;asserzione SAML e la decrittografa utilizzando la chiave privata del AEM.
1. L&#39;IDP richiede utente di eseguire l&#39;autenticazione.

+++

Sia la firma AuthnRequest che la crittografia delle asserzioni SAML sono facoltative, tuttavia sono entrambe abilitate, utilizzando la proprietà [di configurazione OSGi del gestore di `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)autenticazione SAML 2.0, il che significa che entrambe o nessuna delle due può essere utilizzata.

![AEM chiave del servizio di autenticazione store](./assets/saml-2-0/authentication-service-key-store.png)

1. Ottenere la chiave pubblica, la chiave privata (PKCS#8 in formato DER) e il file della catena di certificati (potrebbe essere la chiave pubblica) utilizzati per firmare AuthnRequest e crittografare l&#39;asserzione SAML. Le chiavi vengono in genere fornite dai team di sicurezza dell&#39;organizzazione IT.

   + Una coppia di chiavi autofirmata può essere generata usando __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Carica la chiave pubblica nell&#39;IDP.
   + Utilizzando il `openssl` metodo indicato in precedenza, la chiave pubblica è il `aem-public.crt` file.
1. Effettua l&#39;accesso all AEM Autore come amministratore AEM per caricare la chiave privata.
1. Passare a __Strumenti > Security > Trust Store__, selezionare __utente del servizio__ di autenticazione, quindi selezionare __Proprietà__ dalla barra delle azioni superiore.
1. Passare a __Strumenti > Security > Users__, selezionare __utente del servizio__ di autenticazione, quindi selezionare __Proprietà__ dalla barra delle azioni superiore.
1. Selezionate l&#39;scheda __Registro chiavi__ .
1. Crea o apri il keystore. Se crei un keystore, proteggi la password.
1. Seleziona __Aggiungi chiave privata dal file DER__ e aggiungi la chiave privata e il file della catena ad AEM:
   + __Alias__: fornire un nome significativo, spesso il nome dell&#39;IDP.
   + __File di chiave privata__: carica il file di chiave privata (PKCS#8 in formato DER).
      + Utilizzando il metodo `openssl`, si tratta del file `aem-private-pkcs8.der`
   + __Selezionare il file della catena di certificati__: caricare il file della catena associato (potrebbe essere la chiave pubblica).
      + Utilizzando il metodo `openssl`, si tratta del file `aem-public.crt`
   + Seleziona __Invia__
1. Il certificato appena aggiunto viene visualizzato sopra la __sezione Aggiungi certificato da file__ CRT.
   + Prendi nota dell&#39;alias ____ utilizzato nella configurazione OSGi dell&#39;handler di [autenticazione SAML 2.0](#saml-20-authentication-handler-osgi-configuration)
1. Seleziona __Salva e chiudi__.
1. Crea un pacchetto contenente la utente aggiornata __del servizio__ di autenticazione.

   _Use la seguente soluzione temporanea utilizzando i pacchetti :_

   1. Passare a __Strumenti > Deployment > Packages__.
   1. Crea un pacchetto
      + Nome pacchetto: `Authentication Service`
      + Versione: `1.0.0`
      + Gruppo: `com.your.company`
   1. Modifica il nuovo pacchetto __Archivio chiavi del servizio di autenticazione__.
   1. Selezionare la scheda __Filtri__ e aggiungere un filtro per il percorso radice `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + È `<AUTHENTICATION SERVICE UUID>` possibile trovarlo accedendo a __Strumenti > Security > Users__ e selezionando __authentication-service__ utente. L&#39;UUID è l&#39;ultima parte del URL.
   1. Selezionare __Fine__ , quindi __Salva__.
   1. Selezionare il __pulsante di compilazione__ per il pacchetto dell&#39;archivio __chiavi__ del servizio Authentication.
   1. Una volta generato, selezionare __Altro__ > __Replica__ per attivare il store della chiave del servizio Authentication su AEM Publish.

## Configurare l&#39;handler di autenticazione SAML 2.0{#configure-saml-2-0-authentication-handler}

La configurazione SAML di AEM viene eseguita tramite la configurazione OSGi di __Adobe Systems Granite SAML 2.0 Authentication Handler__ .
La configurazione è una configurazione di fabbrica OSGi, il che significa che un singolo servizio di pubblicazione AEM as a Cloud Service può avere più strutture di risorse discrete di copertura della configurazione SAML dell’archivio; questo è utile per le distribuzioni AEM multisito.

+++ Glossario della configurazione OSGi del gestore autenticazione SAML 2.0

### Adobe Systems Configurazione OSGi di Granite SAML 2.0 Authentication Handler{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | Proprietà OSGi | Obbligatorio | Valore formato | Valore predefinito | Descrizione |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Percorsi | `path` | ✔ | Array di stringhe | `/` | AEM percorsi per cui viene utilizzato questo gestore di autenticazione. |
| URL IDP | `idpUrl` | ✔ | Stringa |                           | L&#39;IDP URL il richiesta di autenticazione SAML viene inviato. |
| Alias certificato IDP | `idpCertAlias` | ✔ | Stringa |                           | Alias del certificato IDP trovato nel Global Trust Store dell&#39;AEM |
| IDP HTTP reindirizzare | `idpHttpRedirect` | ✘ | Booleano | `false` | Indica se un reindirizzamento HTTP all&#39;IDP URL invece di inviare un AuthnRequest. Imposta su `true` per l&#39;autenticazione avviata da IDP. |
| Identificatore IDP | `idpIdentifier` | ✘ | Stringa |                           | ID IDP univoco per garantire l&#39;unicità di AEM utente e gruppo. Se questo campo è vuoto, viene utilizzato il `serviceProviderEntityId` valore desiderato. |
| Asserzione servizio consumer URL | `assertionConsumerServiceURL` | ✘ | Stringa |                           | L&#39;attributo `AssertionConsumerServiceURL` URL in AuthnRequest che specifica dove deve essere inviato il `<Response>` messaggio al AEM. |
| ID entità SP | `serviceProviderEntityId` | ✔ | Stringa |                           | Identifica in modo univoco AEM all&#39;IDP; Di solito il AEM nome host. |
| Crittografia SP | `useEncryption` | ✘ | Booleano | `true` | Indica se l&#39;IDP crittografa le asserzioni SAML. Richiede `spPrivateKeyAlias` e `keyStorePassword` deve essere impostato. |
| Alias chiave privata SP | `spPrivateKeyAlias` | ✘ | Stringa |                           | Alias della chiave privata nella `authentication-service` chiave del utente store. Obbligatorio se `useEncryption` è impostato su `true`. |
| Tasto SP store password | `keyStorePassword` | ✘ | Stringa |                           | L&#39;password delle chiavi del &quot;servizio di autenticazione&quot; utente store. Obbligatorio se `useEncryption` è impostato su `true`. |
| reindirizzare predefinito | `defaultRedirectUrl` | ✘ | Stringa | `/` | La reindirizzare predefinita URL dopo un&#39;autenticazione riuscita. Può essere relativo all&#39;host AEM (ad esempio, `/content/wknd/us/en/html`). |
| Attributo ID utente | `userIDAttribute` | ✘ | Stringa | `uid` | Nome dell&#39;attributo di asserzione SAML contenente l&#39;ID utente dell&#39;utente AEM. Lascia vuoto per usare .`Subject:NameId` |
| Automatico creare utenti AEM | `createUser` | ✘ | Booleano | `true` | Indica se gli utenti AEM vengono creati in seguito all&#39;autenticazione. |
| AEM utente percorso intermedio | `userIntermediatePath` | ✘ | Stringa |                           | Durante la creazione di utenti AEM, questo valore viene utilizzato come percorso intermedio (ad esempio, `/home/users/<userIntermediatePath>/jane@wknd.com`). Richiede `createUser` per essere impostato su `true`. |
| AEM attributi utente | `synchronizeAttributes` | ✘ | Array di stringhe |                           | Elenco dei mapping di attributi SAML da store nel utente AEM, nel formato `[ "saml-attribute-name=path/relative/to/user/node" ]` (ad esempio, `[ "firstName=profile/givenName" ]`). Vedi l&#39;elenco [completo degli attributi AEM nativo](#aem-user-attributes). |
| Aggiungere utente a AEM gruppi | `addGroupMemberships` | ✘ | Booleano | `true` | Indica se un utente di AEM viene aggiunto automaticamente ai gruppi di utenti di AEM dopo la corretta autenticazione. |
| Attributo di iscrizione al gruppo AEM | `groupMembershipAttribute` | ✘ | Stringa | `groupMembership` | Nome dell&#39;attributo di asserzione SAML contenente un elenco di AEM utente gruppi a cui deve essere aggiunto il utente. Richiede `addGroupMemberships` di essere impostato su `true`. |
| Gruppi di AEM predefiniti | `defaultGroups` | ✘ | Array di stringhe |                           | Viene sempre aggiunto un elenco di AEM utente gruppi a cui vengono sempre aggiunti gli utenti autenticati (ad esempio, `[ "wknd-user" ]`). Richiede `addGroupMemberships` di essere impostato su `true`. |
| Formato NameIDPolicy | `nameIdFormat` | ✘ | Stringa | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | Valore del parametro di formato NameIDPolicy da inviare nel messaggio AuthnRequest. |
| Risposta SAML dello store | `storeSAMLResponse` | ✘ | Booleano | `false` | Indica se il `samlResponse` valore è memorizzato nel nodo AEM `cq:User` . |
| Gestisci disconnessione | `handleLogout` | ✘ | Booleano | `false` | Indica se la richiesta di logout viene gestita da questo gestore di autenticazione SAML. Richiede `logoutUrl` di essere impostato. |
| Disconnessione URL | `logoutUrl` | ✘ | Stringa |                           | URL dell&#39;IDP a cui viene inviata la richiesta di disconnessione SAML. Obbligatorio se `handleLogout` è impostato su `true`. |
| Tolleranza clock | `clockTolerance` | ✘ | Numero intero | `60` | La tolleranza di sfasamento dell’orologio IDP e AEM (SP) durante la convalida delle asserzioni SAML. |
| Metodo digest | `digestMethod` | ✘ | Stringa | `http://www.w3.org/2001/04/xmlenc#sha256` | Algoritmo di digest utilizzato dall&#39;IDP per firmare un messaggio SAML. |
| Metodo di firma | `signatureMethod` | ✘ | Stringa | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Algoritmo di firma utilizzato dall&#39;IDP per firmare un messaggio SAML. |
| Tipo di sincronizzazione identità | `identitySyncType` | ✘ | `default` oppure `idp` | `default` | Non modificare `from` l&#39;impostazione predefinita per AEM come Cloud Service. |
| Servizi classificazione | `service.ranking` | ✘ | Numero intero | `5002` | Configurazioni di classificazione più elevate sono preferite per lo stesso `path`. |

### AEM attributi utente{#aem-user-attributes}

AEM utilizza i seguenti attributi di utente, che possono essere popolati tramite la `synchronizeAttributes` proprietà nella configurazione OSGi di Adobe Systems Granite SAML 2.0 Authentication Handler.  Qualsiasi attributo IDP può essere sincronizzato con qualsiasi proprietà utente di AEM, tuttavia la mappatura per AEM utilizzare le proprietà degli attributi (elencate di seguito) AEM consente di utilizzarli naturalmente.

| Attributo utente | Percorso relativo della proprietà dal `rep:User` nodo |
|--------------------------------|--------------------------|
| Titolo (ad esempio, `Mrs`) | `profile/title` |
| Nome (cioè nome) | `profile/givenName` |
| Cognome (cioè cognome) | `profile/familyName` |
| Qualifica | `profile/jobTitle` |
| Indirizzo e-mail | `profile/email` |
| Indirizzo | `profile/street` |
| Città | `profile/city` |
| CAP | `profile/postalCode` |
| Paese | `profile/country` |
| Numero di telefono | `profile/phoneNumber` |
| Informazioni su di me | `profile/aboutMe` |

+++

1. Crea un file di configurazione OSGi nel progetto e `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` aprirlo nell&#39;IDE.
   + Cambia `/wknd-examples/` in `/<project name>/`
   + L&#39;identificatore dopo `~` nel nome file deve identificare in modo univoco questa configurazione, quindi potrebbe essere il nome dell&#39;IDP, ad esempio `...~okta.cfg.json`. Il valore deve essere alfanumerico con trattini.
1. Incolla il seguente codice JSON nel `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` file e aggiorna i `wknd` riferimenti in base alle esigenze.

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. Aggiorna i valori come richiesto dal progetto. Per le descrizioni delle proprietà di configurazione, consulta il glossario __di configurazione OSGi dell&#39;handler__ SAML 2.0 Authentication riportato sopra. Dovrebbe `path` contenere gli alberi contenuto protetti da gruppi di utenti chiusi (CUG) e richiedono l&#39;autenticazione e questo gestore di autenticazione dovrebbe essere responsabile della protezione.
1. È consigliato, ma non obbligatorio, utilizzare variabili di ambiente OSGi e segreti, quando i valori possono cambiare fuori Sincronizzazione con il ciclo di rilascio o quando i valori differiscono tra tipi di ambiente / livelli di servizio simili. I valori predefiniti possono essere impostati utilizzando la `$[env:..;default=the-default-value]"` sintassi come mostrato sopra.

Le configurazioni OSGi per ambiente (`config.publish.dev`, `config.publish.stage`, e `config.publish.prod`) possono essere definite con attributi specifici se la configurazione SAML varia da un ambiente all&#39;altro.

### Usa crittografia

Quando [si crittografano le asserzioni AuthnRequest e SAML](#encrypting-the-authnrequest-and-saml-assertion), sono necessarie le seguenti proprietà: `useEncryption`, `spPrivateKeyAlias` e `keyStorePassword`. `keyStorePassword` contiene una password, pertanto il valore non deve essere memorizzato nel file di configurazione OSGi, ma inserito utilizzando [valori di configurazione segreti](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=it#secret-configuration-values)

+++Facoltativamente, aggiornare la configurazione OSGi per utilizzare la crittografia

1. Apri `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` nell&#39;IDE.
1. Aggiungi le tre proprietà `useEncryption`, `spPrivateKeyAlias`, e `keyStorePassword` come mostrato di seguito.

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. Le tre proprietà di configurazione OSGi richieste per la crittografia sono:

+ `useEncryption` imposta su `true`
+ `spPrivateKeyAlias` contiene l&#39;alias della voce del keystore per la chiave privata usata dall&#39;integrazione SAML.
+ `keyStorePassword` contiene una [variabile](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=it#secret-configuration-values) di configurazione segreta OSGi contenente l&#39;password del `authentication-service` keystore utente.

+++

## Configura filtro referrer

Durante il processo di autenticazione SAML, l&#39;IDP avvia un POST HTTP lato client per AEM l&#39;endpoint del `.../saml_login` Publish. Se l&#39;IDP e la pubblicazione AEM esistono in un&#39;origine diversa, il __filtro referrer__ di AEM Publish viene configurato tramite la configurazione OSGi per consentire i POST HTTP dall&#39;origine dell&#39;IDP.

1. Crea (o modifica) un file di configurazione OSGi nel progetto in `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Cambia `/wknd-examples/` in `/<project name>/`
1. Verificare che il valore `allow.empty` sia impostato su `true`, che `allow.hosts` (o se si preferisce, `allow.hosts.regexp`) contenga l&#39;origine dell&#39;IDP e che `filter.methods` includa `POST`. La configurazione OSGi deve essere simile a:

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM Publish supporta una singola configurazione del filtro Referrer, in modo da unire i requisiti di configurazione SAML con eventuali configurazioni esistenti.

Le configurazioni OSGi per ambiente (`config.publish.dev`, `config.publish.stage` e `config.publish.prod`) possono essere definite con attributi specifici se `allow.hosts` (o `allow.hosts.regex`) variano da un ambiente all&#39;altro.

## Configurare la condivisione CORS (Cross-Origin Resource Sharing)

Durante il processo di autenticazione SAML, l&#39;IDP avvia un HTTP POST lato client all&#39;endpoint `.../saml_login` di AEM Publish. Se l&#39;IDP e la pubblicazione AEM esistono su host/domini diversi, è necessario configurare la condivisione CORS (CRoss-Origin Resource Sharing)__di AEM Publish per consentire i POST HTTP dall&#39;host/dominio dell&#39;IDP.__

L&#39;intestazione `Origin` di questa richiesta HTTP POST in genere ha un valore diverso rispetto all&#39;host AEM Publish, pertanto è necessaria la configurazione CORS.

Durante il test dell&#39;autenticazione SAML sul SDK AEM locale (`localhost:4503`), l&#39;IDP può impostare l&#39;intestazione `Origin` su `null`. In tal caso, aggiungere `"null"` all&#39;elenco `alloworigin`.

1. Crea un file di configurazione OSGi nel progetto in `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Cambia `/wknd-examples/` con il nome del progetto
   + L&#39;identificatore dopo `~` nel nome file deve identificare in modo univoco questa configurazione, quindi potrebbe essere il nome dell&#39;IDP, ad esempio `...CORSPolicyImpl~okta.cfg.json`. Il valore deve essere alfanumerico con trattini.
1. Incolla il seguente JSON nel file `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json`.

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

Le configurazioni OSGi per ambiente (`config.publish.dev`, `config.publish.stage` e `config.publish.prod`) possono essere definite con attributi specifici se `alloworigin` e `allowedpaths` variano da un ambiente all&#39;altro.

## Configura AEM Dispatcher per consentire SAML HTTP POST

Dopo aver eseguito correttamente l&#39;autenticazione nell&#39;IDP, l&#39;IDP orchestrerà un POST HTTP per tornare all&#39;endpoint `/saml_login` registrato di AEM (configurato nell&#39;IDP). Questo HTTP POST a `/saml_login` è bloccato per impostazione predefinita in Dispatcher, pertanto deve essere consentito esplicitamente utilizzando la seguente regola di Dispatcher:

1. Apri `dispatcher/src/conf.dispatcher.d/filters/filters.any` nell&#39;IDE.
1. Aggiungi alla fine del file un regola consenti per HTTP POST agli URL che terminano con `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>Quando distribuisci più configurazioni SAML in AEM per vari percorsi protetti ed endpoint IDP distinti, assicurati che l&#39;IDP indirizzi all&#39;endpoint RESPECTIVE_PROTECTED_PATH/saml_login per selezionare la configurazione SAML appropriata sul lato AEM. Se esistono configurazioni SAML duplicate per lo stesso percorso protetto, la selezione della configurazione SAML avverrà in modo casuale.

Se URL riscrittura sul server Web Apache è configurato (`dispatcher/src/conf.d/rewrites/rewrite.rules`), assicurarsi che le `.../saml_login` richieste agli endpoint non vengano accidentalmente alterate.

## Iscrizione a gruppi dinamici

L&#39;iscrizione dinamica ai gruppi è una funzionalità di [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) che migliora le prestazioni di valutazione e provisioning dei gruppi. Questa sezione descrive come vengono archiviati utenti e gruppi quando questa funzione è abilitata e come modificare la configurazione di SAML Authentication Handler per abilitarlo per ambienti nuovi o esistenti.

### Come abilitare l&#39;iscrizione dinamica a un gruppo per gli utenti SAML in nuovi ambienti

Per migliorare significativamente le prestazioni di valutazione dei gruppi nei nuovi ambienti AEM come Cloud Service, è consigliabile attivare la funzionalità Appartenenza dinamica ai gruppi nei nuovi ambienti.
Questo è anche un passaggio necessario quando viene attivata la sincronizzazione dei dati. Maggiori dettagli [qui](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier) .
A tale scopo, aggiungere la seguente proprietà al file di configurazione OSGI:

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Con questa configurazione, gli utenti e i gruppi vengono creati come [Oak utenti](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html) esterni. In AEM, gli utenti e i gruppi esterni hanno un&#39;impostazione predefinita `rep:principalName` composta da `[user name];[idp]` o `[group name];[idp]`.
Si noti che gli elenchi di controllo di accesso (ACL) sono associati al PrincipalName di utenti o gruppi.
Quando si distribuisce questa configurazione in una distribuzione esistente in cui in precedenza `identitySyncType` non era specificato o impostato su `default`, verranno creati nuovi utenti e gruppi e ACL deve essere applicato a questi nuovi utenti e gruppi. Si noti che i gruppi esterni non possono contenere utenti locali. [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html) può essere utilizzato per creare ACL per gruppi esterni SAML lineare se verranno creati solo quando il utente eseguirà un login.
Per evitare questo refactoring su ACL, è stata implementata una funzionalità[ di migrazione standard](#automatic-migration-to-dynamic-group-membership-for-existing-environments).

### Come vengono memorizzate le appartenenze in gruppi locali ed esterni con iscrizione al gruppo dinamico

Nei gruppi locali i membri del gruppo sono memorizzati nell&#39;attributo oak: `rep:members`. L&#39;attributo contiene l&#39;elenco degli uid di ogni membro del gruppo. Ulteriori dettagli sono disponibili [qui](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository).
Esempio:

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

I gruppi esterni con iscrizione al gruppo dinamici non store alcun membro nella voce del gruppo.
Il iscrizione al gruppo viene invece memorizzato nelle voci degli utenti. La documentazione aggiuntiva è disponibile [qui](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). Per esempio, questo è il nodo OAK per il gruppo:

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

Questo è il nodo di un utente membro del gruppo:

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### Abilitare l’iscrizione al gruppo dinamico per gli utenti SAML negli ambienti esistenti

Come spiegato nella sezione precedente, il formato degli utenti e dei gruppi esterni è leggermente diverso da quello utilizzato per gli utenti e i gruppi locali. È possibile definire un nuovo ACL per i gruppi esterni ed eseguire il provisioning di nuovi utenti esterni, oppure utilizzare lo strumento di migrazione come descritto di seguito.

#### Abilitazione dell’iscrizione dinamica ai gruppi per gli ambienti esistenti con utenti esterni

Il gestore di Authentication SAML crea utenti esterni quando viene specificata la seguente proprietà: `"identitySyncType": "idp"`. In questo caso, è possibile abilitare iscrizione al gruppo dinamici modificando questa proprietà in: `"identitySyncType": "idp_dynamic"` Non è richiesta alcuna migrazione.

#### Migrazione automatica a iscrizione al gruppo dinamici per ambienti esistenti con utenti locali

Il gestore di Authentication SAML crea utenti locali quando viene specificata la seguente proprietà: `"identitySyncType": "default"` Questo è anche il valore predefinito quando la proprietà non è specificata. In questa sezione descriviamo i passaggi eseguiti dalla procedura di migrazione automatica.

Quando questa migrazione è abilitata, viene eseguita durante l’autenticazione dell’utente e consiste dei seguenti passaggi:
1. La utente locale viene migrata in un utente esterno mantenendo il nome utente originale. Ciò implica che gli utenti locali migrati, che ora agiscono come utenti esterni, mantengano il loro nome utente originale invece di seguire la sintassi di denominazione menzionata nella sezione precedente. Verrà aggiunta un&#39;ulteriore proprietà chiamata: `rep:externalId` con il valore di `[user name];[idp]`. Il utente `PrincipalName` non viene modificato.
2. Per ogni gruppo esterno ricevuto nell&#39;asserzione SAML, viene creato un gruppo esterno. Se esiste un gruppo locale corrispondente, il gruppo esterno viene aggiunto al gruppo locale come membro.
3. L&#39;utente viene aggiunto come membro del gruppo esterno.
4. Il utente locale viene quindi rimosso da tutti i gruppi locali Saml di cui era membro. I gruppi locali Saml sono identificati dalla proprietà OAK: `rep:managedByIdp`. Questa proprietà viene impostata dal gestore Authentication Saml quando l&#39;attributo `syncType` non è specificato o impostato su `default`.

Ad istanza, se prima della migrazione `user1` c&#39;è un utente locale e un membro di un gruppo `group1`locale, dopo la migrazione si verificheranno le seguenti modifiche:
`user1` diventa un utente esterno. L&#39;attributo `rep:externalId` viene aggiunto al suo profilo.
`user1`diventa membro di un gruppo esterno: `group1;idp`non è più un membro diretto del gruppo locale: `user1``group1` è un membro del gruppo locale: `group1;idp`.`group1`

`user1` è quindi un membro del gruppo locale: `group1` sebbene l&#39;ereditarietà

L&#39;appartenenza al gruppo per i gruppi esterni è archiviata nel profilo utente nella proprietà `rep:externalPrincipalNames`

### Come configurare la migrazione automatica all&#39;appartenenza a un gruppo dinamico

1. Abilitare la proprietà `"identitySyncType": "idp_dynamic_simplified_id"` nel file di configurazione OSGi SAML: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`:
2. Configurare il nuovo servizio OSGi con PID di fabbrica che inizia con: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`. Ad esempio, un PID può essere: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`. Imposta la seguente proprietà:

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

Per migrare più configurazioni SAML, è necessario creare più configurazioni OSGi factory per `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration`, ognuna delle quali specifica un `idpIdentifier` per la migrazione.

## Distribuzione della configurazione SAML

Le configurazioni OSGi devono essere salvate in Git e distribuite in AEM as a Cloud Service utilizzando Cloud Manager.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Distribuire il ramo Git Cloud Manager di destinazione (in questo esempio, `develop`) utilizzando una pipeline di distribuzione full stack.

## Richiamare l’autenticazione SAML

Il flusso di autenticazione SAML può essere richiamato da una pagina web del sito AEM creando un collegamento creato appositamente o un pulsante. I parametri descritti di seguito possono essere impostati a livello di programmazione in base alle esigenze. Ad esempio, un pulsante di accesso può impostare `saml_request_path`, ovvero il punto in cui l&#39;utente viene indirizzato dopo l&#39;autenticazione SAML, su pagine AEM diverse, in base al contesto del pulsante.

## Memorizzazione in cache protetta durante l’utilizzo di SAML

Nell’istanza di pubblicazione di AEM, la maggior parte delle pagine viene generalmente memorizzata nella cache. Tuttavia, per i percorsi protetti da SAML, la memorizzazione nella cache deve essere disabilitata o abilitata utilizzando la configurazione auth_checker. Per ulteriori informazioni, fare riferimento ai dettagli forniti [qui](https://experienceleague.adobe.com/it/docs/experience-manager-dispatcher/using/configuring/permissions-cache)

Tieni presente che se memorizzi nella cache i percorsi protetti senza abilitare auth_checker, potresti riscontrare un comportamento imprevedibile.

### richiesta GET

L’autenticazione SAML può essere richiamata creando una richiesta HTTP GET nel formato:

`HTTP GET /system/sling/login`

e fornendo parametri di query:

| Nome parametro query | Valore parametro query |
|----------------------|-----------------------|
| `resource` | Qualsiasi percorso JCR, o sottopercorso, che è il gestore di autenticazione SAML è in ascolto, come definito nella proprietà della configurazione[ ](#configure-saml-2-0-authentication-handler) OSGi del `path`gestore di Authentication Adobe Systems Granite SAML 2.0 Granite. |
| `saml_request_path` | Percorso URL a cui deve essere seguito il utente dopo aver eseguito correttamente l&#39;autenticazione SAML. |

Ad esempio, questo collegare HTML attiverà il flusso di log in SAML e, in caso di esito positivo, porterà il utente a `/content/wknd/us/en/protected/page.html`. Questi parametri di query possono essere impostati in modo programmaticale in base alle esigenze.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## richiesta POST

L&#39;autenticazione SAML può essere richiamata creando un richiesta HTTP POST nel formato:

`HTTP POST /system/sling/login`

e fornendo i dati del modulo:

| Nome dati modulo | Valore dei dati del modulo |
|----------------------|-----------------------|
| `resource` | Qualsiasi percorso JCR, o sottopercorso, che è il gestore di autenticazione SAML è in ascolto, come definito nella proprietà della configurazione[ ](#configure-saml-2-0-authentication-handler) OSGi del `path`gestore di Authentication Adobe Systems Granite SAML 2.0 Granite. |
| `saml_request_path` | Percorso URL a cui deve essere seguito il utente dopo aver eseguito correttamente l&#39;autenticazione SAML. |


Ad esempio, questo pulsante HTML utilizzerà un POST HTTP per attivare il flusso di log in SAML e, in caso di esito positivo, porterà il utente a `/content/wknd/us/en/protected/page.html`. Questi parametri dei dati del modulo possono essere impostati in base alle esigenze.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Configurazione del Dispatcher

Sia il GET HTTP che i metodi POST richiedono accesso client agli endpoint di AEM e pertanto devono essere consentiti `/system/sling/login` tramite AEM Dispatcher.

Consenti i pattern di URL necessari in base all&#39;utilizzo di GET o POST

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
