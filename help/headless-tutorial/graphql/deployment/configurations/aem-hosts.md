---
title: Gestione degli host AEM per AEM GraphQL
description: Scopri come configurare gli host AEM nell’app headless di AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 0%

---

# Gestione degli host AEM

La distribuzione di un’applicazione AEM Headless richiede attenzione alla modalità di creazione degli URL AEM per garantire l’utilizzo corretto dell’host o del dominio AEM. I tipi di URL/richiesta principali di cui tenere conto sono:

+ Richieste HTTP a __[API AEM GraphQL](#aem-graphql-api-requests)__
+ __[URL immagine](#aem-image-urls)__ per le risorse immagine a cui si fa riferimento nei frammenti di contenuto e consegnati da AEM

In genere, un’app AEM Headless interagisce con un singolo servizio AEM sia per le richieste API che per quelle di immagini di GraphQL. Il servizio AEM cambia in base all’implementazione dell’app AEM Headless:

| Tipo di implementazione AEM headless | Ambiente AEM | Servizio AEM |
|-------------------------------|:---------------------:|:----------------:|
| Produzione | Produzione | Pubblicazione |
| Anteprima di authoring | Produzione | Anteprima |
| Ambiente di sviluppo | Ambiente di sviluppo | Pubblicazione |

Per gestire le permutazioni del tipo di distribuzione, ogni distribuzione di app viene creata utilizzando una configurazione che specifica il servizio AEM a cui connettersi. L’host/dominio del servizio AEM configurato viene quindi utilizzato per creare gli URL API e gli URL immagine di AEM GraphQL. Per determinare l’approccio corretto per la gestione delle configurazioni dipendenti dalla build, fai riferimento alla documentazione del framework dell’app AEM Headless (ad esempio, React, iOS, Android™ e così via), in quanto l’approccio varia in base al framework.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configurazione degli host AEM | ✔ | ✔ | ✔ | ✔ |

Di seguito sono riportati alcuni esempi di possibili approcci per la costruzione di URL per [API GraphQL di AEM](#aem-graphql-api-requests) e [richieste di immagini](#aem-image-requests), per diversi framework e piattaforme headless popolari.

## Richieste API di AEM GraphQL

Le richieste HTTP GET dall&#39;app headless alle API GraphQL di AEM devono essere configurate per interagire con il servizio AEM corretto, come descritto nella [tabella precedente](#managing-aem-hosts).

Quando si utilizzano [SDK AEM Headless](../../how-to/aem-headless-sdk.md) (disponibili per JavaScript basato su browser, JavaScript basato su server e Java™), un host AEM può inizializzare l&#39;oggetto client AEM Headless con il servizio AEM con cui connettersi.

Quando sviluppi un client AEM Headless personalizzato, assicurati che l’host del servizio AEM sia parametrizzabile in base ai parametri di build.

### Esempi

Di seguito sono riportati alcuni esempi di come le richieste API di AEM GraphQL possono rendere configurabile il valore host di AEM per vari framework di app headless.

+++ Esempio di React

Questo esempio, basato vagamente sull&#39;[app AEM Headless React](../../example-apps/react-app.md), illustra come configurare le richieste API di AEM GraphQL per la connessione a diversi servizi AEM in base alle variabili di ambiente.

Le app React devono utilizzare il [client AEM headless per JavaScript](../../how-to/aem-headless-sdk.md) per interagire con le API GraphQL di AEM. Il client AEM Headless, fornito dal client AEM Headless per JavaScript, deve essere inizializzato con l’host del servizio AEM a cui si connette.

#### File di ambiente React

React utilizza [file di ambiente personalizzati](https://create-react-app.dev/docs/adding-custom-environment-variables/), o `.env` file, memorizzati nella directory principale del progetto per definire valori specifici della build. Ad esempio, il file `.env.development` contiene i valori utilizzati per durante lo sviluppo, mentre `.env.production` contiene i valori utilizzati per le build di produzione.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

È possibile specificare `.env` file per altri usi [&#128279;](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) tramite il postfix di `.env` e un descrittore semantico, ad esempio `.env.stage` o `.env.production`. È possibile utilizzare file `.env` diversi durante l&#39;esecuzione o la creazione dell&#39;app React impostando `REACT_APP_ENV` prima di eseguire un comando `npm`.

Ad esempio, `package.json` di un&#39;app React può contenere la seguente configurazione `scripts`:

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

Il [client AEM Headless per JavaScript](../../how-to/aem-headless-sdk.md) contiene un client AEM Headless che effettua richieste HTTP alle API GraphQL di AEM. Il client AEM Headless deve essere inizializzato con l&#39;host AEM con cui interagisce, utilizzando il valore del file `.env` attivo.

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

#### React useEffect(..) hook

Gli hook useEffect personalizzati di React chiamano il client AEM Headless, inizializzato con l’host AEM, per conto del componente React che esegue il rendering della visualizzazione.

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

L&#39;hook useEffect personalizzato, `useAdventureByPath`, viene importato e utilizzato per ottenere i dati utilizzando il client AEM Headless ed eseguire il rendering del contenuto per l&#39;utente finale.

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

Questo esempio, basato sull&#39;[app AEM Headless iOS™](../../example-apps/ios-swiftui-app.md), illustra come configurare le richieste API di AEM GraphQL per connettersi a diversi host AEM in base a [variabili di configurazione specifiche della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Le app iOS™ richiedono un client AEM Headless personalizzato per interagire con le API GraphQL di AEM. Il client AEM Headless deve essere scritto in modo che l’host del servizio AEM sia configurabile.

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

L&#39;app iOS headless AEM [&#128279;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) di esempio  utilizza un client AEM headless personalizzato inizializzato con i valori di configurazione per `AEM_SCHEME` e `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

Il client AEM headless personalizzato (`api/Aem.swift`) contiene un metodo `makeRequest(..)` che aggiunge il prefisso delle richieste API di AEM GraphQL alle configurazioni di AEM `scheme` e `host`.

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

[È possibile creare nuovi file di configurazione della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) per connettersi a diversi servizi di AEM. I valori specifici della build per `AEM_SCHEME` e `AEM_HOST` vengono utilizzati in base alla build selezionata in XCode, determinando la connessione del client AEM Headless personalizzato con il servizio AEM corretto.

+++

+++ Android™ esempio

Questo esempio, basato sull&#39;[app AEM Headless Android™](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustra come configurare le richieste API di AEM GraphQL per connettersi a diversi servizi AEM in base a variabili di configurazione specifiche della build (o versioni).

Le app Android™ (se scritte in Java™) devono utilizzare il [client AEM headless per Java™](https://github.com/adobe/aem-headless-client-java) per interagire con le API GraphQL di AEM. Il client AEM Headless, fornito da AEM Headless Client per Java™, deve essere inizializzato con l’host del servizio AEM a cui si connette.

#### File di configurazione della build

Le app Android™ definiscono i &quot;productFlavors&quot; utilizzati per creare artefatti per usi diversi.
In questo esempio viene illustrato come è possibile definire due versioni di prodotto Android™, fornendo diversi valori host del servizio AEM (`AEM_HOST`) per gli utilizzi di sviluppo (`dev`) e produzione (`prod`).

Nel file `build.gradle` dell&#39;app, viene creato un nuovo `flavorDimension` denominato `env`.

Nella dimensione `env` sono definiti due `productFlavors`: `dev` e `prod`. Ogni `productFlavor` utilizza `buildConfigField` per impostare variabili specifiche della build che definiscono il servizio AEM a cui connettersi.

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

Inizializza il generatore `AEMHeadlessClient`, fornito dal client headless AEM per Java™ con il valore `AEM_HOST` dal campo `buildConfigField`.

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

Quando crei l&#39;app Android™ per usi diversi, specifica il sapore `env` e viene utilizzato il valore host AEM corrispondente.

+++

## URL immagine AEM

Le richieste di immagini dall&#39;app headless ad AEM devono essere configurate per interagire con il servizio AEM corretto, come descritto nella [tabella precedente](#managing-aem-hosts).

Adobe consiglia di utilizzare [immagini ottimizzate](../../how-to/images.md) rese disponibili tramite il campo `_dynamicUrl` nelle API GraphQL di AEM. Il campo `_dynamicUrl` restituisce un URL senza host che può essere preceduto dall&#39;host del servizio AEM utilizzato per eseguire query sulle API di AEM GraphQL. Il campo `_dynamicUrl` nella risposta di GraphQL si presenta come:

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Esempi

Di seguito sono riportati alcuni esempi di come gli URL delle immagini possono anteporre il valore host di AEM, reso configurabile per vari framework di app headless. Gli esempi presuppongono l&#39;utilizzo di query GraphQL che restituiscono riferimenti immagine utilizzando il campo `_dynamicUrl`.

Ad esempio:

#### Query persistente GraphQL

Questa query GraphQL restituisce un riferimento a un&#39;immagine `_dynamicUrl`. Come visto nella [risposta GraphQL](#examples-react-graphql-response) che esclude un host.

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

Questa risposta di GraphQL restituisce il riferimento all&#39;immagine `_dynamicUrl`, che esclude un host.

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

Questo esempio, basato sull&#39;[app AEM Headless React](../../example-apps/react-app.md), illustra come configurare gli URL delle immagini per connettersi ai servizi AEM corretti in base alle variabili di ambiente.

In questo esempio viene illustrato il prefisso del campo di riferimento immagine `_dynamicUrl`, con una variabile di ambiente React configurabile `REACT_APP_AEM_HOST`.

#### File di ambiente React

React utilizza [file di ambiente personalizzati](https://create-react-app.dev/docs/adding-custom-environment-variables/), o `.env` file, memorizzati nella directory principale del progetto per definire valori specifici della build. Ad esempio, il file `.env.development` contiene i valori utilizzati per durante lo sviluppo, mentre `.env.production` contiene i valori utilizzati per le build di produzione.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

È possibile specificare `.env` file per altri usi [&#128279;](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) tramite il postfix di `.env` e un descrittore semantico, ad esempio `.env.stage` o `.env.production`. È possibile utilizzare un file `.env` diverso durante l&#39;esecuzione o la creazione dell&#39;app React impostando `REACT_APP_ENV` prima di eseguire un comando `npm`.

Ad esempio, `package.json` di un&#39;app React può contenere la seguente configurazione `scripts`:

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

Il componente React importa la variabile di ambiente `REACT_APP_AEM_HOST` e aggiunge il prefisso `_dynamicUrl` all&#39;immagine per fornire un URL immagine completamente risolvibile.

La stessa variabile di ambiente `REACT_APP_AEM_HOST` viene utilizzata per inizializzare il client AEM Headless utilizzato dall&#39;hook useEffect personalizzato `useAdventureByPath(..)` utilizzato per recuperare i dati GraphQL da AEM. Utilizzando la stessa variabile per creare la richiesta API GraphQL dell’URL dell’immagine, assicurati che l’app React interagisca con lo stesso servizio AEM in entrambi i casi d’uso.

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

Questo esempio, basato sull&#39;[app AEM Headless iOS™](../../example-apps/ios-swiftui-app.md), illustra come configurare gli URL dell&#39;immagine AEM per connettersi a diversi host AEM in base a [variabili di configurazione specifiche della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

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

In `Aem.swift`, l&#39;implementazione client headless AEM personalizzata, una funzione personalizzata `imageUrl(..)` prende il percorso dell&#39;immagine come specificato nel campo `_dynamicUrl` nella risposta di GraphQL e lo precede con l&#39;host di AEM. Questa funzione viene quindi richiamata nelle visualizzazioni di iOS ogni volta che viene eseguito il rendering di un’immagine.

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

La vista iOS e il prefisso del valore dell&#39;immagine `_dynamicUrl` forniscono un URL immagine completamente risolvibile.

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

[È possibile creare nuovi file di configurazione della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) per connettersi a diversi servizi di AEM. I valori specifici della build per `AEM_SCHEME` e `AEM_HOST` vengono utilizzati in base alla build selezionata in Xcode, consentendo al client AEM Headless personalizzato di interagire con il servizio AEM corretto.

+++

+++ Android™ esempio

Questo esempio, basato sull&#39;[app AEM Headless Android™](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustra come gli URL dell&#39;immagine AEM possono essere configurati per connettersi a diversi servizi AEM in base a variabili di configurazione specifiche della build (o versioni).

#### File di configurazione della build

Le app Android™ definiscono &quot;productFlavors&quot; che vengono utilizzati per creare artefatti per usi diversi.
In questo esempio viene illustrato come è possibile definire due versioni di prodotto Android™, fornendo diversi valori host del servizio AEM (`AEM_HOST`) per gli utilizzi di sviluppo (`dev`) e produzione (`prod`).

Nel file `build.gradle` dell&#39;app, viene creato un nuovo `flavorDimension` denominato `env`.

Nella dimensione `env` sono definiti due `productFlavors`: `dev` e `prod`. Ogni `productFlavor` utilizza `buildConfigField` per impostare variabili specifiche della build che definiscono il servizio AEM a cui connettersi.

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

#### Caricamento dell’immagine AEM

Android™ utilizza `ImageGetter` per recuperare e memorizzare nella cache locale i dati immagine da AEM. In `prepareDrawableFor(..)` l&#39;host del servizio AEM, definito nella configurazione di compilazione attiva, viene utilizzato per prefissare il percorso dell&#39;immagine creando un URL risolvibile in AEM.

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

La visualizzazione Android™ ottiene i dati dell&#39;immagine tramite `RemoteImagesCache` utilizzando il valore `_dynamicUrl` della risposta di GraphQL.

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

Quando crei l&#39;app Android™ per usi diversi, specifica il sapore `env` e viene utilizzato il valore host AEM corrispondente.

+++
