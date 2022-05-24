---
title: SAML 2.0 su AEM as a Cloud Service
description: Scopri come configurare l’autenticazione SAML 2.0 su AEM servizio Pubblicazione as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: 343040.jpeg
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
source-git-commit: f2b5adea71ec8e772385b6e0ae068369798030e0
workflow-type: tm+mt
source-wordcount: '2774'
ht-degree: 2%

---

# Autenticazione SAML 2.0{#saml-2-0-authentication}

Scopri come configurare e autenticare gli utenti finali (non AEM gli autori) in un IDP compatibile SAML 2.0 a tua scelta.

## Quale SAML per AEM as a Cloud Service?

L’integrazione SAML 2.0 con AEM Publish (o Preview) consente agli utenti finali di un’esperienza web AEM di eseguire l’autenticazione in un IDP non Adobe (provider di identità) e di accedere AEM come utente autorizzato e denominato.

|  | Autore AEM | AEM Publish |
|-----------------------|:----------:|:-----------:|
| Supporto SAML 2.0 | ✘ | ↓ |

+++ Comprendere il flusso SAML 2.0 con AEM

Il flusso tipico di un’integrazione AEM Publish SAML è il seguente:

1. L&#39;utente effettua una richiesta ad AEM Publish the indica che l&#39;autenticazione è necessaria.
   + L&#39;utente richiede una risorsa protetta CUG/ACL.
   + L’utente richiede una risorsa soggetta a un requisito di autenticazione.
   + L’utente segue un collegamento AEM endpoint di accesso (ad es. `/system/sling/login`) che richiede esplicitamente l’azione di accesso.
1. AEM invia una richiesta AuthnRequest all’IDP, richiedendo all’IDP di avviare il processo di autenticazione.
1. L&#39;utente si autentica nell&#39;IDP.
   + Viene richiesto all’utente dall’IDP di specificare le credenziali.
   + L&#39;utente è già autenticato con l&#39;IDP e non deve fornire ulteriori credenziali.
1. L’IDP genera un’asserzione SAML contenente i dati dell’utente e la firma utilizzando il certificato privato dell’IDP.
1. IDP invia l’asserzione SAML tramite HTTP POST, tramite il browser web dell’utente, ad AEM Publish.
1. AEM Publish riceve l&#39;asserzione SAML e convalida l&#39;integrità e l&#39;autenticità dell&#39;asserzione SAML utilizzando il certificato pubblico IDP.
1. AEM Publish gestisce il record utente AEM in base alla configurazione SAML 2.0 OSGi e al contenuto dell’asserzione SAML.
   + Crea utente
   + Sincronizza gli attributi utente
   + Aggiornamenti AEM&#39;iscrizione al gruppo utenti
1. AEM Publish imposta il AEM `login-token` cookie sulla risposta HTTP, che viene utilizzato per autenticare le richieste successive ad AEM Publish.
1. AEM Publish reindirizza l’utente all’URL su AEM Publish come specificato da `saml_request_path` cookie.

+++

## Procedura dettagliata di configurazione

