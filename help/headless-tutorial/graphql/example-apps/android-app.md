---
title: App Android - Esempio di AEM headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Android illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM headless as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 235
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# App Android

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Android illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM. Il [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java) viene utilizzato per eseguire le query GraphQL e mappare i dati su oggetti Java per alimentare l’app.

![App Java Android con AEM headless](./assets/android-java-app/android-app.png)


Visualizza [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## Requisiti AEM

L’applicazione Android funziona con le seguenti opzioni di implementazione AEM. Tutte le distribuzioni richiedono [Sito WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) da installare.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

L’applicazione Android è progettata per connettersi a un __Pubblicazione AEM__ Tuttavia, può originare il contenuto da AEM Author se l’autenticazione viene fornita nella configurazione dell’applicazione Android.

## Come usare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) e apri la cartella `android-app`
1. Modificare il file `config.properties` a `app/src/main/assets/config.properties` e aggiorna `contentApi.endpoint` per adattarsi all’ambiente AEM di destinazione:

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __Autenticazione di base__

   Il `contentApi.user` e `contentApi.password` autentica un utente AEM locale con accesso al contenuto WKND GraphQL.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Scarica un [Dispositivo virtuale Android](https://developer.android.com/studio/run/managing-avds) (API minima 28).
1. Crea e implementa l’app utilizzando l’emulatore Android.


### Connessione agli ambienti AEM

Se ci si connette a un ambiente di authoring AEM [autorizzazione](https://github.com/adobe/aem-headless-client-java#using-authorization) è obbligatorio. Il [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) consente di utilizzare [autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Per utilizzare il generatore client di aggiornamento autenticazione basato su token in `AdventureLoader.java` e `AdventuresLoader.java`:

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## Il codice

Di seguito è riportato un breve riepilogo dei file e del codice importanti utilizzati per alimentare l&#39;applicazione. Il codice completo è disponibile su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### Query persistenti

Seguendo le best practice di AEM Headless, l’applicazione iOS utilizza query persistenti AEM GraphQL per eseguire query sui dati di avventura. L’applicazione utilizza due query persistenti:

+ `wknd/adventures-all` query persistente, che restituisce tutte le avventure in AEM con un set abbreviato di proprietà. Questa query persistente guida l’elenco di avventure della visualizzazione iniziale.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _dynamicUrl
                _path
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` query persistente, che restituisce una singola avventura di `slug` (proprietà personalizzata che identifica in modo univoco un’avventura) con un set completo di proprietà. Questa query persistente attiva le visualizzazioni dei dettagli dell’avventura.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
      }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
          _path
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Esegui query persistente GraphQL

Le query persistenti dell’AEM vengono eseguite tramite HTTP GET, pertanto [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java) viene utilizzato per eseguire le query GraphQL persistenti sull’AEM e caricare il contenuto dell’avventura nell’app.

Ogni query persistente ha una classe &quot;loader&quot; corrispondente, che chiama in modo asincrono l’endpoint del GET HTTP dell’AEM e restituisce i dati dell’avventura utilizzando il file personalizzato definito [modello dati](#data-models).

+ `loader/AdventuresLoader.java`

  Recupera l’elenco delle Avventure nella schermata iniziale dell’applicazione utilizzando `wknd-shared/adventures-all` query persistente.

+ `loader/AdventureLoader.java`

  Recupera una singola avventura selezionandola tramite `slug` , utilizzando `wknd-shared/adventure-by-slug` query persistente.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### Modelli di dati di risposta GraphQL{#data-models}

`Adventure.java` è un POJO Java inizializzato con i dati JSON della richiesta GraphQL e modella un’avventura da utilizzare nelle viste dell’applicazione Android.

### Viste

L’applicazione Android utilizza due viste per presentare i dati dell’avventura nell’esperienza mobile.

+ `AdventureListFragment.java`

  Richiama `AdventuresLoader` e mostra le avventure restituite in un elenco.

+ `AdventureDetailFragment.java`

  Richiama `AdventureLoader` utilizzando `slug` param passato tramite la selezione di avventura sul `AdventureListFragment` visualizza e mostra i dettagli di una singola avventura.

### Immagini remote

`loader/RemoteImagesCache.java` è una classe di utilità che consente di preparare immagini remote in una cache in modo che possano essere utilizzate con elementi dell’interfaccia utente Android. Il contenuto dell’avventura fa riferimento alle immagini in AEM Assets tramite un URL e questa classe viene utilizzata per visualizzare tale contenuto.

## Risorse aggiuntive

+ [Guida introduttiva di AEM headless - Tutorial GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=it)
+ [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java)
