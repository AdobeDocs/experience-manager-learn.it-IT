---
title: App Android - AEM esempio headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Viene fornita un'applicazione Android che dimostra come eseguire query sul contenuto utilizzando le API GraphQL di AEM. Il client Android Apollo viene utilizzato per generare le query GraphQL e mappare i dati sugli oggetti Swift per alimentare l’app. SwiftUI viene utilizzato per eseguire il rendering di una semplice visualizzazione elenco e dettagli del contenuto.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 2%

---


# App Android

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Viene fornita un&#39;applicazione Android che dimostra come eseguire query sul contenuto utilizzando le API GraphQL di AEM. La [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java) viene utilizzato per eseguire le query GraphQL e mappare i dati su oggetti Java per alimentare l&#39;app.

Visualizza la [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

* [Android Studio](https://developer.android.com/studio)
* [Git](https://git-scm.com/)

## Requisiti AEM

L&#39;applicazione è progettata per connettersi a un AEM **Pubblica** ambiente con la versione più recente del [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) installato.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=it)

Consigliamo [distribuzione del sito WKND Reference in un ambiente di Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Impostazione locale tramite [l’SDK di AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) o [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) può essere utilizzato anche.

## Come utilizzare

1. Clona il `aem-guides-wknd-graphql` archivio:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) e apri la cartella `android-app`
1. Modificare il file `config.properties` a `app/src/main/assets/config.properties` e aggiorna `contentApi.endpoint` per far corrispondere l’ambiente AEM di destinazione:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Scaricare un [Dispositivo virtuale Android](https://developer.android.com/studio/run/managing-avds) (API min 28)
1. Crea e distribuisci l’app utilizzando l’emulatore Android.


### Connessione ad ambienti AEM

`10.0.2.2` è un [alias speciale](https://developer.android.com/studio/run/emulator-networking) per localhost quando si utilizza l&#39;emulatore. Quindi `10.0.2.2:4502` equivale a `localhost:4502`. Se ci si connette a un ambiente di pubblicazione AEM (consigliato), non è necessaria alcuna autorizzazione e `contentAPi.user` e `contentApi.password` può essere lasciato vuoto.

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

### Recupero contenuto

La [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java) viene utilizzato dall’app per eseguire la query GraphQL su AEM e caricare il contenuto dell’avventura nell’app.

`AdventuresLoader.java` è il file che recupera e carica l’elenco iniziale di Avventure nella schermata iniziale dell’applicazione. Utilizza [Query persistenti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) che [preconfezionato](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) con il sito di riferimento WKND. L&#39;endpoint è `/wknd/adventures-all`. `AEMHeadlessClientBuilder` crea un&#39;istanza nuova in base all&#39;endpoint api impostato in `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` è il file che recupera e carica il contenuto Avventura per ciascuna visualizzazione di dettaglio. Ancora una volta `AEMHeadlessClient` viene utilizzato per eseguire la query. Viene eseguita una normale query GraphQL in base al percorso del frammento di contenuto Adventure. La query viene mantenuta in un file separato denominato [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` è un POJO che viene inizializzato con i dati JSON dalla richiesta GraphQL.

`RemoteImagesCache.java` è una classe di utilità che aiuta a preparare le immagini remote in una cache in modo che possano essere utilizzate con gli elementi dell&#39;interfaccia utente Android. Il contenuto Adventure fa riferimento alle immagini in AEM Assets tramite un URL e questa classe viene utilizzata per visualizzare tale contenuto.

### Viste

`AdventureListFragment.java` - quando viene chiamato , viene attivato il `AdventuresLoader` e visualizza le avventure restituite in un elenco.

`AdventureDetailFragment.java` - inizializza `AdventureLoader` e visualizza i dettagli di una singola avventura.

## Risorse aggiuntive

* [Guida introduttiva a AEM Headless - Esercitazione GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [Client AEM headless per Java](https://github.com/adobe/aem-headless-client-java)