>[!VIDEO](https://video.tv.adobe.com/v/343040/?quality=12&learn=on)

Questo video illustra come configurare l&#39;integrazione SAML 2.0 con AEM servizio di pubblicazione as a Cloud Service e come utilizzare Okta come IDP.

## Prerequisiti

Quando si configura l’autenticazione SAML 2.0, sono necessari i seguenti requisiti:

+ Accesso a Cloud Manager da Deployment Manager
+ Accesso AEM amministratore all’ambiente as a Cloud Service AEM
+ Accesso dell&#39;amministratore all&#39;IDP
+ Facoltativamente, l&#39;accesso a una coppia di chiavi pubblica/privata utilizzata per crittografare i payload SAML

SAML 2.0 è supportato solo per l’autenticazione e utilizza per AEM Publish o Preview. Per gestire l’autenticazione di AEM Author tramite e IDP, [integrare l’IDP con Adobe IMS](https://helpx.adobe.com/it/enterprise/using/set-up-identity.html).


## Installa il certificato pubblico IDP su AEM

Il certificato pubblico dell&#39;IDP viene aggiunto AEM&#39;archivio attendibilità globale e utilizzato per convalidare l&#39;asserzione SAML inviata dall&#39;IDP è valida.

Flusso di firma dell’asserzione +++SAML

![SAML 2.0 - Firma dell&#39;asserzione SAML IDP](./assets/saml-2-0/idp-signing-diagram.png)

1. L&#39;utente si autentica nell&#39;IDP.
1. L’IDP genera un’asserzione SAML contenente i dati dell’utente.
1. IDP firma l’asserzione SAML utilizzando il certificato privato dell’IDP.
1. IDP avvia un POST HTTP lato client all’endpoint SAML di AEM Publish (`.../saml_login`) che include l&#39;asserzione SAML firmata.
1. AEM Publish riceve il POST HTTP contenente l’asserzione SAML firmata, che può convalidare la firma utilizzando il certificato pubblico IDP.

+++

![Aggiungi il certificato pubblico IDP all’archivio attendibilità globale](./assets/saml-2-0/global-trust-store.png)

1. Ottieni il __certificato pubblico__ file dall&#39;IDP. Questo certificato consente AEM convalidare l’asserzione SAML fornita a AEM dall’IDP.

   Il certificato è in formato PEM e deve essere simile a:

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Accedi ad AEM Author come amministratore AEM.
1. Passa a __Strumenti > Protezione > Archivio fonti attendibili__.
1. Crea o apri l&#39;archivio globale dei trust. Se crei un Global Trust Store, memorizza la password in un posto sicuro.
1. Espandi __Aggiungi certificato dal file CER__.
1. Seleziona __Seleziona file di certificato__ e carica il file del certificato fornito dall’IDP.
1. Esci __Mappa certificato all’utente__ vuoto.
1. Seleziona __Invia__.
1. Il certificato appena aggiunto viene visualizzato sopra il __Aggiungi certificato dal file CRT__ sezione .
1. Prendi nota del __alias__, poiché questo valore viene utilizzato nella variabile [Configurazione OSGi del gestore di autenticazione SAML 2.0](#saml-2-0-authentication-handler-osgi-configuration).
1. Seleziona __Salva e chiudi__.

L&#39;archivio locale globale è configurato con il certificato pubblico dell&#39;IDP su AEM Author, ma poiché SAML è utilizzato solo su AEM Publish, l&#39;archivio locale globale deve essere replicato in AEM Publish affinché il certificato pubblico IDP sia accessibile in tale ambiente.

![Replicare l&#39;archivio di attendibilità globale su AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. Passa a __Strumenti > Implementazione > Pacchetti__.
1. Creare un pacchetto
   + Nome pacchetto: `Global Trust Store`
   + Versione: `1.0.0`
   + Gruppo: `com.your.company`
1. Modificare il nuovo __Archivio fonti attendibili globale__ pacchetto.
1. Seleziona la __Filtri__ e aggiungere un filtro per il percorso principale `/etc/truststore`.
1. Seleziona __Fine__ e poi __Salva__.
1. Seleziona la __Crea__ per __Archivio fonti attendibili globale__ pacchetto.
1. Una volta generato, seleziona __Altro__ > __Replicare__ per attivare il nodo Global Trust Store (`/etc/truststore`) in AEM Publish.

## Installa AEM coppia di chiavi pubblica/privata{#install-aem-public-private-key-pair}

_L’installazione della coppia di chiavi pubblica/privata AEM è facoltativa_

AEM Publish può essere configurato per firmare AuthnRequests (su IDP) e crittografare le asserzioni SAML (su AEM). Questo si ottiene fornendo una chiave privata ad AEM Publish, e corrisponde alla chiave pubblica all&#39;IDP.

+++ Comprendere il flusso di firma AuthnRequest (facoltativo)

La richiesta AuthnRequest (la richiesta all’IDP da AEM Publish che avvia il processo di accesso) può essere firmata da AEM Publish. A questo scopo, AEM Publish firma AuthnRequest utilizzando la chiave privata, affinché l’IDP convalidi la firma utilizzando la chiave pubblica. Questo garantisce all’IDP che AuthnRequest è stato avviato e richiesto da AEM Publish e non da una terza parte malintenzionata.

![SAML 2.0 - Firma di SP AuthnRequest](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. L&#39;utente effettua una richiesta HTTP ad AEM Publish che si traduce in una richiesta di autenticazione SAML all&#39;IDP.
1. AEM Publish genera la richiesta SAML da inviare all’IDP.
1. AEM Publish firma la richiesta SAML utilizzando AEM chiave privata.
1. AEM Publish avvia AuthnRequest, un reindirizzamento HTTP lato client all’IDP che contiene la richiesta SAML firmata.
1. IDP riceve la Richiesta di autenticazione e convalida la firma utilizzando AEM chiave pubblica, garantendo che AEM Publish abbia avviato la Richiesta di autenticazione.
1. AEM Publish quindi convalida l&#39;integrità e l&#39;autenticità dell&#39;asserzione SAML decifrata utilizzando il certificato pubblico IDP.

+++

+++ Comprendere il flusso di crittografia dell’asserzione SAML (facoltativo)

Tutte le comunicazioni HTTP tra IDP e AEM Publish devono essere su HTTPS, e quindi protette per impostazione predefinita. Tuttavia, come richiesto, le asserzioni SAML possono essere crittografate nel caso in cui sia necessaria un&#39;ulteriore riservatezza oltre a quella fornita da HTTPS. A questo scopo, l’IDP crittografa i dati dell’asserzione SAML utilizzando la chiave privata e AEM Publish decrittografa l’asserzione SAML utilizzando la chiave privata.

![SAML 2.0 - Crittografia SAML Assertion SP](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. L&#39;utente si autentica nell&#39;IDP.
1. L’IDP genera un’asserzione SAML contenente i dati dell’utente e la firma utilizzando il certificato privato dell’IDP.
1. IDP crittografa quindi l&#39;asserzione SAML con AEM chiave pubblica, che richiede la chiave privata AEM per decrittografare.
1. L’asserzione SAML crittografata viene inviata tramite il browser web dell’utente ad AEM Publish.
1. AEM Publish riceve l&#39;asserzione SAML e la decrittografa utilizzando AEM chiave privata.
1. IDP richiede all&#39;utente di effettuare l&#39;autenticazione.

+++

La firma AuthnRequest e la crittografia dell’asserzione SAML sono facoltative, ma sono entrambe abilitate, utilizzando la variabile [Proprietà di configurazione OSGi del gestore di autenticazione SAML 2.0 `useEncryption`](#saml-20-authenticationsaml-2-0-authentication), ovvero è possibile utilizzare entrambi o nessuno dei due.

![AEM archivio chiavi del servizio di autenticazione](./assets/saml-2-0/authentication-service-key-store.png)

1. Recuperare la chiave pubblica, la chiave privata (PKCS#8 in formato DER) e il file della catena di certificati (questa può essere la chiave pubblica) utilizzati per firmare la Richiesta di autenticazione e crittografare l&#39;asserzione SAML. Le chiavi vengono in genere fornite dal team di sicurezza dell&#39;organizzazione IT.

   + È possibile generare una coppia di chiavi autofirmata utilizzando __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Carica la chiave pubblica nell’IDP.
   + Utilizzo della `openssl` metodo precedente, la chiave pubblica è `aem-public.crt` file.
1. Accedi ad AEM Author come amministratore AEM per caricare la chiave privata.
1. Passa a __Strumenti > Protezione > Archivio fonti attendibili__, quindi seleziona __servizio di autenticazione__ utente e seleziona __Proprietà__ dalla barra delle azioni superiore.
1. Passa a __Strumenti > Protezione > Utenti__, quindi seleziona __servizio di autenticazione__ utente e seleziona __Proprietà__ dalla barra delle azioni superiore.
1. Seleziona la __Keystore__ scheda .
1. Crea o apri il keystore. Se crei un keystore, mantieni la password al sicuro.
1. Seleziona __Aggiungi chiave privata dal file DER__ e aggiungi la chiave privata e il file della catena a AEM:
   + __Alias__: Fornisci un nome significativo, spesso il nome dell’IDP.
   + __File di chiave privata__: Carica il file della chiave privata (PKCS#8 in formato DER).
      + Utilizzo della `openssl` metodo precedente, è il `aem-private-pkcs8.der` file
   + __Seleziona il file della catena di certificati__: Carica il file della catena che lo accompagna (potrebbe essere la chiave pubblica).
      + Utilizzo della `openssl` metodo precedente, è il `aem-public.crt` file
   + Seleziona __Invia__
1. Il certificato appena aggiunto viene visualizzato sopra il __Aggiungi certificato dal file CRT__ sezione .
   + Prendi nota del __alias__ come viene utilizzato nella [Configurazione OSGi del gestore di autenticazione SAML 2.0](#saml-20-authentication-handler-osgi-configuration)
1. Seleziona __Salva e chiudi__.
1. Seleziona __servizio di autenticazione__ utente e seleziona __Attiva__ dalla barra delle azioni superiore.

## Configurare il gestore di autenticazione SAML 2.0{#configure-saml-2-0-authentication-handler}

AEM la configurazione SAML viene eseguita tramite __Gestore autenticazione Adobe Granite SAML 2.0__ Configurazione OSGi.
La configurazione è una configurazione di fabbrica OSGi, il che significa che un singolo servizio di pubblicazione as a Cloud Service AEM può avere più configurazioni SAML che coprono le strutture di risorse discrete dell&#39;archivio; questa funzione è utile per le distribuzioni di AEM multisito.

+++ Glossario di configurazione OSGi del gestore di autenticazione SAML 2.0

### Configurazione OSGi del gestore dell&#39;autenticazione Adobe Granite SAML 2.0{#configure-saml-2-0-authentication-handler-osgi-configuration}

|  | OSGi, proprietà | Obbligatorio | Formato del valore | Valore predefinito | Descrizione |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Percorsi | `path` | ↓ | Matrice di stringhe | `/` | AEM percorsi per i quali viene utilizzato questo gestore di autenticazione. |
| URL IDP | `idpUrl` | ↓ | Stringa |  | URL IDP viene inviata la richiesta di autenticazione SAML. |
| Alias del certificato IDP | `idpCertAlias` | ↓ | Stringa |  | L&#39;alias del certificato IDP trovato nell&#39;AEM Global Trust Store |
| Reindirizzamento HTTP IDP | `idpHttpRedirect` | ✘ | Booleano | `false` | Indica se un Reindirizzamento HTTP all&#39;URL IDP anziché inviare una AuthnRequest. Imposta su `true` per l&#39;autenticazione avviata da IDP. |
| Identificatore IDP | `idpIdentifier` | ✘ | Stringa |  | Id IDP univoco per garantire AEM&#39;univocità dell&#39;utente e del gruppo. Se vuoto, il `serviceProviderEntityId` viene invece utilizzato. |
| URL del servizio consumer di asserzione | `assertionConsumerServiceURL` | ✘ | Stringa |  | La `AssertionConsumerServiceURL` Attributo URL in AuthnRequest che specifica dove si trova la variabile `<Response>` Il messaggio deve essere inviato a AEM. |
| ID entità SP | `serviceProviderEntityId` | ↓ | Stringa |  | Identifica in maniera univoca AEM all&#39;IDP; in genere il nome host AEM. |
| Crittografia SP | `useEncryption` | ✘ | Booleano | `true` | Indica se l&#39;IDP crittografa le asserzioni SAML. Richiede `spPrivateKeyAlias` e `keyStorePassword` da impostare. |
| Alias chiave privata SP | `spPrivateKeyAlias` | ✘ | Stringa |  | L&#39;alias della chiave privata nel `authentication-service` l&#39;archivio chiavi dell&#39;utente. Obbligatorio se `useEncryption` è impostato su `true`. |
| Password dell&#39;archivio chiavi SP | `keyStorePassword` | ✘ | Stringa |  | La password dell&#39;archivio chiavi dell&#39;utente del servizio di autenticazione. Obbligatorio se `useEncryption` è impostato su `true`. |
| Reindirizzamento predefinito | `defaultRedirectUrl` | ✘ | Stringa | `/` | L&#39;URL di reindirizzamento predefinito dopo l&#39;autenticazione. Può essere relativo all’host AEM (ad esempio, `/content/wknd/us/en/html`). |
| Attributo ID utente | `userIDAttribute` | ✘ | Stringa | `uid` | Nome dell&#39;attributo di asserzione SAML contenente l&#39;ID utente dell&#39;utente AEM. Lascia vuoto per utilizzare il `Subject:NameId`. |
| Creazione automatica AEM utenti | `createUser` | ✘ | Booleano | `true` | Indica se AEM utenti vengono creati in seguito a un&#39;autenticazione corretta. |
| AEM percorso intermedio utente | `userIntermediatePath` | ✘ | Stringa |  | Durante la creazione di utenti AEM, questo valore viene utilizzato come percorso intermedio (ad esempio, `/home/users/<userIntermediatePath>/jane@wknd.com`). Richiede `createUser` da impostare `true`. |
| Attributi utente AEM | `synchronizeAttributes` | ✘ | Matrice di stringhe |  | Elenco delle mappature degli attributi SAML da memorizzare sull&#39;utente AEM, nel formato `[ "saml-attribute-name=path/relative/to/user/node" ]` (ad esempio, `[ "firstName=profile/givenName" ]`). Consulta la sezione [elenco completo degli attributi AEM nativi](#aem-user-attributes). |
| Aggiungi utente a gruppi AEM | `addGroupMemberships` | ✘ | Booleano | `true` | Indica se un utente AEM viene aggiunto automaticamente a AEM gruppi di utenti dopo l&#39;autenticazione riuscita. |
| Attributo di appartenenza al gruppo AEM | `groupMembershipAttribute` | ✘ | Stringa | `groupMembership` | Nome dell&#39;attributo di asserzione SAML contenente un elenco di gruppi di utenti AEM cui l&#39;utente deve essere aggiunto. Richiede `addGroupMemberships` da impostare `true`. |
| Gruppi AEM predefiniti | `defaultGroups` | ✘ | Matrice di stringhe |  | Un elenco AEM gruppi di utenti a cui vengono sempre aggiunti gli utenti autenticati (ad esempio, `[ "wknd-user" ]`). Richiede `addGroupMemberships` da impostare `true`. |
| Formato NameIDPolicy | `nameIdFormat` | ✘ | Stringa | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | Il valore del parametro del formato NameIDPolicy da inviare nel messaggio AuthnRequest. |
| Archiviare la risposta SAML | `storeSAMLResponse` | ✘ | Booleano | `false` | Indica se la `samlResponse` viene memorizzato nel AEM `cq:User` nodo. |
| Gestisci logout | `handleLogout` | ✘ | Booleano | `false` | Indica se la richiesta di logout è gestita da questo gestore di autenticazione SAML. Richiede `logoutUrl` da impostare. |
| URL di disconnessione | `logoutUrl` | ✘ | Stringa |  | URL dell’IDP a cui viene inviata la richiesta di logout SAML. Obbligatorio se `handleLogout` è impostato su `true`. |
| Tolleranza all&#39;orologio | `clockTolerance` | ✘ | Numero intero | `60` | Tolleranza di inclinazione dell&#39;orologio IDP e AEM (SP) durante la convalida delle asserzioni SAML. |
| Metodo digest | `digestMethod` | ✘ | Stringa | `http://www.w3.org/2001/04/xmlenc#sha256` | L’algoritmo digest utilizzato dall’IDP per firmare un messaggio SAML. |
| Metodo della firma | `signatureMethod` | ✘ | Stringa | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | L’algoritmo di firma utilizzato dall’IDP per firmare un messaggio SAML. |
| Tipo di sincronizzazione identità | `identitySyncType` | ✘ | `default` oppure `idp` | `default` | Non modificare `from` impostazione predefinita per AEM as a Cloud Service. |
| Classificazione del servizio | `service.ranking` | ✘ | Numero intero | `5002` | Configurazioni di classificazione più elevate sono preferite per le stesse `path`. |

### Attributi utente AEM{#aem-user-attributes}

AEM utilizza i seguenti attributi utente, che possono essere compilati tramite il `synchronizeAttributes` nella configurazione OSGi del gestore dell&#39;autenticazione Adobe Granite SAML 2.0.  Tutti gli attributi IDP possono essere sincronizzati con qualsiasi proprietà utente AEM, tuttavia la mappatura per AEM utilizzare le proprietà degli attributi (elencate di seguito) consente AEM utilizzarli naturalmente.

| Attributo utente | Percorso proprietà relativo da `rep:User` nodo |
|--------------------------------|--------------------------|
| Titolo (ad esempio, `Mrs`) | `profile/title` |
| Nome assegnato (ad esempio nome) | `profile/givenName` |
| Cognome (cognome) | `profile/familyName` |
| Titolo del processo | `profile/jobTitle` |
| Indirizzo e-mail | `profile/email` |
| Indirizzo | `profile/street` |
| Città | `profile/city` |
| Codice postale | `profile/postalCode` |
| Paese | `profile/country` |
| Numero di telefono | `profile/phoneNumber` |
| Informazioni su di me | `profile/aboutMe` |

+++

1. Crea un file di configurazione OSGi nel tuo progetto in `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` e si apre nell&#39;IDE.
   + Modifica `/wknd-examples/` al tuo `/<project name>/`
   + L&#39;identificatore dopo `~` nel nome file deve identificare in modo univoco questa configurazione, quindi può essere il nome dell&#39;IDP, come `...~okta.cfg.json`. Il valore deve essere alfanumerico con trattini.
1. Incolla il seguente JSON nel `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` e aggiorna il `wknd` i riferimenti necessari.

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

1. Aggiorna i valori come richiesto dal progetto. Consulta la sezione __Glossario di configurazione OSGi del gestore di autenticazione SAML 2.0__ qui sopra per le descrizioni delle proprietà di configurazione
1. Si consiglia, ma non è necessario, di utilizzare le variabili e i segreti di ambiente OSGi, quando i valori possono cambiare fuori sincronia con il ciclo di rilascio, o quando i valori differiscono tra tipi di ambiente/livelli di servizio simili. I valori predefiniti possono essere impostati utilizzando `$[env:..;default=the-default-value]"` come mostrato sopra.

Configurazioni OSGi per ambiente (`config.publish.dev`, `config.publish.stage`e `config.publish.prod`) può essere definita con attributi specifici se la configurazione SAML varia da un ambiente all’altro.

### Usa crittografia

Quando [crittografia dell&#39;asserzione AuthnRequest e SAML](#encrypting-the-authnrequest-and-saml-assertion), sono necessarie le seguenti proprietà: `useEncryption`, `spPrivateKeyAlias`e `keyStorePassword`. La `keyStorePassword` contiene una password, pertanto il valore non deve essere memorizzato nel file di configurazione OSGi, ma viene inserito utilizzando [valori di configurazione segreti](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++Facoltativamente, aggiorna la configurazione OSGi per utilizzare la crittografia

1. Apri `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` nell’IDE.
1. Aggiungi le tre proprietà `useEncryption`, `spPrivateKeyAlias`e `keyStorePassword` come mostrato di seguito.

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

1. Le tre proprietà di configurazione OSGi necessarie per la crittografia sono:

+ `useEncryption` impostato su `true`
+ `spPrivateKeyAlias` contiene l&#39;alias della voce keystore per la chiave privata utilizzata dall&#39;integrazione SAML.
+ `keyStorePassword` contiene un [Variabile di configurazione segreta OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) contenente `authentication-service` password dell&#39;archivio chiavi utente.

+++

## Configura filtro referente

Durante il processo di autenticazione SAML, l’IDP avvia un POST HTTP lato client su AEM Publish’s `.../saml_login` punto finale. Se l’IDP e AEM Publish esistono su un’origine diversa, AEM Publish’s __Filtro di riferimento__ è configurato tramite la configurazione OSGi per consentire i POST HTTP dall’origine dell’IDP.

1. Crea (o modifica) un file di configurazione OSGi nel tuo progetto in `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Modifica `/wknd-examples/` al tuo `/<project name>/`
1. Assicurati che `allow.empty` è impostato su `true`, `allow.hosts` (o se preferisci, `allow.hosts.regexp`) contiene l&#39;origine dell&#39;IDP, e `filter.methods` include `POST`. La configurazione OSGi deve essere simile a:

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

AEM Publish supporta una singola configurazione di filtro referente, quindi unisci i requisiti di configurazione SAML con eventuali configurazioni esistenti.

Configurazioni OSGi per ambiente (`config.publish.dev`, `config.publish.stage`e `config.publish.prod`) può essere definita con attributi specifici se `allow.hosts` o `allow.hosts.regex`) varia in base agli ambienti.

## Configurare la condivisione risorse tra le origini (CORS, Cross-Origin Resource Sharing)

Durante il processo di autenticazione SAML, l’IDP avvia un POST HTTP lato client su AEM Publish’s `.../saml_login` punto finale. Se l’IDP e AEM Publish esistono su host/domini diversi, AEM Publish’s __Condivisione risorse CRoss-Origin (CORS)__ deve essere configurato per consentire i POST HTTP dall&#39;host/dominio dell&#39;IDP.

Questa richiesta HTTP POST è `Origin` L’intestazione ha in genere un valore diverso dall’host AEM Publish , che richiede quindi la configurazione CORS.

Durante il test dell&#39;autenticazione SAML sull&#39;SDK AEM locale (`localhost:4503`), l&#39;IDP può impostare il `Origin` intestazione `null`. In tal caso, aggiungi `"null"` al `alloworigin` elenco.

1. Crea un file di configurazione OSGi nel tuo progetto in `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Modifica `/wknd-examples/` al nome del progetto
   + L&#39;identificatore dopo `~` nel nome file deve identificare in modo univoco questa configurazione, quindi può essere il nome dell&#39;IDP, come `...CORSPolicyImpl~okta.cfg.json`. Il valore deve essere alfanumerico con trattini.
1. Incolla il seguente JSON nel `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` file.

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

Configurazioni OSGi per ambiente (`config.publish.dev`, `config.publish.stage`e `config.publish.prod`) può essere definita con attributi specifici se `alloworigin` e `allowedpaths` varia da un ambiente all&#39;altro.

## Configurare AEM Dispatcher per consentire i POST HTTP SAML

Dopo l’autenticazione all’IDP, l’IDP orchestrerà un POST HTTP per AEM registrato `/saml_login` end point (configurato nell&#39;IDP). Questo POST HTTP a `/saml_login` è bloccato per impostazione predefinita in Dispatcher, pertanto deve essere consentito esplicitamente utilizzando la seguente regola di Dispatcher:

1. Apri `dispatcher/src/conf.dispatcher.d/filters/filters.any` nell’IDE.
1. Aggiungi in fondo al file , una regola allow per i POST HTTP agli URL che terminano con `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

Se è configurata la riscrittura URL sul server web Apache (`dispatcher/src/conf.d/rewrites/rewrite.rules`), assicurati che le richieste `.../saml_login` i punti finali non vengono accidentalmente gestiti.

## Abilita sincronizzazione dati

I record utente devono essere sincronizzati nel livello di pubblicazione AEM, una volta che il flusso di autenticazione SAML crea un utente in AEM Publish. A [abilitare la sincronizzazione dati](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#data-synchronization), invia una richiesta all’Assistenza clienti di Adobe (tramite [AdminConsole](https://adminconsole.adobe.com) > Supporto) per richiedere l&#39;abilitazione.

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

Distribuire il ramo Git di target Cloud Manager (in questo esempio, `develop`), utilizzando una pipeline di distribuzione Stack completo.
