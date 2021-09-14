---
title: Credenziali del servizio
description: Le credenziali del servizio AEM sono utilizzate per facilitare l’interazione programmatica di applicazioni, sistemi e servizi esterni con i servizi di authoring o pubblicazione AEM tramite HTTP.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1860'
ht-degree: 0%

---

# Credenziali del servizio

Le integrazioni con AEM come Cloud Service devono essere in grado di eseguire l’autenticazione in modo sicuro per AEM. AEM Developer Console consente di accedere alle credenziali del servizio, utilizzate per facilitare l’interazione programmatica di applicazioni, sistemi e servizi esterni con i servizi di authoring o pubblicazione AEM tramite HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Le credenziali del servizio possono apparire simili [Token di accesso allo sviluppo locale](./local-development-access-token.md) ma sono diverse in alcuni modi chiave:

+ Le credenziali del servizio sono _non_ token di accesso, ma sono credenziali utilizzate per _ottenere_ token di accesso.
+ Le credenziali del servizio sono più permanenti (scadono ogni 365 giorni) e non cambiano se non vengono revocate, mentre i token di accesso allo sviluppo locale scadono ogni giorno.
+ Le credenziali del servizio per un AEM come ambiente di Cloud Service vengono mappate a un singolo utente di account tecnico AEM, mentre i token di accesso allo sviluppo locale si autenticano come utente AEM che ha generato il token di accesso.

Sia le credenziali di servizio che i token di accesso che generano, nonché i token di accesso allo sviluppo locale dovrebbero essere mantenuti segreti, in quanto tutti e tre possono essere utilizzati per ottenere l&#39;accesso ai rispettivi AEM come ambienti di Cloud Service

## Genera credenziali del servizio

La generazione delle credenziali del servizio è suddivisa in due passaggi:

1. Inizializzazione delle credenziali di servizio una tantum da parte di un amministratore dell’organizzazione Adobe IMS
1. Download e utilizzo del JSON delle credenziali del servizio

### Inizializzazione delle credenziali del servizio

Le credenziali del servizio, a differenza dei token di accesso allo sviluppo locale, richiedono un _inizializzazione una tantum_ da parte dell&#39;amministratore IMS dell&#39;organizzazione di Adobe prima di poter essere scaricate.

![Inizializzare le credenziali del servizio](assets/service-credentials/initialize-service-credentials.png)

__Si tratta di un&#39;inizializzazione una tantum per AEM come ambiente di Cloud Service__

1. Verifica di aver effettuato l’accesso come segue:
   + Amministratore dell’organizzazione IMS di Adobe
   + Membro del profilo di prodotto __Cloud Manager - Developer__ IMS
   + Membro del profilo di prodotto __AEM utente__ o __AEM amministratori__ IMS su __Autore AEM__
1. Accedi a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Apri il programma contenente il AEM come ambiente di Cloud Service per integrare l’impostazione delle credenziali del servizio per
1. Tocca i puntini di sospensione accanto all’ambiente nella sezione __Ambienti__ e seleziona __Console per sviluppatori__
1. Tocca la scheda __Integrazioni__ .
1. Tocca il pulsante __Ottieni credenziali servizio__
1. Le credenziali del servizio verranno inizializzate e visualizzate come JSON

![AEM Developer Console - Integrazioni - Ottieni credenziali del servizio](./assets/service-credentials/developer-console.png)

Una volta inizializzata l’AEM come credenziali di servizio dell’ambiente di Cloud Service, altri sviluppatori AEM nella tua organizzazione Adobe IMS possono scaricarle.

### Scaricare le credenziali del servizio

![Scaricare le credenziali del servizio](assets/service-credentials/download-service-credentials.png)

Il download delle credenziali del servizio segue gli stessi passaggi dell&#39;inizializzazione. Se l&#39;inizializzazione non si è ancora verificata, all&#39;utente verrà visualizzato un errore premendo il pulsante __Ottieni credenziali servizio__ .

