---
title: Credenziali del servizio
description: AEM Credenziali di servizio sono utilizzate per facilitare l'interazione programmatica di applicazioni, sistemi e servizi esterni con i servizi AEM Author o Publish via HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
translation-type: tm+mt
source-git-commit: eabd8650886fa78d9d177f3c588374a443ac1ad6
workflow-type: tm+mt
source-wordcount: '1781'
ht-degree: 0%

---


# Credenziali del servizio

Le integrazioni con AEM come Cloud Service devono poter essere autenticate in modo sicuro per AEM. AEM Developer Console consente di accedere alle credenziali del servizio, che vengono utilizzate per facilitare l&#39;interazione programmatica di applicazioni, sistemi e servizi esterni con i servizi AEM Author o Publish Services tramite HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

Le credenziali del servizio possono essere simili a [Token di accesso allo sviluppo locale](./local-development-access-token.md), ma sono diverse in alcuni modi principali:

+ Le credenziali di servizio sono _token di accesso_ non, ma credenziali utilizzate per ottenere i token di accesso.
+ Le credenziali del servizio sono permanenti e non vengono modificate se non revocate, mentre i token di accesso allo sviluppo locale scadono ogni giorno.
+ Le credenziali del servizio per un AEM come ambiente di Cloud Service vengono mappate a un singolo utente AEM, mentre i token di accesso allo sviluppo locale vengono autenticati come l&#39;utente AEM che ha generato il token di accesso.

## Genera credenziali del servizio

La generazione delle credenziali del servizio è suddivisa in due fasi:

1. Inizializzazione delle credenziali di servizio una tantum da parte di un amministratore dell&#39;organizzazione IMS di Adobe 
1. Download e utilizzo del JSON delle credenziali del servizio

### Inizializzazione credenziali di servizio

A differenza dei Token di accesso allo sviluppo locale, le credenziali del servizio richiedono un&#39;inizializzazione una tantum da parte dell&#39;amministratore IMS dell&#39;organizzazione del Adobe  prima di poter essere scaricate.

![Inizializzare le credenziali del servizio](assets/service-credentials/initialize-service-credentials.png)

__Si tratta di un&#39;inizializzazione una tantum per AEM come ambiente di Cloud Service__

