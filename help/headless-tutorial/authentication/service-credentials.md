---
title: Credenziali del servizio
description: Scopri come utilizzare le credenziali del servizio utilizzate per facilitare l’interazione programmatica tra applicazioni, sistemi e servizi esterni e i servizi Author o Publish tramite HTTP.
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
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: 65d8fd58f421a186e3624918c70cc5d79ec23700
workflow-type: tm+mt
source-wordcount: '1967'
ht-degree: 0%

---

# Credenziali del servizio

Le integrazioni con Adobe Experience Manager (AEM) as a Cloud Service devono essere in grado di eseguire l’autenticazione al servizio AEM in modo sicuro. La console per sviluppatori AEM consente di accedere alle credenziali del servizio, utilizzate per facilitare l’interazione programmatica tra applicazioni, sistemi e servizi esterni e i servizi di creazione o pubblicazione dell’AEM tramite HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Le credenziali del servizio potrebbero essere simili [Token di accesso per lo sviluppo locale](./local-development-access-token.md) ma sono diversi in alcuni modi chiave:

+ Le credenziali del servizio sono associate agli account tecnici. Per un account tecnico possono essere attive più credenziali di servizio.
+ Le credenziali del servizio sono _non_ token di accesso, ma sono credenziali utilizzate per _ottenere_ token di accesso.
+ Le credenziali del servizio sono più permanenti (il loro certificato scade ogni 365 giorni) e non cambiano se non revocate, mentre i token di accesso per lo sviluppo locale scadono ogni giorno.
+ Le credenziali di servizio per un ambiente as a Cloud Service AEM vengono mappate a un singolo utente di account tecnico AEM, mentre i token di accesso per lo sviluppo locale vengono autenticati come utente AEM che ha generato il token di accesso.
+ Un ambiente AEM as a Cloud Service può avere fino a dieci account tecnici, ciascuno con le proprie credenziali di servizio, ciascuno con mappatura per utente AEM account tecnico discreto.

Sia le credenziali del servizio che i token di accesso generati e i token di accesso per lo sviluppo locale devono essere tenuti segreti. Poiché tutti e tre possono essere utilizzati per ottenere, l&#39;accesso al rispettivo ambiente as a Cloud Service AEM.

## Genera credenziali servizio

La generazione delle credenziali del servizio è suddivisa in due passaggi:

1. Creazione di un account tecnico una tantum da parte di un amministratore dell’organizzazione Adobe IMS
1. Download e utilizzo del codice JSON per le credenziali del servizio dell’account tecnico

### Creare un account tecnico

A differenza dei token di accesso per lo sviluppo locale, le credenziali del servizio richiedono la creazione di un account tecnico da parte di un amministratore IMS dell’organizzazione di Adobi prima che sia possibile scaricarle. Devono essere creati conti tecnici discreti per ogni cliente che richiede un accesso programmatico all’AEM.

![Creare un account tecnico](assets/service-credentials/initialize-service-credentials.png)

Gli account tecnici vengono creati una volta, tuttavia le chiavi private utilizzate per gestire le credenziali del servizio associate all’account tecnico possono essere gestite nel tempo. Ad esempio, è necessario generare nuove credenziali chiave privata/servizio prima della scadenza della chiave privata corrente, per consentire a un utente di accedere ininterrottamente alle credenziali del servizio.

1. Assicurati di aver effettuato l’accesso come:
   + __Amministratore di sistema dell’organizzazione Adobe IMS__
   + Membro del __Amministratori AEM__ Profilo prodotto IMS su __Autore AEM__
1. Accedi a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Apri il programma contenente l’ambiente AEM as a Cloud Service per integrare configurare le credenziali del servizio per
1. Tocca i puntini di sospensione accanto all’ambiente in __Ambienti__ e seleziona __Console per sviluppatori__
1. Tocca in __Integrazioni__ scheda
1. Tocca il __Account tecnici__ scheda
1. Tocca __Crea nuovo account tecnico__ pulsante
1. Le credenziali di servizio dell’account tecnico vengono inizializzate e visualizzate come JSON