1. Verifica di aver effettuato l’accesso come:
   + Membro del profilo di prodotto __Cloud Manager - Developer__ IMS (che consente l&#39;accesso a AEM Developer Console)
      + Gli ambienti Sandbox AEM as a Cloud Service non richiedono l’iscrizione a __Cloud Manager - Developer__
   + Membro del profilo di prodotto __AEM utente__ o __AEM amministratori__ IMS su __Autore AEM__
1. Accedi a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Apri il programma contenente l’AEM come ambiente di Cloud Service con cui eseguire l’integrazione
1. Tocca i puntini di sospensione accanto all’ambiente nella sezione __Ambienti__ e seleziona __Console per sviluppatori__
1. Tocca la scheda __Integrazioni__ .
1. Tocca il pulsante __Ottieni credenziali servizio__
1. Tocca il pulsante di download nell’angolo in alto a sinistra per scaricare il file JSON contenente il valore delle credenziali del servizio e salvare il file in una posizione sicura.
   + _Se le credenziali del servizio sono compromesse, contatta immediatamente il supporto Adobe per richiedere la revoca_

## Installare le credenziali del servizio

Le credenziali del servizio forniscono i dettagli necessari per generare un JWT, scambiato con un token di accesso utilizzato per l’autenticazione con AEM come Cloud Service. Le credenziali del servizio devono essere archiviate in un percorso sicuro accessibile dalle applicazioni, dai sistemi o dai servizi esterni che le utilizzano per accedere alle AEM. Come e dove vengono gestite le credenziali del servizio saranno univoci per cliente.

Per semplicità, questa esercitazione passa le credenziali di servizio in tramite la riga di comando, tuttavia, collabora con il team di sicurezza IT per comprendere come archiviare e accedere a tali credenziali in conformità alle linee guida di sicurezza della tua organizzazione.

1. Copia il [scaricato Service Credentials JSON](#download-service-credentials) in un file denominato `service_token.json` nella directory principale del progetto
   + Ma ricorda, non impegnare mai nessuna creatività a Git!

## Utilizzare le credenziali del servizio

Le credenziali del servizio, un oggetto JSON completo, non sono uguali né al JWT né al token di accesso. Le credenziali del servizio (che contengono una chiave privata) vengono invece utilizzate per generare un JWT, che viene scambiato con le API Adobe IMS per un token di accesso.

![Credenziali del servizio - Applicazione esterna](assets/service-credentials/service-credentials-external-application.png)

1. Scaricare le credenziali del servizio da AEM Console per sviluppatori in un percorso protetto
1. Un’applicazione esterna deve interagire programmaticamente con AEM come ambiente di Cloud Service
1. L&#39;applicazione esterna legge le credenziali del servizio da una posizione protetta
1. L’applicazione esterna utilizza le informazioni provenienti dalle credenziali del servizio per creare un token JWT
1. Il token JWT viene inviato ad Adobe IMS per scambiare un token di accesso
1. Adobe IMS restituisce un token di accesso che può essere utilizzato per accedere a AEM come Cloud Service
   + La scadenza dei token di accesso può essere richiesta. È consigliabile mantenere la vita breve del token di accesso e aggiornarlo quando necessario.
1. L’applicazione esterna invia richieste HTTP a AEM come Cloud Service, aggiungendo il token di accesso come token portatore all’intestazione Autorizzazione delle richieste HTTP
1. AEM come Cloud Service riceve la richiesta HTTP, autentica la richiesta ed esegue il lavoro richiesto dalla richiesta HTTP e restituisce una risposta HTTP all&#39;applicazione esterna

### Aggiornamenti dell’applicazione esterna

Per accedere a AEM come Cloud Service utilizzando le Credenziali del servizio, l’applicazione esterna deve essere aggiornata in 3 modi:

1. Leggi nelle credenziali del servizio
   + Per semplicità, li leggeremo dal file JSON scaricato, ma in scenari di utilizzo reale, le credenziali del servizio devono essere memorizzate in modo sicuro in conformità alle linee guida per la sicurezza della tua organizzazione
1. Generare una JWT dalle credenziali del servizio
1. Scambio JWT per un token di accesso
   + Quando sono presenti credenziali di servizio, la nostra applicazione esterna utilizza questo token di accesso invece del token di accesso allo sviluppo locale, quando accedi a AEM come Cloud Service

In questa esercitazione, il modulo npm di Adobe `@adobe/jwt-auth` viene utilizzato per entrambi, (1) generare il JWT dalle credenziali del servizio e (2) scambiarlo per un token di accesso, in una singola chiamata della funzione. Se la tua applicazione non è basata su JavaScript, controlla il [codice di esempio in altri linguaggi](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) per sapere come creare un JWT dalle credenziali del servizio e scambialo per un token di accesso con Adobe IMS.

## Leggi le credenziali del servizio

Rivedi la sezione `getCommandLineParams()` e scopri che è possibile leggere nei file JSON delle credenziali del servizio utilizzando lo stesso codice utilizzato per leggere nel JSON del token di accesso allo sviluppo locale.

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

Una volta lette le credenziali del servizio, vengono utilizzate per generare un JWT che viene poi scambiato con le API Adobe IMS per un token di accesso, che può quindi essere utilizzato per accedere a AEM come Cloud Service.

Questa applicazione di esempio è basata su Node.js, quindi è meglio utilizzare il modulo [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm per facilitare la generazione (1) JWT e (20 scambio con Adobe IMS. Se l&#39;applicazione viene sviluppata utilizzando un&#39;altra lingua, controlla [gli esempi di codice appropriati](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) su come creare la richiesta HTTP ad Adobe IMS utilizzando altri linguaggi di programmazione.

1. Aggiorna il `getAccessToken(..)` per controllare il contenuto del file JSON e determinare se rappresenta un token di accesso allo sviluppo locale o le credenziali del servizio. Questo può essere facilmente ottenuto controllando l’esistenza della proprietà `.accessToken` , che esiste solo per il token di accesso allo sviluppo locale JSON.

   Se vengono fornite le credenziali di servizio, l’applicazione genera un JWT e lo scambia con Adobe IMS per un token di accesso. Useremo la funzione [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) `auth(...)` che genera un JWT e lo scambia per un token di accesso in una singola chiamata di funzione.  I parametri a `auth(..)` è un oggetto [JSON composto da informazioni specifiche](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponibile dal JSON delle credenziali del servizio, come descritto di seguito nel codice.

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

   Ora, a seconda del file JSON - il JSON del token di accesso allo sviluppo locale o il JSON delle credenziali del servizio - viene passato tramite il parametro della riga di comando `file` , l’applicazione deriva un token di accesso.

   Tieni presente che, mentre le credenziali del servizio scadono ogni 365 giorni, il JWT e il token di accesso corrispondente scadono frequentemente e devono essere aggiornati prima della loro scadenza. Per farlo, utilizza un `refresh_token` [fornito dall&#39;Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Con queste modifiche, e il JSON delle credenziali del servizio scaricato da AEM Developer Console (e per semplicità, salvato come `service_token.json` la stessa cartella di questo `index.js`), esegui l&#39;applicazione sostituendo il parametro della riga di comando `file` con `service_token.json` e aggiorna il `propertyValue` a un nuovo valore in modo che gli effetti siano evidenti in AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   L&#39;output sul terminale sarà simile al seguente:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   Le righe __403 - Forbidden__ indicano gli errori nelle chiamate API HTTP da AEM come Cloud Service. Questi 403 Errori proibiti si verificano quando si tenta di aggiornare i metadati delle risorse.

   Il motivo è che il token di accesso derivato da Service Credentials autentica la richiesta per AEM utilizzando un account tecnico creato automaticamente AEM utente, che per impostazione predefinita ha accesso in sola lettura. Per fornire l’accesso in scrittura all’AEM dell’applicazione, è necessario concedere l’autorizzazione all’account tecnico AEM utente associato al token di accesso in AEM.

## Configurare l’accesso in AEM

Il token di accesso derivato da Credenziali di servizio utilizza un account tecnico AEM Utente che ha l’appartenenza al gruppo di utenti AEM collaboratori.

![Credenziali del servizio - Account tecnico AEM utente](./assets/service-credentials/technical-account-user.png)

Una volta che l&#39;account tecnico AEM utente esiste in AEM (dopo la prima richiesta HTTP con il token di accesso), le autorizzazioni di questo utente AEM possono essere gestite come gli altri utenti AEM.

1. Per prima cosa, individua il nome di accesso AEM dell’account tecnico aprendo il JSON sulle credenziali del servizio scaricato da AEM Developer Console e individua il valore `integration.email` che dovrebbe essere simile a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Accedi al servizio Author dell’ambiente AEM corrispondente come amministratore AEM
1. Passa a __Strumenti__ > __Sicurezza__ > __Utenti__
1. Individua l&#39;utente AEM con il __Nome di accesso__ identificato nel passaggio 1 e apri le relative __Proprietà__
1. Passa alla scheda __Gruppi__ e aggiungi il gruppo __Utenti DAM__ (che come accesso in scrittura alle risorse)
1. Tocca __Salva e chiudi__

Se l’account tecnico in AEM dispone delle autorizzazioni di scrittura sulle risorse, esegui nuovamente l’applicazione:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

L&#39;output sul terminale sarà simile al seguente:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verifica le modifiche

1. Accedi al AEM come ambiente di Cloud Service aggiornato (utilizzando lo stesso nome host fornito nel parametro della riga di comando `aem` )
1. Passa a __Risorse__ > __File__
1. Spostati nella cartella risorse specificata dal parametro della riga di comando `folder`, ad esempio __WKND__ > __Inglese__ > __Avventure__ > __Degustazione dei vini Napa__
1. Apri __Proprietà__ per qualsiasi risorsa nella cartella
1. Passa alla scheda __Avanzate__
1. Rivedi il valore della proprietà aggiornata, ad esempio __Copyright__ mappata alla proprietà `metadata/dc:rights` JCR aggiornata, che ora riflette il valore fornito nel parametro `propertyValue`, ad esempio __Uso limitato WKND__

![Aggiornamento dei metadati con restrizioni WKND](./assets/service-credentials/asset-metadata.png)

## Congratulazioni!

Ora che abbiamo effettuato l’accesso programmaticamente AEM come Cloud Service utilizzando un token di accesso allo sviluppo locale, così come un token di accesso da servizio a servizio pronto per la produzione!
