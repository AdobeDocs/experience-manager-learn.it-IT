---
title: App Android - AEM esempio headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Android illustra come eseguire query sul contenuto utilizzando le API GraphQL di AEM.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 0204d9aaf7b79b0745adbe749f44245716203b88
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 4%

---

# App Android

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione Android illustra come eseguire query sul contenuto utilizzando le API GraphQL di AEM. La [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java) viene utilizzato per eseguire le query GraphQL e mappare i dati su oggetti Java per alimentare l&#39;app.

![App Java Android con AEM Headless](./assets/android-java-app/android-app.png)


Visualizza la [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## Requisiti AEM

L’applicazione Android funziona con le seguenti opzioni di distribuzione AEM. Tutte le implementazioni richiedono l’ [Sito WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) da installare.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configurazione locale tramite [l’SDK di AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it?lang=en#install-local-aem-instances)

L&#39;applicazione Android è progettata per connettersi a un __Pubblicazione AEM__ ambiente, tuttavia può sorgente di contenuto da AEM Author se l’autenticazione è fornita nella configurazione dell’applicazione Android.

## Come utilizzare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) e apri la cartella `android-app`
1. Modificare il file `config.properties` a `app/src/main/assets/config.properties` e aggiorna `contentApi.endpoint` per far corrispondere l’ambiente AEM di destinazione:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __Autenticazione di base__

   La `contentApi.user` e `contentApi.password` autentica un utente AEM locale con accesso al contenuto WKND GraphQL.

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Scaricare un [Dispositivo virtuale Android](https://developer.android.com/studio/run/managing-avds) (API minima 28).
1. Crea e distribuisci l’app utilizzando l’emulatore Android.


### Connessione ad ambienti AEM

`10.0.2.2` è un [alias IP speciale](https://developer.android.com/studio/run/emulator-networking) per localhost quando si utilizza la creazione dell&#39;emulatore `10.0.2.2:4502` equivale a `localhost:4502`. Se ci si connette a un ambiente di pubblicazione AEM (consigliato), non è necessaria alcuna autorizzazione e `contentAPi.user` e `contentApi.password` può essere lasciato vuoto.

Connessione a un ambiente di authoring AEM [autorizzazione](https://github.com/adobe/aem-headless-client-java#using-authorization) è obbligatorio. Per impostazione predefinita, l’applicazione è configurata per utilizzare l’autenticazione di base con un nome utente e una password di `admin:admin`. La [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) consente di utilizzare [autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Per utilizzare il generatore client di aggiornamento dell’autenticazione basato su token in `AdventureLoader.java` e `AdventuresLoader.java`:

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

Di seguito è riportato un breve riepilogo dei file importanti e del codice utilizzati per alimentare l&#39;applicazione. Il codice completo è disponibile all&#39;indirizzo [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### Query persistenti

Seguendo AEM best practice headless, l’applicazione iOS utilizza query persistenti AEM GraphQL per eseguire query sui dati di avventura. L&#39;applicazione utilizza due query persistenti:

+ `wknd/adventures-all` query persistente, che restituisce tutte le avventure in AEM con un set abbreviato di proprietà. Questa query persistente guida l&#39;elenco di avventura della visualizzazione iniziale.

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
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` query persistente, che restituisce una singola avventura per `slug` (proprietà personalizzata che identifica in modo univoco un’avventura) con un set completo di proprietà. Questa query persistente potenzia le visualizzazioni dei dettagli dell’avventura.

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
          _path
          mimeType
          width
          height
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

## Esegui query persistente GraphQL

Le query persistenti AEM vengono eseguite su HTTP GET e quindi, il [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java) viene utilizzato per eseguire le query GraphQL persistenti rispetto a AEM e caricare il contenuto dell’avventura nell’app.

Ogni query persistente ha una classe corrispondente &quot;loader&quot;, che chiama in modo asincrono il punto finale AEM HTTP GET e restituisce i dati dell’avventura utilizzando il valore personalizzato definito [modello dati](#data-models).

+ `loader/AdventuresLoader.java`

   Recupera l&#39;elenco delle Avventure nella schermata iniziale dell&#39;applicazione utilizzando `wknd-shared/adventures-all` query persistente.

+ `loader/AdventureLoader.java`

   Recupera una singola avventura selezionandola tramite il `slug` utilizzando `wknd-shared/adventure-by-slug` query persistente.

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

`Adventure.java` è un POJO Java inizializzato con i dati JSON della richiesta GraphQL e modella un’avventura da utilizzare nelle visualizzazioni dell’applicazione Android.

### Viste

L’applicazione Android utilizza due visualizzazioni per presentare i dati dell’avventura nell’esperienza mobile.

+ `AdventureListFragment.java`

   Richiama il `AdventuresLoader` e visualizza le avventure restituite in un elenco.

+ `AdventureDetailFragment.java`

   Richiama il `AdventureLoader` utilizzando `slug` Param passato attraverso la selezione dell&#39;avventura sul `AdventureListFragment` visualizza e visualizza i dettagli di una singola avventura.

### Immagini remote

`loader/RemoteImagesCache.java` è una classe di utilità che aiuta a preparare le immagini remote in una cache in modo che possano essere utilizzate con gli elementi dell&#39;interfaccia utente Android. Il contenuto dell’avventura fa riferimento alle immagini in AEM Assets tramite un URL e questa classe viene utilizzata per visualizzare tale contenuto.

## Altro materiale di riferimento

+ [Guida introduttiva a AEM Headless - Esercitazione GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java)