1. Assicurati di aver effettuato l&#39;accesso come amministratore dell&#39;organizzazione IMS del Adobe 
1. Accedi a [ Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Aprire il programma contenente il AEM come ambiente Cloud Service per integrare l&#39;impostazione delle credenziali di servizio per
1. Toccate i puntini di sospensione accanto all&#39;ambiente nella sezione __Ambienti__ e selezionate __Developer Console__
1. Toccate la scheda __Integrazioni__
1. Toccare il pulsante __Ottieni credenziali servizio__
1. Le credenziali del servizio verranno inizializzate e visualizzate come JSON

![AEM Developer Console - Integrazioni - Ottieni credenziali di servizio](./assets/service-credentials/developer-console.png)

Una volta inizializzata l&#39;AEM come credenziali di servizio dell&#39;ambiente di Cloud Service, altri utenti possono scaricarle.

### Download delle credenziali del servizio

![Download delle credenziali del servizio](assets/service-credentials/download-service-credentials.png)

Il download delle credenziali del servizio segue gli stessi passaggi dell&#39;inizializzazione. Se l&#39;inizializzazione non si è ancora verificata, all&#39;utente verrà visualizzato un errore toccando il pulsante __Get Service Credentials__.

1. Assicurati di essere membro del profilo di prodotto __Cloud Manager - Developer__ IMS (che concede l&#39;accesso a AEM Developer Console)
   + La AEM sandbox come ambiente di Cloud Service richiede solo l&#39;iscrizione al profilo di prodotto __AEM Administrators__ o __AEM Users__
1. Accedi a [ Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Aprire il programma contenente il AEM come ambiente Cloud Service da integrare con
1. Toccate i puntini di sospensione accanto all&#39;ambiente nella sezione __Ambienti__ e selezionate __Developer Console__
1. Toccate la scheda __Integrazioni__
1. Toccare il pulsante __Ottieni credenziali servizio__
1. Toccate il pulsante di download nell&#39;angolo in alto a sinistra per scaricare il file JSON contenente il valore delle credenziali del servizio e salvate il file in una posizione sicura.
   + _Se queste credenziali di servizio sono compromesse, contatta immediatamente  supporto di Adobe per richiederne la revoca_

## Installare le credenziali del servizio

Le credenziali di servizio forniscono i dettagli necessari per generare un JWT, scambiato con un token di accesso utilizzato per l&#39;autenticazione con AEM come Cloud Service. Le credenziali del servizio devono essere archiviate in una posizione protetta accessibile dalle applicazioni, dai sistemi o dai servizi esterni che le utilizzano per accedere alle AEM. La modalità e la posizione di gestione delle credenziali del servizio saranno univoche per cliente.

Per semplicità, questa esercitazione passa le credenziali di servizio tramite la riga di comando, tuttavia, collaborate con il team di sicurezza IT per comprendere come memorizzare e accedere a tali credenziali in conformità alle linee guida di sicurezza della vostra organizzazione.

1. Copiare il [scaricato Service Credentials JSON](#download-service-credentials) in un file denominato `service_token.json` nella directory principale del progetto
   + Ma ricorda, non fare mai credenziali a Git!

## Usa credenziali del servizio

Le credenziali del servizio, un oggetto JSON completo, non sono uguali né al JWT né al token di accesso. Le credenziali di servizio (che contengono una chiave privata) vengono invece utilizzate per generare un JWT, che viene scambiato con  API IMS Adobe per un token di accesso.

![Credenziali del servizio - Applicazione esterna](assets/service-credentials/service-credentials-external-application.png)

1. Scaricare le credenziali di servizio da AEM Developer Console in una posizione protetta
1. Un&#39;applicazione esterna deve interagire in modo programmatico con AEM come ambiente di Cloud Service
1. L&#39;applicazione esterna legge le credenziali del servizio da una posizione protetta
1. L&#39;applicazione esterna utilizza le informazioni provenienti dalle credenziali del servizio per creare un token JWT
1. Il token JWT viene inviato a  Adobe IMS per scambiare un token di accesso
1.  Adobe IMS restituisce un token di accesso che può essere utilizzato per accedere AEM come Cloud Service
   + I token di accesso possono presentare una richiesta di scadenza. È consigliabile mantenere la durata del token di accesso breve e aggiornarlo quando necessario.
1. L&#39;applicazione esterna esegue le richieste HTTP da AEM come Cloud Service, aggiungendo il token di accesso come token del portatore all&#39;intestazione Autorizzazione delle richieste HTTP
1. AEM come Cloud Service riceve la richiesta HTTP, autentica la richiesta ed esegue il lavoro richiesto dalla richiesta HTTP, quindi restituisce una risposta HTTP all’applicazione esterna

### Aggiornamenti all’applicazione esterna

Per accedere AEM come Cloud Service utilizzando le credenziali di servizio, l&#39;applicazione esterna deve essere aggiornata in 3 modi:

1. Leggi in Credenziali di servizio
   + Per semplicità, li leggeremo dal file JSON scaricato, ma negli scenari di utilizzo reale, le credenziali di servizio devono essere archiviate in modo sicuro in conformità alle linee guida sulla sicurezza della tua organizzazione
1. Generazione di un JWT dalle credenziali del servizio
1. Exchange JWT per un token di accesso
   + Quando sono presenti credenziali di servizio, l&#39;applicazione esterna utilizza questo token di accesso invece del token di accesso allo sviluppo locale, quando accede AEM come Cloud Service

In questa esercitazione,  modulo  `@adobe/jwt-auth` npm viene utilizzato per entrambi, (1) generare il JWT dalle credenziali di servizio e (2) scambiarlo per un token di accesso, in una singola chiamata di funzione. Se l&#39;applicazione non è basata su JavaScript, controllare il codice di esempio [in altri linguaggi](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) per come creare un JWT dalle credenziali del servizio e scambiarlo per un token di accesso con  Adobe IMS.

## Leggi le credenziali del servizio

Rivedete la `getCommandLineParams()` e verificate che è possibile leggere nei file JSON delle credenziali di servizio utilizzando lo stesso codice utilizzato per leggere nel JSON del token di accesso allo sviluppo locale.

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## Creare un JWT e scambiare un token di accesso

Una volta lette le credenziali di servizio, queste vengono utilizzate per generare un JWT che viene poi scambiato con  API IMS Adobe per un token di accesso, che può quindi essere utilizzato per accedere AEM come un Cloud Service.

Questa applicazione di esempio è basata su Node.js, quindi è consigliabile utilizzare il modulo npm [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) per facilitare la generazione (1) JWT e (20 scambio con  Adobe IMS. Se l&#39;applicazione viene sviluppata utilizzando un&#39;altra lingua, consultare [gli esempi di codice appropriati](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) su come creare la richiesta HTTP per  Adobe IMS utilizzando altri linguaggi di programmazione.

1. Aggiornate la `getAccessToken(..)` per ispezionare il contenuto del file JSON e determinare se rappresenta un token di accesso allo sviluppo locale o credenziali di servizio. Questo può essere facilmente ottenuto verificando l&#39;esistenza della proprietà `.accessToken`, che esiste solo per il token di accesso allo sviluppo locale JSON.

Se vengono fornite le credenziali del servizio, l&#39;applicazione genera un JWT e lo scambia con  Adobe IMS per un token di accesso. Utilizzeremo la funzione [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) `auth(...)` che genera un JWT e lo scambia per un token di accesso in una singola chiamata di funzione.  I parametri a `auth(..)` è un oggetto [JSON composto da informazioni specifiche](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponibile dal JSON delle credenziali del servizio, come descritto di seguito nel codice.

```javascript
 async function getAccessToken(developerConsoleCredentials) {

     if (developerConsoleCredentials.accessToken) {
         // This is a Local Development access token
         return developerConsoleCredentials.accessToken;
     } else {
         // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
         let serviceCredentials = developerConsoleCredentials.integration;

         // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
         // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
         let { access_token } = await auth({
             clientId: serviceCredentials.technicalAccount.clientId, // Client Id
             technicalAccountId: serviceCredentials.id,              // Technical Account Id
             orgId: serviceCredentials.org,                          // Adobe IMS Org Id
             clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
             privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
             metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
             ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
         });

         return access_token;
     }
 }
```

    Ora, a seconda del file JSON - il token di accesso allo sviluppo locale JSON o il JSON delle credenziali del servizio - viene passato tramite il parametro della riga di comando `file`, l&#39;applicazione ricava un token di accesso.
    
    Tenere presente che, mentre le credenziali del servizio non scadono, il JWT e il token di accesso corrispondente lo fanno e devono essere aggiornati 12 ore dopo il rilascio. Ciò può essere fatto utilizzando un `refresh_token` [fornito  Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-token).

1. Con queste modifiche, e il JSON delle credenziali del servizio scaricato dalla AEM Developer Console (e per semplicità, salvato come `service_token.json` la stessa cartella di questo `index.js`), eseguite l&#39;applicazione sostituendo il parametro della riga di comando `file` con `service_token.json` e aggiornate il `propertyValue` a un nuovo valore in modo che gli effetti siano evidenti in AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   L&#39;output al terminale sarà simile al seguente:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   Le righe __403 - Forbidden__ indicano gli errori nelle chiamate API HTTP da AEM come Cloud Service. Questi 403 errori proibiti si verificano quando si tenta di aggiornare i metadati delle risorse.

   Questo è dovuto al fatto che il token di accesso derivato da Credenziali di servizio autentica la richiesta di AEM utilizzando un account tecnico creato automaticamente AEM utente, che per impostazione predefinita dispone solo dell&#39;accesso in lettura. Per fornire l&#39;accesso in scrittura all&#39;AEM dell&#39;applicazione, all&#39;account tecnico AEM utente associato al token di accesso deve essere concessa l&#39;autorizzazione AEM.

## Configurare l&#39;accesso in AEM

Il token di accesso derivato da Credenziali di servizio utilizza un account tecnico AEM Utente con appartenenza al gruppo AEM collaboratori.

![Credenziali del servizio - Account tecnico AEM utente](./assets/service-credentials/technical-account-user.png)

Una volta che l&#39;account tecnico AEM l&#39;utente esiste in AEM (dopo la prima richiesta HTTP con il token di accesso), le autorizzazioni di questo AEM utente possono essere gestite come gli altri AEM utenti.

1. Innanzitutto, individuare il nome di accesso AEM dell&#39;account tecnico aprendo il JSON delle credenziali del servizio scaricato da AEM Developer Console, quindi individuare il valore `integration.email`, che dovrebbe essere simile a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Accedete al servizio Author dell&#39;ambiente AEM corrispondente come amministratore AEM
1. Passare a __Strumenti__ > __Sicurezza__ > __Utenti__
1. Individuare l&#39;utente AEM con il __nome di accesso__ identificato nel passaggio 1, e aprire le relative __proprietà__
1. Andate alla scheda __Groups__ e aggiungete il gruppo __Utenti DAM__ (a cui è possibile accedere in scrittura alle risorse)
1. Toccare __Save and Close__

Con l’account tecnico autorizzato in AEM a disporre delle autorizzazioni di scrittura sulle risorse, eseguite nuovamente l’applicazione:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

L&#39;output al terminale sarà simile al seguente:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verificare le modifiche

1. Accedete al AEM come ambiente di Cloud Service che è stato aggiornato (utilizzando lo stesso nome host fornito nel parametro della riga di comando `aem`)
1. Passa a __Risorse__ > __File__
1. Spostatevi nella cartella delle risorse specificata dal parametro della riga di comando `folder`, ad esempio __WKND__ > __English__ > __Adventures__ > __Napa Wine Tasting__
1. Aprite la cartella __Properties__ per qualsiasi risorsa presente nella cartella
1. Passare alla scheda __Avanzate__
1. Rivedete il valore della proprietà aggiornata, ad esempio __Copyright__ mappato alla proprietà JCR `metadata/dc:rights` aggiornata, che ora riflette il valore fornito nel parametro `propertyValue`, ad esempio __Uso limitato WKND__

![Aggiornamento per l’utilizzo limitato dei metadati WKND](./assets/service-credentials/asset-metadata.png)

## Revoca delle credenziali del servizio




## Congratulazioni!

Ora che abbiamo eseguito l&#39;accesso a livello di programmazione AEM come Cloud Service utilizzando un token di accesso allo sviluppo locale, così come un token di accesso ai servizi pronti per la produzione!