![Console per sviluppatori AEM - Integrazioni - Ottieni credenziali del servizio](./assets/service-credentials/developer-console.png)

Una volta inizializzate le credenziali del servizio dell’ambiente AEM as Cloud Service, possono essere scaricate da altri sviluppatori AEM della tua organizzazione Adobe IMS.

### Scarica credenziali servizio

![Scarica credenziali servizio](assets/service-credentials/download-service-credentials.png)

Il download delle credenziali del servizio segue la stessa procedura dell&#39;inizializzazione.

1. Assicurati di aver effettuato l’accesso come:
   + __Amministratore dell’organizzazione Adobe IMS__
   + Membro del __Amministratori AEM__ Profilo prodotto IMS su __Autore AEM__
1. Accedi a [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Apri il Programma contenente l’ambiente as a Cloud Service AEM da integrare con
1. Tocca i puntini di sospensione accanto all’ambiente in __Ambienti__ e seleziona __Console per sviluppatori__
1. Tocca in __Integrazioni__ scheda
1. Tocca il __Account tecnici__ scheda
1. Espandi __Account tecnico__ da utilizzare
1. Espandi __Chiave privata__ le cui credenziali del servizio verranno scaricate e verifica che lo stato sia __Attivo__
1. Tocca il __...__ > __Visualizza__ associato al __Chiave privata__, che visualizza il codice JSON per le credenziali del servizio
1. Tocca il pulsante Scarica nell’angolo in alto a sinistra per scaricare il file JSON contenente il valore delle credenziali del servizio e salvare il file in una posizione sicura

## Installare le credenziali del servizio

Le credenziali del servizio forniscono i dettagli necessari per generare un JWT, che viene scambiato con un token di accesso utilizzato per l’autenticazione con AEM as a Cloud Service. Le credenziali del servizio devono essere archiviate in una posizione sicura accessibile dalle applicazioni, dai sistemi o dai servizi esterni che le utilizzano per accedere all&#39;AEM. La modalità e la posizione di gestione delle credenziali del servizio sono univoche per cliente.

Per semplicità, questo tutorial trasmette le credenziali del servizio in tramite la riga di comando. Tuttavia, rivolgiti al tuo team di sicurezza IT per scoprire come memorizzare e accedere a queste credenziali in conformità alle linee guida di sicurezza della tua organizzazione.

1. Copia il [ha scaricato il codice JSON per le credenziali del servizio](#download-service-credentials) in un file denominato `service_token.json` nella directory principale del progetto
   + Ricorda, non eseguire mai il commit _eventuali credenziali_ a Git!

## Usa credenziali servizio

Le credenziali del servizio, un oggetto JSON completo, non corrispondono al JWT né al token di accesso. Al contrario, le credenziali del servizio (che contengono una chiave privata) vengono utilizzate per generare un JWT, che viene scambiato con le API Adobe IMS per un token di accesso.

![Credenziali servizio - Applicazione esterna](assets/service-credentials/service-credentials-external-application.png)

1. Scaricare le credenziali del servizio da AEM Developer Console in un percorso sicuro
1. L’applicazione esterna deve interagire a livello di programmazione con l’ambiente as a Cloud Service AEM
1. L&#39;applicazione esterna legge le credenziali del servizio da un percorso sicuro
1. L’applicazione esterna utilizza le informazioni contenute nelle credenziali del servizio per creare un token JWT
1. Il token JWT viene inviato ad Adobe IMS per scambiarlo con un token di accesso
1. Adobe IMS restituisce un token di accesso che può essere utilizzato per accedere a AEM as a Cloud Service
   + I token di accesso non possono modificare un’ora di scadenza.
1. L’applicazione esterna effettua richieste HTTP all’AEM as a Cloud Service, aggiungendo il token di accesso come token Bearer all’intestazione Authorization delle richieste HTTP
1. AEM as a Cloud Service riceve la richiesta HTTP, la autentica, esegue il lavoro richiesto dalla richiesta HTTP e restituisce una risposta HTTP all’applicazione esterna.

### Aggiornamenti dell’applicazione esterna

Per accedere a AEM as a Cloud Service utilizzando le credenziali del servizio, l’applicazione esterna deve essere aggiornata in tre modi:

1. Leggi nelle credenziali del servizio

+ Per semplicità, le credenziali del servizio vengono lette dal file JSON scaricato. Tuttavia, in scenari di utilizzo reale, le credenziali del servizio devono essere memorizzate in modo sicuro in conformità alle linee guida sulla sicurezza della tua organizzazione

1. Generare un JWT dalle credenziali del servizio
1. Sostituire il JWT con un token di accesso

+ Se sono presenti le credenziali del servizio, l&#39;applicazione esterna utilizza questo token di accesso invece del token di accesso per lo sviluppo locale quando accede a AEM as a Cloud Service

In questo tutorial, Adobe di `@adobe/jwt-auth` Il modulo npm viene utilizzato per entrambi, (1) generare il JWT dalle credenziali del servizio e (2) scambiarlo per un token di accesso, in una singola chiamata di funzione. Se l&#39;applicazione non è basata su JavaScript, controlla [codice di esempio in altre lingue](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) informazioni su come creare un JWT dalle credenziali del servizio e scambiarlo per un token di accesso con Adobe IMS.

## Lettura delle credenziali del servizio

Rivedi `getCommandLineParams()` Scopri come viene letto il file JSON delle credenziali del servizio utilizzando lo stesso codice utilizzato per leggere nel JSON del token di accesso per lo sviluppo locale.

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

## Creare un JWT e scambiarlo con un token di accesso

Una volta lette le credenziali del servizio, queste vengono utilizzate per generare un JWT che viene quindi scambiato con le API Adobe IMS per un token di accesso. Questo token di accesso può quindi essere utilizzato per accedere a AEM as a Cloud Service.

Questa applicazione di esempio è basata su Node.js ed è quindi consigliabile utilizzarla [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) modulo npm per facilitare la (1) generazione JWT e (20 scambi con Adobe IMS. Se l&#39;applicazione viene sviluppata in un&#39;altra lingua, rivedere [gli esempi di codice appropriati](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) su come creare la richiesta HTTP ad Adobe IMS utilizzando altri linguaggi di programmazione.

1. Aggiornare il `getAccessToken(..)` per esaminare il contenuto del file JSON e determinare se rappresenta un token di accesso per lo sviluppo locale o credenziali del servizio. Ciò può essere facilmente ottenuto verificando l&#39;esistenza del `.accessToken` , che esiste solo per il token di accesso per lo sviluppo locale JSON.

   Se vengono fornite le credenziali del servizio, l’applicazione genera un JWT e lo scambia con Adobe IMS per ottenere un token di accesso. Utilizza il [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)di `auth(...)` che genera un JWT e lo scambia per un token di accesso in una singola chiamata di funzione. I parametri per `auth(..)` sono un [Oggetto JSON costituito da informazioni specifiche](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponibile dal codice JSON per le credenziali del servizio, come descritto di seguito nel codice.

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

    Ora, a seconda del file JSON (il JSON per il token di accesso per lo sviluppo locale o il JSON per le credenziali del servizio) trasmesso tramite il parametro della riga di comando &quot;file&quot;, l’applicazione ricava un token di accesso.
    
    Tieni presente che mentre le credenziali del servizio scadono ogni 365 giorni, il JWT e il token di accesso corrispondente scadono di frequente e devono essere aggiornati prima della scadenza. Per farlo, usa un &quot;refresh_token&quot; [fornito da Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Con queste modifiche implementate, il codice JSON per le credenziali del servizio è stato scaricato da AEM Developer Console e, per semplicità, salvato come `service_token.json` nella stessa cartella di questo `index.js`. Ora eseguiamo l’applicazione sostituendo il parametro della riga di comando `file` con `service_token.json`, e l&#39;aggiornamento `propertyValue` a un nuovo valore, in modo che gli effetti siano evidenti nell’AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   L&#39;output sul terminale è simile al seguente:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   Il __403 - Non consentito__ righe, indica gli errori nelle chiamate API HTTP a AEM as a Cloud Service. Questi errori 403 Forbidden si verificano quando si tenta di aggiornare i metadati delle risorse.

   Il motivo è che il token di accesso derivato dalle credenziali del servizio autentica la richiesta all’AEM utilizzando un utente AEM con account tecnico creato automaticamente che, per impostazione predefinita, dispone solo dell’accesso in lettura. Per fornire all’applicazione l’accesso in scrittura all’AEM, l’utente AEM dell’account tecnico associato al token di accesso deve ottenere l’autorizzazione nell’AEM.

## Configurare l’accesso in AEM

Il token di accesso derivato dalle credenziali del servizio utilizza un account tecnico utente AEM con appartenenza nel __Collaboratori__ Gruppo di utenti AEM.

![Credenziali di servizio - Utente AEM dell’account tecnico](./assets/service-credentials/technical-account-user.png)

Una volta che l’utente AEM dell’account tecnico esiste nell’AEM (dopo la prima richiesta HTTP con il token di accesso), le autorizzazioni di questo utente AEM possono essere gestite come quelle di altri utenti AEM.

1. Innanzitutto, individua il nome di accesso AEM dell’account tecnico aprendo il file JSON delle credenziali del servizio scaricato da AEM Developer Console, quindi individua `integration.email` valore, che deve essere simile a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Accedere al servizio Author dell’ambiente AEM corrispondente come amministratore AEM
1. Accedi a __Strumenti__ > __Sicurezza__ > __Utenti__
1. Individuare l’utente AEM con il __Nome di accesso__ identificati nel passaggio 1 e aprono i relativi __Proprietà__
1. Accedi a __Gruppi__ e aggiungi il __Utenti DAM__ gruppo (con accesso in scrittura alle risorse)
   + [Consulta l’elenco dei gruppi di utenti AEM forniti](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html#built-in-users-and-groups) per aggiungere l’utente del servizio a per ottenere le autorizzazioni ottimali. Se nessun gruppo di utenti AEM fornito è sufficiente, creane uno tuo e aggiungi le autorizzazioni appropriate.
1. Tocca __Salva e chiudi__

Se l&#39;account tecnico è autorizzato in AEM a disporre delle autorizzazioni di scrittura per le risorse, eseguire nuovamente l&#39;applicazione:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

L&#39;output sul terminale è simile al seguente:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verifica le modifiche

1. Accedi all’ambiente as a Cloud Service dell’AEM che è stato aggiornato (utilizzando lo stesso nome host fornito in `aem` (parametro della riga di comando)
1. Accedi a __Risorse__ > __File__
1. Spostati nella cartella delle risorse specificata da `folder` parametro della riga di comando, ad esempio __WKND__ > __Inglese__ > __Avventure__ > __Degustazione del vino Napa__
1. Apri __Proprietà__ per qualsiasi risorsa nella cartella
1. Accedi a __Avanzate__ scheda
1. Rivedi il valore della proprietà aggiornata, ad esempio __Copyright__ che è mappato sul file aggiornato `metadata/dc:rights` proprietà JCR, che ora riflette il valore fornito nella sezione `propertyValue` parametro, ad esempio __Utilizzo limitato WKND__

![Aggiornamento metadati per utilizzo limitato WKND](./assets/service-credentials/asset-metadata.png)

## Congratulazioni.

Ora che abbiamo effettuato l’accesso a livello di programmazione a AEM as a Cloud Service utilizzando un token di accesso per lo sviluppo locale e un token di accesso per il servizio pronto per la produzione.
