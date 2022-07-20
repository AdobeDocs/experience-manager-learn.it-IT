---
title: App iOS - AEM esempio headless
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione iOS illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: cd7cb89f407f5e0c465544593563534472daf928
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 3%

---

# app iOS

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione iOS illustra come eseguire query sul contenuto utilizzando AEM API GraphQL utilizzando query persistenti.

![App iOS SwiftUI con AEM Headless](./assets/ios-swiftui-app/ios-app.png)

Visualizza la [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

+ [Xcode 9.3+](https://developer.apple.com/xcode/) (richiede macOS)
+ [Git](https://git-scm.com/)

## Requisiti AEM

L’applicazione iOS funziona con le seguenti opzioni di distribuzione AEM. Tutte le implementazioni richiedono l’ [Sito WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) da installare.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configurazione locale tramite [l’SDK di AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it?lang=en#install-local-aem-instances)

L’applicazione iOS è progettata per connettersi a un __Pubblicazione AEM__ Tuttavia, può generare contenuto da AEM Author se nella configurazione dell’applicazione iOS viene fornita l’autenticazione.

## Come utilizzare

1. Clona il `adobe/aem-guides-wknd-graphql` archivio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) e apri la cartella `ios-app`
1. Modificare il file `Config.xcconfig` file e aggiornamento `AEM_SCHEME` e `AEM_HOST` per corrispondere al servizio AEM Publish di destinazione.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   Se ti connetti ad AEM Author, aggiungi la `AEM_AUTH_TYPE` e supporto delle proprietà di autenticazione per `Config.xcconfig`.

   __Autenticazione di base__

   La `AEM_USERNAME` e `AEM_PASSWORD` autentica un utente AEM locale con accesso al contenuto WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticazione token__

   La `AEM_TOKEN` è un [token di accesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) che esegue l’autenticazione a un utente AEM con accesso al contenuto WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Crea l’applicazione utilizzando Xcode e distribuisci l’app al simulatore iOS
1. Nell’applicazione deve essere visualizzato un elenco delle avventure del sito WKND. Selezionando un’avventura si aprono i dettagli dell’avventura. Nella vista elenco Avventures, seleziona per aggiornare i dati da AEM.

## Il codice

Di seguito è riportato un riepilogo di come viene creata l&#39;applicazione iOS, di come si connette a AEM Headless per recuperare il contenuto utilizzando le query persistenti GraphQL e di come tali dati vengono presentati. Il codice completo è disponibile all&#39;indirizzo [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

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

### Esegui query persistente GraphQL

AEM le query persistenti vengono eseguite su HTTP GET, pertanto non è possibile utilizzare le librerie GraphQL comuni che utilizzano HTTP POST, come Apollo. Al contrario, crea una classe personalizzata che esegua le richieste HTTP di query persistenti a AEM.

`AEM/Aem.swift` crea un&#39;istanza del `Aem` Classe utilizzata per tutte le interazioni con AEM Headless. Il pattern è:

1. Ogni query persistente ha una funzione pubblica corrispondente (ad esempio. `getAdventures(..)` o `getAdventureBySlug(..)`) le visualizzazioni dell’applicazione iOS richiamano per ottenere i dati relativi all’avventura.
1. Il func pubblico chiama un func privato `makeRequest(..)` che richiama una richiesta HTTP GET asincrona a AEM Headless e restituisce i dati JSON.
1. Prima di restituire i dati di Adventure alla visualizzazione, ogni funzione pubblica decodifica i dati JSON ed esegue tutti i controlli o le trasformazioni necessarie.

+ AEM i dati JSON GraphQL vengono decodificati utilizzando gli structs/classes definiti in `AEM/Models.swift`, che viene mappata agli oggetti JSON che hanno restituito il mio AEM Headless.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
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

iOS preferisce la mappatura di oggetti JSON a modelli di dati tipizzati.

La `src/AEM/Models.swift` definisce il [decodificabile](https://developer.apple.com/documentation/swift/decodable) Le strutture e le classi Swift da mappare alle risposte JSON AEM restituite dalle risposte JSON AEM.

### Viste

SwiftUI viene utilizzato per le varie visualizzazioni nell’applicazione. Apple fornisce un tutorial introduttivo per [creazione di elenchi e navigazione con SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   L&#39;iscrizione della domanda e comprende `AdventureListView` di cui `.onAppear` il gestore eventi viene utilizzato per recuperare tutti i dati relativi alle avventure tramite `aem.getAdventures()`. Il `aem` oggetto inizializzato qui ed esposto ad altre viste come [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   Visualizza un elenco di avventure (in base ai dati di `aem.getAdventures()`) e visualizza una voce di elenco per ogni avventura che utilizza `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   Visualizza ogni elemento nell&#39;elenco delle avventure (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

   Visualizza i dettagli di un’avventura, inclusi il titolo, la descrizione, il prezzo, il tipo di attività e l’immagine primaria. Questa visualizzazione richiede AEM per dettagli completi sull&#39;avventura utilizzando `aem.getAdventureBySlug(slug: slug)`, dove `slug` viene passato in base alla riga dell’elenco di selezione.

### Immagini remote

Le immagini a cui fa riferimento Adventure Content Fragments sono servite da AEM. Questa app iOS utilizza il percorso `_path` nella risposta GraphQL e prefissa il `AEM_SCHEME` e `AEM_HOST` per creare un URL completo.

Se ci si connette a risorse protette su AEM che richiede un’autorizzazione, è necessario aggiungere le credenziali anche alle richieste di immagini.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) e [SDWebImage](https://github.com/SDWebImage/SDWebImage) vengono utilizzati per caricare le immagini remote da AEM che popolano l&#39;immagine Adventure sul `AdventureListItemView` e `AdventureDetailView` visualizzazioni.

La `aem` (in `AEM/Aem.swift`) facilita l&#39;utilizzo di immagini AEM in due modi:

1. `aem.imageUrl(path: String)` viene utilizzato nelle visualizzazioni per anteporre lo schema di AEM e ospitare il percorso dell&#39;immagine, creando un URL completo.

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. La `convenience init(..)` in `Aem` imposta le intestazioni di autorizzazione HTTP sulla richiesta HTTP immagine, in base alla configurazione delle applicazioni iOS.

   + Se __autenticazione di base__ viene configurata, quindi l’autenticazione di base viene associata a tutte le richieste di immagini.

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

   + Se __autenticazione token__ è configurata, quindi l’autenticazione token è associata a tutte le richieste di immagini.

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

   + Se __nessuna autenticazione__ è configurata, quindi non viene allegata alcuna autenticazione alle richieste di immagini.



Un approccio simile può essere utilizzato con l’interfaccia nativa SwiftUI [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` è supportato da iOS 15.0+.

## Altro materiale di riferimento

+ [Guida introduttiva a AEM Headless - Esercitazione GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Esercitazione sugli elenchi e sulla navigazione dell’interfaccia utente Swift](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
