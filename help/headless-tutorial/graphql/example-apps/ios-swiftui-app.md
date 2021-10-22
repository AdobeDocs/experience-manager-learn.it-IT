---
title: App SwiftUI di iOS - AEM senza intestazione
description: Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Viene fornita un'applicazione iOS che dimostra come eseguire query sul contenuto utilizzando le API GraphQL di AEM. L’iOS client Apollo viene utilizzato per generare le query GraphQL e mappare i dati sugli oggetti Swift per alimentare l’app. SwiftUI viene utilizzato per eseguire il rendering di una semplice visualizzazione elenco e dettagli del contenuto.
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
source-wordcount: '960'
ht-degree: 1%

---


# App SwiftUI di iOS

Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione iOS illustra come eseguire query sul contenuto utilizzando le API GraphQL di AEM. L’iOS client Apollo viene utilizzato per generare le query GraphQL e mappare i dati sugli oggetti Swift per alimentare l’app. SwiftUI viene utilizzato per eseguire il rendering di una semplice visualizzazione elenco e dettagli del contenuto.

Visualizza la [codice sorgente su GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app)

>[!VIDEO](https://video.tv.adobe.com/v/338042/?quality=12&learn=on)

## Prerequisiti {#prerequisites}

È necessario installare localmente i seguenti strumenti:

* [Xcode 9.3+](https://developer.apple.com/xcode/)
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

1. Launch [Xcode](https://developer.apple.com/xcode/) e apri la cartella `ios-swiftui-app`
1. Modificare il file `Config.xcconfig` file e aggiornamento `AEM_HOST` per soddisfare le esigenze dell’ambiente di pubblicazione AEM di destinazione

   ```plain
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   // GraphQL Endpoint
   AEM_GRAPHQL_ENDPOINT = /content/cq:graphql/wknd/endpoint.json
   ```

1. Crea l’applicazione utilizzando Xcode e distribuisci l’app al simulatore iOS
1. Nell’applicazione deve essere visualizzato un elenco delle avventure del sito di riferimento WKND.

## Il codice

Di seguito è riportato un breve riepilogo dei file importanti e del codice utilizzati per alimentare l&#39;applicazione. Il codice completo è disponibile all&#39;indirizzo [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app).

### iOS Apollo

La [iOS Apollo](https://www.apollographql.com/docs/ios/) client viene utilizzato dall’app per eseguire la query GraphQL rispetto a AEM. Il funzionario [Tutorial su Apollo](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/) ha molti più dettagli su come installare e utilizzare.

`schema.json` è un file che rappresenta lo schema GraphQL da un ambiente AEM con il sito di riferimento WKND installato. `schema.json` è stato scaricato da AEM e aggiunto al progetto. Il client Apollo esamina tutti i file con l’estensione `.graphql` come parte di una fase di creazione personalizzata. Il client Apollo utilizza quindi l’ `schema.json` file ed eventuali `.graphql` query per generare automaticamente il file `API.swift`.

Questo fornisce all&#39;applicazione un modello fortemente tipizzato per eseguire la query e i modelli che rappresentano i risultati.

![Fase di compilazione personalizzata Xcode](assets/ios-swiftui-app/xcode-build-phase-apollo.png)

`AdventureList.graphql` contiene la query utilizzata per eseguire query sulle avventure:

```
query AdventureList
{
  adventureList {
    items {
      _path
      adventureTitle
      adventurePrice
      adventureActivity
      adventureDescription {
        plaintext
        markdown
      }
      adventureDifficulty
      adventureTripLength
      adventurePrimaryImage {
        ...on ImageRef {
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

`Network.swift` costruisce la `ApolloClient`. La `endpointURL` utilizzato è costruito leggendo i valori del `Config.xcconfig` file. Se desideri connetterti a un AEM **Autore** istanza e necessario per aggiungere intestazioni aggiuntive per l&#39;autenticazione, si desidera modificare il `ApolloClient` qui.

```swift
// Network.swift
private(set) lazy var apollo: ApolloClient = {
        // The cache is necessary to set up the store, which we're going to hand to the provider
        let cache = InMemoryNormalizedCache()
        let store = ApolloStore(cache: cache)
  
        let client = URLSessionClient()
        let provider = DefaultInterceptorProvider(client: client, shouldInvalidateClientOnDeinit: true, store: store)
        let url = Connection.baseURL // from Configx.xcconfig 

        // no additional headers, public instances by default require no additional authentication
        let requestChainTransport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)

        return ApolloClient(networkTransport: requestChainTransport,store: store)
    }()
}
```

### Dati avventura

L&#39;applicazione è progettata per visualizzare un elenco di Avventure e poi una visualizzazione dettagliata di ogni avventura.

`AdventuresDataModel.swift` è una classe che include una funzione `fetchAdventures()`. Questa funzione utilizza `ApolloClient` per eseguire la query. In una query riuscita l&#39;array dei risultati sarà di tipo `AdventureListQuery.Data.AdventureList.Item`, generato automaticamente da `API.swift` file.

```swift
func fetchAdventures() {
        Network.shared.apollo
            //AdventureListQuery() generated based on AdventureList.graphql file
           .fetch(query: AdventureListQuery()) { [weak self] result in
           
             guard let self = self else {
               return
             }
                   
             switch result {
             case .success(let graphQLResult):
                print("Success AdventureListQuery() from: \(graphQLResult.source)")

                if let adventureDataItems =  graphQLResult.data?.adventureList.items {
                    // map graphQL items to an array of Adventure objects
                    self.adventures = adventureDataItems.compactMap { Adventure(adventureData: $0!) }
                }
                ...
             }
           }
}
```

È possibile utilizzare `AdventureListQuery.Data.AdventureList.Item` direttamente per alimentare l&#39;applicazione. Tuttavia è molto possibile che alcuni dei dati siano incompleti e quindi alcune delle proprietà potrebbero essere nulle.

`Adventure.swift` è un modello personalizzato introdotto agisce come wrapper del modello generato da Apollo. `Adventure` è inizializzato con `AdventureListQuery.Data.AdventureList.Item`. A `typealias` viene utilizzato per accorciare per rendere il codice più leggibile:

```
// use typealias
typealias AdventureData = AdventureListQuery.Data.AdventureList.Item
```

La `Adventure` struct inizializzato con un `AdventureData` oggetto:

```swift
struct Adventure: Identifiable {
    let id: String
    let adventureTitle: String
    let adventurePrice: String
    let adventureDescription: String
    let adventureActivity: String
    let adventurePrimaryImageUrl: String
    
    // initialize with AdventureData object aka AdventureListQuery.Data.AdventureList.Item
    init(adventureData: AdventureData) {
        // use path as unique idenitifer, otherwise
        self.id = adventureData._path ?? UUID().uuidString
        self.adventureTitle = adventureData.adventureTitle ?? "Untitled"
        self.adventurePrice = adventureData.adventurePrice ?? "Free"
        self.adventureActivity = adventureData.adventureActivity ?? ""
        ...
```

Questo ci consente di fornire i valori predefiniti ed eseguire ulteriori controlli per i dati incompleti. A questo punto possiamo utilizzare il `Adventure` per alimentare in modo sicuro vari elementi dell&#39;interfaccia utente e non è necessario verificare costantemente la presenza di valori nulli.

In AEM, i contenuti sono identificati in modo univoco da `_path`. In `Adventure.swift` popoliamo i `id` con il valore di `_path`. Ciò consente `Adventure` per implementare `Identifiable` e facilita l&#39;iterazione su una matrice o un elenco.

### Viste

SwiftUI viene utilizzato per le varie visualizzazioni nell’applicazione. Un&#39;ottima esercitazione per [elenchi di edifici e navigazione](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation) disponibile sul sito per sviluppatori Apple. Il codice per questa applicazione viene derivato liberamente da essa.

`WKNDAdventuresApp.swift` è la voce della domanda. Include `AdventureListView` e `.onAppear` viene utilizzato per recuperare i dati dell’avventura.

`AdventureListView.swift` - crea un `NavigationView` e un elenco di avventure popolate da `AdventureRowView`. Navigazione a un `AdventureDetailView` è qui sopra.

`AdventureRowView` - visualizza l&#39;immagine principale dell&#39;Avventura e il Titolo dell&#39;Avventura in una riga.

`AdventureDetailView` - visualizza un dettaglio completo della singola avventura, compreso il titolo, la descrizione, il prezzo, il tipo di attività e l&#39;immagine primaria.

Quando Apollo CLI viene eseguito e rigenerato `API.swift` causa l&#39;interruzione dell&#39;anteprima. Per utilizzare la funzione Anteprima automatica, è necessario aggiornare il **Apollo CLI** Crea fase e controlla l&#39;esecuzione dello script **Solo per build di installazione**.

![Controlla solo le build di installazione](assets/ios-swiftui-app/update-build-phases.png)

### Immagini remote

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) e [SDWEbImage](https://github.com/SDWebImage/SDWebImage) vengono utilizzati per caricare le immagini remote da AEM che popolano l’immagine principale Avventura nelle visualizzazioni Riga e Dettaglio.

La [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) è una visualizzazione nativa SwiftUI che può essere utilizzata anche. `AsyncImage` è supportato solo per iOS 15.0+.

## Risorse aggiuntive

* [Guida introduttiva a AEM Headless - Esercitazione GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [Esercitazione sugli elenchi e sulla navigazione dell’interfaccia utente Swift](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
* [Tutorial del client Apollo iOS](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/)

