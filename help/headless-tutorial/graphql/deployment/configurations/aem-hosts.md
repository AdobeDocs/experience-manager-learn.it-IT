---
title: Gestione degli host AEM per AEM GraphQL
description: Scopri come configurare gli host AEM nell’app AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 632
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 0%

---

# Gestione degli host AEM

La distribuzione di un’applicazione AEM Headless richiede attenzione sul modo in cui gli URL dell’AEM vengono costruiti per garantire l’utilizzo corretto dell’host/dominio AEM. I tipi di URL/richiesta principali di cui tenere conto sono:

+ richieste HTTP a __[API GraphQL dell’AEM](#aem-graphql-api-requests)__
+ __[URL immagine](#aem-image-urls)__ per visualizzare le risorse a cui si fa riferimento nei frammenti di contenuto e consegnate da AEM

In genere, un’app AEM headless interagisce con un singolo servizio AEM sia per le richieste API che di immagini di GraphQL. Il servizio AEM cambia in base all’implementazione dell’app headless AEM:

| Tipo di implementazione AEM headless | Ambiente AEM | Servizio AEM |
|-------------------------------|:---------------------:|:----------------:|
| Produzione | Produzione | Pubblicazione |
| Anteprima di authoring | Produzione | Anteprima |
| Ambiente di sviluppo | Ambiente di sviluppo | Pubblicazione |

Per gestire le permutazioni del tipo di distribuzione, ogni distribuzione di app viene creata utilizzando una configurazione che specifica il servizio AEM a cui connettersi. L’host/dominio del servizio AEM configurato viene quindi utilizzato per creare gli URL API e gli URL immagine di GraphQL dell’AEM. Per determinare l’approccio corretto per la gestione delle configurazioni dipendenti dalla build, fai riferimento alla documentazione del framework dell’app headless AEM (ad esempio, React, iOS, Android™ e così via), in quanto l’approccio varia in base al framework.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configurazione degli host AEM | ✔ | ✔ | ✔ | ✔ |

Di seguito sono riportati alcuni esempi di possibili approcci per la costruzione di URL per [API GRAPHQL AEM](#aem-graphql-api-requests) e [richieste di immagini](#aem-image-requests), per diversi framework e piattaforme headless popolari.

## Richieste API GraphQL per AEM

Le richieste HTTP GET dall’app headless alle API GraphQL dell’AEM devono essere configurate per interagire con il servizio AEM corretto, come descritto nella [tabella precedente](#managing-aem-hosts).

Quando si utilizza [SDK di AEM headless](../../how-to/aem-headless-sdk.md) (disponibile per JavaScript basato su browser, JavaScript basato su server e Java™), un host AEM può inizializzare l’oggetto client AEM Headless con il servizio AEM con cui connettersi.

Quando sviluppi un client AEM headless personalizzato, assicurati che l’host del servizio AEM sia parametrizzabile in base ai parametri di build.

### Esempi

Di seguito sono riportati alcuni esempi di come le richieste API GraphQL di AEM possono rendere configurabile il valore host AEM per vari framework di app headless.

+++ Esempio di React

Questo esempio, liberamente basato su [App AEM Headless React](../../example-apps/react-app.md), illustra come configurare le richieste API di GraphQL per l’AEM per connettersi a diversi servizi AEM in base alle variabili di ambiente.

Le app React devono utilizzare il [Client AEM headless per JavaScript](../../how-to/aem-headless-sdk.md) interagire con le API GraphQL dell’AEM. Il client AEM headless, fornito dal client AEM headless per JavaScript, deve essere inizializzato con l’host del servizio AEM a cui si connette.

#### File di ambiente React

React utilizza [file di ambiente personalizzati](https://create-react-app.dev/docs/adding-custom-environment-variables/), o `.env` file, memorizzati nella directory principale del progetto per definire valori specifici della build. Ad esempio, il `.env.development` contiene i valori utilizzati per durante lo sviluppo, mentre `.env.production` contiene i valori utilizzati per le build di produzione.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` file per altri utilizzi [può essere specificato](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) con postfix `.env` e un descrittore semantico, ad esempio `.env.stage` o `.env.production`. Diverso `.env` I file possono essere utilizzati durante l’esecuzione o la creazione dell’app React, impostando `REACT_APP_ENV` prima di eseguire una `npm` comando.

Ad esempio, l&#39; `package.json` può contenere quanto segue `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### Client AEM headless

Il [Client AEM headless per JavaScript](../../how-to/aem-headless-sdk.md) contiene un client AEM headless che invia richieste HTTP alle API AEM GraphQL. Il client headless AEM deve essere inizializzato con l’host AEM con cui interagisce, utilizzando il valore della proprietà `.env` file.

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### React useEffect(..) gancio

Gli hook useEffect personalizzati di React chiamano il client headless AEM, inizializzato con l’host AEM, per conto del componente React che esegue il rendering della visualizzazione.

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### Componente React

Il gancio useEffect personalizzato, `useAdventureByPath` viene importato e utilizzato per ottenere i dati utilizzando il client headless AEM e in ultima analisi per eseguire il rendering del contenuto per l’utente finale.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™ esempio

Questo esempio, basato su [esempio di app iOS™ headless per AEM](../../example-apps/ios-swiftui-app.md), illustra come configurare le richieste API GraphQL di AEM per connettersi a diversi host AEM in base a [variabili di configurazione specifiche della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Le app iOS™ richiedono un client AEM headless personalizzato per interagire con le API AEM GraphQL. Il client headless AEM deve essere scritto in modo che l’host del servizio AEM sia configurabile.

#### Configurazione della build

Il file di configurazione Xcode contiene i dettagli di configurazione predefiniti.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Inizializzare il client headless AEM personalizzato

Il [esempio di app iOS headless per AEM](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) utilizza un client AEM headless personalizzato inizializzato con i valori di configurazione per `AEM_SCHEME` e `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

Client AEM headless personalizzato (`api/Aem.swift`) contiene un metodo `makeRequest(..)` che aggiunge il prefisso delle richieste API GraphQL dell’AEM alla configurazione AEM `scheme` e `host`.

+ `api/Aem.swift`

```swift
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
    
    return request;
}
```

[È possibile creare nuovi file di configurazione della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) per connettersi a diversi servizi dell’AEM. I valori specifici della build per `AEM_SCHEME` e `AEM_HOST` vengono utilizzati in base alla build selezionata in XCode, con conseguente client AEM headless personalizzato per la connessione al servizio AEM corretto.

+++

+++ Android™ esempio

Questo esempio, basato su [esempio di app Android™ headless per AEM](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustra come configurare le richieste API di GraphQL per l’AEM per connettersi a diversi servizi AEM in base a variabili di configurazione specifiche della build (o versioni).

Le app Android™ (se scritte in Java™) devono utilizzare [Client AEM headless per Java™](https://github.com/adobe/aem-headless-client-java) interagire con le API GraphQL dell’AEM. Il client AEM headless, fornito dal client AEM headless per Java™, deve essere inizializzato con l’host del servizio AEM a cui si connette.

#### File di configurazione della build

Le app Android™ definiscono i &quot;productFlavors&quot; utilizzati per creare artefatti per usi diversi.
Questo esempio mostra come definire due versioni di prodotto Android™, fornendo diversi host per il servizio AEM (`AEM_HOST`) valori per lo sviluppo (`dev`) e produzione (`prod`) utilizza.

Nel file dell’app `build.gradle` file, un nuovo `flavorDimension` denominato `env` viene creato.

In `env` dimensione, due `productFlavors` sono definiti: `dev` e `prod`. Ogni `productFlavor` utilizza `buildConfigField` per impostare variabili specifiche della build che definiscono il servizio AEM a cui connettersi.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Caricatore Android™

Inizializzare `AEMHeadlessClient` fornito dal client headless AEM per Java™ con `AEM_HOST` valore da `buildConfigField` campo.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

Quando crei l&#39;app Android™ per usi diversi, specifica `env` e viene utilizzato il corrispondente valore dell’host dell’AEM.

+++

## URL immagine AEM

Le richieste di immagini dall’app headless all’AEM devono essere configurate per interagire con il servizio AEM corretto, come descritto nella sezione [sopra la tabella](#managing-aem-hosts).

L’Adobe consiglia di utilizzare [immagini ottimizzate](../../how-to/images.md) resi disponibili tramite il `_dynamicUrl` nelle API GraphQL dell’AEM. Il `_dynamicUrl` restituisce un URL senza host che può essere preceduto dall’host del servizio AEM utilizzato per eseguire query sulle API GraphQL dell’AEM. Per `_dynamicUrl` nella risposta di GraphQL è simile al seguente:

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Esempi

Di seguito sono riportati alcuni esempi di come gli URL delle immagini possono anteporre il valore dell’host AEM in modo che sia configurabile per vari framework di app headless. Gli esempi presuppongono l’utilizzo di query GraphQL che restituiscono riferimenti a immagini utilizzando `_dynamicUrl` campo.

Ad esempio:

#### Query persistente GraphQL

Questa query GraphQL restituisce un riferimento immagine `_dynamicUrl`. Come mostrato nella [Risposta GraphQL](#examples-react-graphql-response) che esclude un host.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### Risposta GraphQL

Questa risposta di GraphQL restituisce i `_dynamicUrl` che esclude un host.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ Esempio di React

Questo esempio, basato su [esempio di app AEM Headless React](../../example-apps/react-app.md), illustra come configurare gli URL delle immagini per la connessione ai servizi AEM corretti in base alle variabili di ambiente.

Questo esempio mostra il prefisso del riferimento immagine `_dynamicUrl` con un campo configurabile `REACT_APP_AEM_HOST` Variabile di ambiente React.

#### File di ambiente React

React utilizza [file di ambiente personalizzati](https://create-react-app.dev/docs/adding-custom-environment-variables/), o `.env` file, memorizzati nella directory principale del progetto per definire valori specifici della build. Ad esempio, il `.env.development` contiene i valori utilizzati per durante lo sviluppo, mentre `.env.production` contiene i valori utilizzati per le build di produzione.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` file per altri utilizzi [può essere specificato](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) con postfix `.env` e un descrittore semantico, ad esempio `.env.stage` o `.env.production`. Diverso `.env` può essere utilizzato quando si esegue o si crea l’app React, impostando il `REACT_APP_ENV` prima di eseguire una `npm` comando.

Ad esempio, l&#39; `package.json` può contenere quanto segue `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### Componente React

Il componente React importa `REACT_APP_AEM_HOST` variabile di ambiente e utilizza i prefissi per l&#39;immagine `_dynamicUrl` per fornire un URL immagine completamente risolvibile.

Lo stesso `REACT_APP_AEM_HOST` variabile di ambiente utilizzata per inizializzare il client headless AEM utilizzato da `useAdventureByPath(..)` hook useEffect personalizzato utilizzato per recuperare i dati GraphQL dall’AEM. Utilizzando la stessa variabile per creare la richiesta API GraphQL dell’URL dell’immagine, assicurati che l’app React interagisca con lo stesso servizio AEM per entrambi i casi d’uso.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™ esempio

Questo esempio, basato su [esempio di app iOS™ headless per AEM](../../example-apps/ios-swiftui-app.md), illustra come configurare gli URL dell’immagine dell’AEM per connettersi a diversi host AEM in base a [variabili di configurazione specifiche della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Configurazione della build

Il file di configurazione Xcode contiene i dettagli di configurazione predefiniti.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Generatore URL immagine

In entrata `Aem.swift`, l’implementazione client headless AEM personalizzata, una funzione personalizzata `imageUrl(..)` prende il percorso dell&#39;immagine come fornito nella `_dynamicUrl` nella risposta di GraphQL e la precede con l’host AEM. Questa funzione viene quindi richiamata nelle visualizzazioni di iOS ogni volta che viene eseguito il rendering di un’immagine.

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### Vista iOS

La vista iOS e i prefissi dell&#39;immagine `_dynamicUrl` per fornire un URL immagine completamente risolvibile.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[È possibile creare nuovi file di configurazione della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) per connettersi a diversi servizi dell’AEM. I valori specifici della build per `AEM_SCHEME` e `AEM_HOST` vengono utilizzati in base alla build selezionata in XCode, con conseguente client AEM Headless personalizzato per interagire con il servizio AEM corretto.

+++

+++ Android™ esempio

Questo esempio, basato su [esempio di app Android™ headless per AEM](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustra come gli URL dell’immagine dell’AEM possono essere configurati per connettersi a diversi servizi AEM in base a variabili di configurazione specifiche della build (o versioni).

#### File di configurazione della build

Le app Android™ definiscono &quot;productFlavors&quot; che vengono utilizzati per creare artefatti per usi diversi.
Questo esempio mostra come definire due versioni di prodotto Android™, fornendo diversi host per il servizio AEM (`AEM_HOST`) valori per lo sviluppo (`dev`) e produzione (`prod`) utilizza.

Nel file dell’app `build.gradle` file, un nuovo `flavorDimension` denominato `env` viene creato.

In `env` dimensione, due `productFlavors` sono definiti: `dev` e `prod`. Ogni `productFlavor` utilizza `buildConfigField` per impostare variabili specifiche della build che definiscono il servizio AEM a cui connettersi.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Caricamento dell’immagine dell’AEM

Android™ utilizza un `ImageGetter` per recuperare e memorizzare nella cache locale i dati immagine dall’AEM. In entrata `prepareDrawableFor(..)` l’host del servizio AEM, definito nella configurazione della build attiva, viene utilizzato per anteporre il percorso dell’immagine creando un URL risolvibile all’AEM.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Vista Android™

La visualizzazione Android™ ottiene i dati dell’immagine tramite `RemoteImagesCache` utilizzando `_dynamicUrl` valore della risposta di GraphQL.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

Quando crei l&#39;app Android™ per usi diversi, specifica `env` e viene utilizzato il corrispondente valore dell’host dell’AEM.

+++
