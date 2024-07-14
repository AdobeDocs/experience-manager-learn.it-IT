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
duration: 278
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# app iOS

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione iOS illustra come eseguire query sui contenuti che utilizzano le API GraphQL dell’AEM utilizzando query persistenti.

![App iOS SwiftUI con AEM headless](./assets/ios-swiftui-app/ios-app.png)

Visualizza il [codice sorgente in GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Prerequisiti {#prerequisites}

I seguenti strumenti devono essere installati localmente:

+ [Xcode](https://developer.apple.com/xcode/) (richiede macOS)
+ [Git](https://git-scm.com/)

## Requisiti AEM

L’applicazione iOS funziona con le seguenti opzioni di distribuzione dell’AEM. Tutte le distribuzioni richiedono l&#39;installazione del sito [WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest).

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configurazione locale con [SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it)

L&#39;applicazione iOS è progettata per connettersi a un ambiente __AEM Publish__, tuttavia può creare contenuto da AEM Author se l&#39;autenticazione viene fornita nella configurazione dell&#39;applicazione iOS.

## Come usare

1. Clona l&#39;archivio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri [Xcode](https://developer.apple.com/xcode/) e la cartella `ios-app`
1. Modificare il file `Config.xcconfig` e aggiornare `AEM_SCHEME` e `AEM_HOST` in modo che corrispondano al servizio Publish AEM di destinazione.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Se ci si connette a AEM Author, aggiungere `AEM_AUTH_TYPE` e le proprietà di autenticazione di supporto a `Config.xcconfig`.

   __Autenticazione di base__

   `AEM_USERNAME` e `AEM_PASSWORD` autenticano un utente AEM locale con accesso al contenuto GraphQL WKND.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticazione token__

   `AEM_TOKEN` è un [token di accesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) che esegue l&#39;autenticazione a un utente AEM con accesso al contenuto WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Crea l’applicazione utilizzando Xcode e distribuisci l’app sul simulatore iOS
1. Nell’applicazione deve essere visualizzato un elenco di avventure dal sito WKND. Selezionando un’avventura si aprono i relativi dettagli. Nella vista a elenco avventure, seleziona per aggiornare i dati dall’AEM.

## Il codice

Di seguito è riportato un riepilogo di come viene creata l’applicazione iOS, di come si connette a AEM Headless per recuperare contenuti utilizzando query persistenti di GraphQL e di come vengono presentati tali dati. Il codice completo si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Query persistenti

Seguendo le best practice di AEM Headless, l’applicazione iOS utilizza query persistenti AEM GraphQL per eseguire query sui dati di avventura. L’applicazione utilizza due query persistenti:

+ Query persistente `wknd/adventures-all`, che restituisce tutte le avventure in AEM con un set abbreviato di proprietà. Questa query persistente guida l’elenco di avventure della visualizzazione iniziale.

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

+ Query persistente `wknd/adventure-by-slug`, che restituisce una singola avventura di `slug` (una proprietà personalizzata che identifica in modo univoco un&#39;avventura) con un set completo di proprietà. Questa query persistente attiva le visualizzazioni dei dettagli dell’avventura.

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

Le query persistenti dell’AEM vengono eseguite su HTTP GET e pertanto non è possibile utilizzare le librerie GraphQL comuni che utilizzano HTTP POST, come Apollo. Creare invece una classe personalizzata che esegua le richieste HTTP di query persistenti a AEM GET.

`AEM/Aem.swift` crea un&#39;istanza della classe `Aem` utilizzata per tutte le interazioni con AEM Headless. Il pattern è:

1. A ogni query persistente corrisponde un func pubblico (ad es. `getAdventures(..)` o `getAdventureBySlug(..)`) le visualizzazioni dell&#39;applicazione iOS vengono richiamate per ottenere i dati relativi all&#39;avventura.
1. Il func pubblico chiama un func privato `makeRequest(..)` che richiama una richiesta HTTP GET asincrona a AEM Headless e restituisce i dati JSON.
1. Ogni funzione pubblica decodifica quindi i dati JSON ed esegue tutte le verifiche o trasformazioni necessarie, prima di restituire i dati di Adventure alla visualizzazione.

   + I dati JSON GraphQL dell&#39;AEM vengono decodificati utilizzando le strutture/classi definite in `AEM/Models.swift`, che corrispondono agli oggetti JSON restituiti dall&#39;AEM headless.

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

`src/AEM/Models.swift` definisce le [strutture e classi Swift decodificabili](https://developer.apple.com/documentation/swift/decodable) mappate alle risposte JSON AEM restituite dalle risposte JSON AEM.

### Viste

SwiftUI viene utilizzato per le varie visualizzazioni nell’applicazione. Apple fornisce un tutorial introduttivo per [creare elenchi e navigare con SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  La voce dell&#39;applicazione e include `AdventureListView` il cui gestore eventi `.onAppear` è utilizzato per recuperare tutti i dati di Adventures tramite `aem.getAdventures()`. L&#39;oggetto `aem` condiviso è inizializzato qui ed esposto ad altre visualizzazioni come [OggettoAmbiente](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Visualizza un elenco di avventure (in base ai dati di `aem.getAdventures()`) e visualizza una voce di elenco per ogni avventura utilizzando `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  Visualizza ogni elemento nell&#39;elenco Avventure (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Visualizza i dettagli di un&#39;avventura, tra cui il titolo, la descrizione, il prezzo, il tipo di attività e l&#39;immagine primaria. Questa visualizzazione richiede all&#39;AEM i dettagli completi dell&#39;avventura utilizzando `aem.getAdventureBySlug(slug: slug)`, dove il parametro `slug` viene passato in base alla riga dell&#39;elenco di selezione.

### Immagini remote

Le immagini a cui si fa riferimento nei frammenti di contenuto dell’avventura sono servite dall’AEM. Questa app iOS utilizza il campo percorso `_dynamicUrl` nella risposta di GraphQL e aggiunge i prefissi `AEM_SCHEME` e `AEM_HOST` per creare un URL completo. Se si sviluppa in base all&#39;SDK AE, `_dynamicUrl` restituisce null, quindi per lo sviluppo il fallback al campo `_path` dell&#39;immagine.

Se ti connetti a risorse protette su AEM che richiedono un’autorizzazione, è necessario aggiungere le credenziali anche alle richieste di immagini.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) e [SDWebImage](https://github.com/SDWebImage/SDWebImage) vengono utilizzati per caricare le immagini remote da AEM che popolano l&#39;immagine Adventure nelle visualizzazioni `AdventureListItemView` e `AdventureDetailView`.

La classe `aem` (in `AEM/Aem.swift`) facilita l&#39;utilizzo delle immagini AEM in due modi:

1. `aem.imageUrl(path: String)` viene utilizzato nelle visualizzazioni per anteporre lo schema AEM e ospitare il percorso dell&#39;immagine, creando un URL completo.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. `convenience init(..)` in `Aem` ha impostato le intestazioni di autorizzazione HTTP nella richiesta HTTP dell&#39;immagine, in base alla configurazione delle applicazioni iOS.

   + Se è configurata l&#39;__autenticazione di base__, l&#39;autenticazione di base viene associata a tutte le richieste di immagini.

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

   + Se è configurata l&#39;autenticazione __token__, l&#39;autenticazione token viene associata a tutte le richieste di immagini.

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

   + Se __non è configurata alcuna autenticazione__, non verrà associata alcuna autenticazione alle richieste di immagini.

Un approccio simile può essere utilizzato con [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) nativo per SwiftUI. `AsyncImage` è supportato in iOS 15.0+.

## Risorse aggiuntive

+ [Guida introduttiva di AEM headless - Tutorial GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=it)
+ [Elenchi SwiftUI e tutorial di navigazione](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
