---
title: App iOS - Esempio di AEM headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione iOS illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM utilizzando query persistenti.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM headless as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 3%

---

# app iOS

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione iOS illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM utilizzando query persistenti.

![App iOS SwiftUI con AEM Headless](./assets/ios-swiftui-app/ios-app.png)

Visualizza [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [Xcode](https://developer.apple.com/xcode/) (richiede macOS)
+ [Git](https://git-scm.com/)

## Requisiti AEM

L’applicazione iOS funziona con le seguenti opzioni di distribuzione dell’AEM. Tutte le distribuzioni richiedono [Sito WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) da installare.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=it)
+ Configurazione locale con [l’SDK di AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it)

L’applicazione iOS è progettata per connettersi a un __Pubblicazione AEM__ Tuttavia, può originare il contenuto da AEM Author se l’autenticazione viene fornita nella configurazione dell’applicazione iOS.

## Come usare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) e apri la cartella `ios-app`
1. Modificare il file `Config.xcconfig` file e aggiornamento `AEM_SCHEME` e `AEM_HOST` affinché corrisponda al servizio di pubblicazione AEM di destinazione.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Se ti connetti a AEM Author, aggiungi `AEM_AUTH_TYPE` e il supporto delle proprietà di autenticazione `Config.xcconfig`.

   __Autenticazione di base__

   Il `AEM_USERNAME` e `AEM_PASSWORD` autentica un utente AEM locale con accesso al contenuto WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticazione token__

   Il `AEM_TOKEN` è un [token di accesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) che si autentica per un utente AEM con accesso al contenuto WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Crea l’applicazione utilizzando Xcode e distribuisci l’app sul simulatore iOS
1. Nell’applicazione deve essere visualizzato un elenco di avventure dal sito WKND. Selezionando un’avventura si aprono i relativi dettagli. Nella vista a elenco avventure, seleziona per aggiornare i dati dall’AEM.

## Il codice

Di seguito è riportato un riepilogo di come viene creata l’applicazione iOS, di come si connette a AEM Headless per recuperare contenuti utilizzando query persistenti di GraphQL e di come vengono presentati tali dati. Il codice completo è disponibile su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Query persistenti

Seguendo le best practice di AEM Headless, l’applicazione iOS utilizza query persistenti AEM GraphQL per eseguire query sui dati di avventura. L’applicazione utilizza due query persistenti:

+ `wknd/adventures-all` query persistente, che restituisce tutte le avventure in AEM con un set abbreviato di proprietà. Questa query persistente guida l’elenco di avventure della visualizzazione iniziale.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug` query persistente, che restituisce una singola avventura di `slug` (proprietà personalizzata che identifica in modo univoco un’avventura) con un set completo di proprietà. Questa query persistente attiva le visualizzazioni dei dettagli dell’avventura.

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

Le query persistenti AEM vengono eseguite su HTTP GET e pertanto non è possibile utilizzare le librerie GraphQL comuni che utilizzano HTTP POST, come Apollo. Creare invece una classe personalizzata che esegua le richieste HTTP di query persistenti a AEM GET.

`AEM/Aem.swift` crea un&#39;istanza di `Aem` classe utilizzata per tutte le interazioni con AEM Headless. Il pattern è:

1. A ogni query persistente corrisponde un func pubblico (ad es. `getAdventures(..)` o `getAdventureBySlug(..)`) le visualizzazioni dell’applicazione iOS richiamano per ottenere dati relativi all’avventura.
1. Il func pubblico chiama func privato `makeRequest(..)` che richiama una richiesta HTTP GET asincrona a AEM Headless e restituisce i dati JSON.
1. Ogni funzione pubblica decodifica quindi i dati JSON ed esegue tutte le verifiche o trasformazioni necessarie, prima di restituire i dati di Adventure alla visualizzazione.

   + I dati JSON di AEM GraphQL vengono decodificati utilizzando le strutture/classi definite in `AEM/Models.swift`, che si mappano agli oggetti JSON, ha restituito il mio oggetto AEM headless.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }
    
    ...

    /// #makeRequest(..)
    /// Generic method for constructing and executing AEM GraphQL persisted queries
    private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
        // Encode optional parameters as required by AEM
        let persistedQueryParams = params.map { (param) -> String in
            encode(string: ";\(param.key)=\(param.value)")
        }.joined(separator: "")
        
        // Construct the AEM GraphQL persisted query URL, including optional query params
        let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

        var request = URLRequest(url: URL(string: url)!);

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### Modelli di dati di risposta GraphQL

iOS preferisce mappare gli oggetti JSON ai modelli di dati tipizzati.

Il `src/AEM/Models.swift` definisce la [decodificabile](https://developer.apple.com/documentation/swift/decodable) Strutture e classi Swift mappate alle risposte JSON AEM restituite dalle risposte JSON AEM.

### Viste

SwiftUI viene utilizzato per le varie visualizzazioni nell’applicazione. Apple fornisce un tutorial introduttivo per [creazione di elenchi e navigazione con SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  L’iscrizione della domanda e comprende `AdventureListView` il cui `.onAppear` il gestore eventi viene utilizzato per recuperare tutti i dati di adventures tramite `aem.getAdventures()`. Il valore condiviso `aem` l&#39;oggetto viene inizializzato qui ed esposto ad altre viste come [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Visualizza un elenco di avventure (in base ai dati provenienti da `aem.getAdventures()`) e visualizza una voce di elenco per ogni avventura utilizzando `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  Visualizza ogni elemento nell&#39;elenco Avventure (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Visualizza i dettagli di un&#39;avventura, tra cui il titolo, la descrizione, il prezzo, il tipo di attività e l&#39;immagine primaria. Questa visualizzazione richiede all’AEM tutti i dettagli dell’avventura utilizzando `aem.getAdventureBySlug(slug: slug)`, in cui `slug` Il parametro viene trasmesso in base alla riga dell&#39;elenco di selezione.

### Immagini remote

Le immagini a cui si fa riferimento nei frammenti di contenuto dell’avventura sono servite dall’AEM. Questa app iOS utilizza il percorso `_dynamicUrl` nella risposta di GraphQL e aggiunge i prefissi `AEM_SCHEME` e `AEM_HOST` per creare un URL completo. Se si sviluppa rispetto all’SDK di AEM, `_dynamicUrl` restituisce null, pertanto per lo sviluppo il fallback su `_path` campo.

Se ti connetti a risorse protette su AEM che richiedono un’autorizzazione, è necessario aggiungere le credenziali anche alle richieste di immagini.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) e [SDWebImage](https://github.com/SDWebImage/SDWebImage) vengono utilizzati per caricare le immagini remote da AEM che popolano l’immagine Adventure sul `AdventureListItemView` e `AdventureDetailView` visualizzazioni.

Il `aem` classe (in `AEM/Aem.swift`) facilita l&#39;uso delle immagini dell&#39;AEM in due modi:

1. `aem.imageUrl(path: String)` viene utilizzato nelle visualizzazioni per anteporre lo schema AEM e ospitare il percorso dell’immagine, creando un URL completo.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. Il `convenience init(..)` in `Aem` imposta le intestazioni di autorizzazione HTTP sulla richiesta HTTP dell’immagine, in base alla configurazione delle applicazioni iOS.

   + Se __autenticazione di base__ è configurato, quindi l’autenticazione di base viene associata a tutte le richieste di immagini.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + Se __autenticazione token__ è configurato, quindi l’autenticazione token viene associata a tutte le richieste di immagini.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + Se __nessuna autenticazione__ è configurato, quindi non viene associata alcuna autenticazione alle richieste di immagini.

Un approccio simile può essere utilizzato con SwiftUI-native [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` è supportato in iOS 15.0+.

## Risorse aggiuntive

+ [Guida introduttiva di AEM headless - Tutorial GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=it)
+ [Elenchi e tutorial di navigazione di SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
