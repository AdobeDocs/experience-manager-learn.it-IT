---
title: App Android - Esempio AEM Headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Android illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM.
version: Experience Manager as a Cloud Service
mini-toc-levels: 2
jira: KT-10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 160
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# App Android

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Android illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM. Il client [AEM Headless per Java](https://github.com/adobe/aem-headless-client-java) viene utilizzato per eseguire le query GraphQL e mappare i dati su oggetti Java per alimentare l&#39;app.

![App Java Android con AEM Headless](./assets/android-java-app/android-app.png)


Visualizza il [codice sorgente in GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## Requisiti di AEM

L’applicazione Android funziona con le seguenti opzioni di distribuzione AEM. Tutte le distribuzioni richiedono l&#39;installazione del sito [WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest).

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=it)

L&#39;applicazione Android è progettata per connettersi a un ambiente __AEM Publish__, tuttavia può creare contenuto da AEM Author se l&#39;autenticazione viene fornita nella configurazione dell&#39;applicazione Android.

## Come usare

1. Clona l&#39;archivio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri [Android Studio](https://developer.android.com/studio) e apri la cartella `android-app`
1. Modifica il file `config.properties` in `app/src/main/assets/config.properties` e aggiorna `contentApi.endpoint` in base all&#39;ambiente AEM di destinazione:

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __Autenticazione di base__

   `contentApi.user` e `contentApi.password` autenticano un utente AEM locale con accesso al contenuto GraphQL WKND.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=my-special-android-app-user
   contentApi.password=password123
   ```

1. Scarica un [dispositivo virtuale Android](https://developer.android.com/studio/run/managing-avds) (API minima 28).
1. Crea e implementa l’app utilizzando l’emulatore Android.


### Connessione agli ambienti AEM

Se la connessione a un ambiente di authoring AEM [autorizzazione](https://github.com/adobe/aem-headless-client-java#using-authorization) è obbligatoria. [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) consente di utilizzare l&#39;autenticazione [basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=it). Per utilizzare il generatore client di aggiornamento autenticazione basato su token in `AdventureLoader.java` e `AdventuresLoader.java`:

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

Di seguito è riportato un breve riepilogo dei file e del codice importanti utilizzati per alimentare l&#39;applicazione. Il codice completo si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### Query persistenti

Seguendo le best practice di AEM Headless, l’applicazione iOS utilizza query persistenti di AEM GraphQL per eseguire query sui dati di avventura. L’applicazione utilizza due query persistenti:

+ Query persistente `wknd/adventures-all`, che restituisce tutte le avventure in AEM con un set ridotto di proprietà. Questa query persistente guida l’elenco di avventure della visualizzazione iniziale.

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

+ Query persistente `wknd/adventure-by-slug`, che restituisce una singola avventura di `slug` (una proprietà personalizzata che identifica in modo univoco un&#39;avventura) con un set completo di proprietà. Questa query persistente attiva le visualizzazioni dei dettagli dell’avventura.

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

Le query persistenti di AEM vengono eseguite tramite HTTP GET, pertanto il client [AEM Headless per Java](https://github.com/adobe/aem-headless-client-java) viene utilizzato per eseguire le query persistenti di GraphQL su AEM e caricare il contenuto dell&#39;avventura nell&#39;app.

Ogni query persistente ha una classe &quot;loader&quot; corrispondente, che chiama in modo asincrono l&#39;endpoint AEM HTTP GET e restituisce i dati dell&#39;avventura utilizzando il [modello dati](#data-models) definito personalizzato.

+ `loader/AdventuresLoader.java`

  Recupera l&#39;elenco delle avventure nella schermata iniziale dell&#39;applicazione utilizzando la query persistente `wknd-shared/adventures-all`.

+ `loader/AdventureLoader.java`

  Recupera una singola avventura selezionandola tramite il parametro `slug`, utilizzando la query persistente `wknd-shared/adventure-by-slug`.

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

`Adventure.java` è un POJO Java inizializzato con i dati JSON della richiesta GraphQL e modella un&#39;avventura da utilizzare nelle viste dell&#39;applicazione Android.

### Viste

L’applicazione Android utilizza due viste per presentare i dati dell’avventura nell’esperienza mobile.

+ `AdventureListFragment.java`

  Richiama `AdventuresLoader` e visualizza le avventure restituite in un elenco.

+ `AdventureDetailFragment.java`

  Richiama `AdventureLoader` utilizzando il parametro `slug` trasmesso tramite la selezione di avventura nella visualizzazione `AdventureListFragment` e visualizza i dettagli di una singola avventura.

### Immagini remote

`loader/RemoteImagesCache.java` è una classe di utilità che consente di preparare immagini remote in una cache in modo che possano essere utilizzate con gli elementi dell&#39;interfaccia utente di Android. Il contenuto dell’avventura fa riferimento alle immagini in AEM Assets tramite un URL e questa classe viene utilizzata per visualizzare tale contenuto.

## Risorse aggiuntive

+ [Guida introduttiva di AEM Headless - Esercitazione GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=it)
+ [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java)
