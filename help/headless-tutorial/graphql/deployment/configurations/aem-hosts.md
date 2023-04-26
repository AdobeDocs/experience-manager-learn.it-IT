---
title: Gestione degli host AEM per AEM GraphQL
description: Scopri come configurare gli host AEM nell’app AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
source-git-commit: ec2609ed256ebe6cdd7935f3e8d476c1ff53b500
workflow-type: tm+mt
source-wordcount: '1669'
ht-degree: 1%

---

# Gestione degli host AEM

La distribuzione di un&#39;applicazione senza intestazione AEM richiede attenzione alla modalità di creazione degli URL AEM per garantire che venga utilizzato l&#39;host/dominio AEM corretto. I tipi di URL/richieste principali di cui tenere conto sono:

+ richieste HTTP a __[API di GraphQL AEM](#aem-graphql-api-requests)__
+ __[URL immagine](#aem-image-urls)__ alle risorse di immagini a cui si fa riferimento in Frammenti di contenuto e fornite da AEM

In genere, un’app AEM headless interagisce con un singolo servizio AEM sia per le richieste API GraphQL che per le richieste di immagini. Il servizio AEM cambia in base alla distribuzione dell&#39;app AEM Headless:

| Tipo di distribuzione headless AEM | ambiente AEM | AEM |
|-------------------------------|:---------------------:|:----------------:|
| Produzione | Produzione | Pubblicazione |
| Anteprima di authoring | Produzione | Anteprima |
| Ambiente di sviluppo | Ambiente di sviluppo | Pubblicazione |

Per gestire le permutazioni dei tipi di distribuzione, ogni implementazione dell’app viene creata utilizzando una configurazione che specifica il servizio AEM a cui connettersi. L’host o il dominio del servizio AEM configurato viene quindi utilizzato per creare gli URL API e gli URL immagine AEM GraphQL. Per determinare l&#39;approccio corretto per la gestione delle configurazioni dipendenti dalla build, fai riferimento alla documentazione del framework dell&#39;app AEM Headless (ad esempio, React, iOS, Android™ e così via), in quanto l&#39;approccio varia in base al framework.

| Tipo di client | [App a pagina singola (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Server-to-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configurazione AEM host | ↓ | ✔ | ✔ | ✔ |

Di seguito sono riportati alcuni esempi di possibili approcci per la costruzione di URL per [API GraphQL AEM](#aem-graphql-api-requests) e [richieste di immagini](#aem-image-requests), per diversi framework e piattaforme headless popolari.

## AEM richieste API GraphQL

Le richieste HTTP GET dall’app headless a AEM API GraphQL devono essere configurate per interagire con il servizio AEM corretto, come descritto in [tabella precedente](#managing-aem-hosts).

Quando utilizzi [SDK AEM headless](../../how-to/aem-headless-sdk.md) (disponibile per JavaScript basato su browser, JavaScript basato su server e Java™), un host AEM può inizializzare l&#39;oggetto client AEM Headless con il servizio AEM con cui connettersi.

Quando sviluppi un client AEM headless personalizzato, assicurati che l&#39;host del servizio AEM sia parametrizzabile in base ai parametri di creazione.

### Esempi

Di seguito sono riportati alcuni esempi di come AEM le richieste API GraphQL possono rendere configurabile il valore host AEM per diversi framework di app headless.

+++ React example

In questo esempio, puoi utilizzare [App AEM Headless React](../../example-apps/react-app.md), illustra come AEM le richieste API di GraphQL possono essere configurate per connettersi a diversi servizi AEM in base a variabili di ambiente.

Le app React devono utilizzare [Client AEM headless per JavaScript](../../how-to/aem-headless-sdk.md) per interagire con AEM API GraphQL. Il client AEM Headless, fornito dal client AEM Headless per JavaScript, deve essere inizializzato con l&#39;host del servizio AEM a cui si connette.

#### Reazione del file dell’ambiente

Reagisce agli usi [file di ambiente personalizzati](https://create-react-app.dev/docs/adding-custom-environment-variables/)oppure `.env` file memorizzati nella directory principale del progetto per definire valori specifici della build. Ad esempio, il `.env.development` il file contiene i valori utilizzati durante lo sviluppo, mentre `.env.production` contiene i valori utilizzati per le build di produzione.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` file per altri utilizzi [può essere specificato](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) mediante postfissazione `.env` e un descrittore semantico, come `.env.stage` o `.env.production`. Diverso `.env` I file possono essere utilizzati durante l&#39;esecuzione o la creazione dell&#39;app React, impostando la variabile `REACT_APP_ENV` prima di eseguire un `npm` comando.

Ad esempio, un’app React `package.json` possono contenere: `scripts` config:

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

#### AEM client headless

La [Client AEM headless per JavaScript](../../how-to/aem-headless-sdk.md) contiene un client AEM Headless che effettua richieste HTTP a AEM API GraphQL. Il client AEM Headless deve essere inizializzato con l&#39;host AEM con cui interagisce, utilizzando il valore dell&#39;attivo `.env` file.

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

I ganci di Custom React useEffect chiamano il client AEM Headless, inizializzato con l&#39;host AEM, per conto del componente React che esegue il rendering della visualizzazione.

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

Il gancio useEffect personalizzato, `useAdventureByPath` viene importato e utilizzato per ottenere i dati utilizzando il client AEM Headless e, in ultima analisi, per eseguire il rendering del contenuto per l’utente finale.

+ &quot;src/components/AdventureDetail.js&quot;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ Esempio iOS™

In questo esempio, in base al [esempio AEM applicazione iOS™ headless](../../example-apps/ios-swiftui-app.md), illustra come AEM le richieste API di GraphQL possono essere configurate per connettersi a diversi host AEM basati su [variabili di configurazione specifiche per la build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Le app iOS™ richiedono un client AEM headless personalizzato per interagire con AEM API GraphQL. Il client AEM Headless deve essere scritto in modo che l&#39;host del servizio AEM sia configurabile.

#### Configurazione della build

Il file di configurazione XCode contiene i dettagli di configurazione predefiniti.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Inizializzare il client AEM headless personalizzato

La [esempio AEM app iOS headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) utilizza un client headless personalizzato AEM inizializzato con i valori di configurazione per `AEM_SCHEME` e `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

Client AEM headless personalizzato (`api/Aem.swift`) contiene un metodo `makeRequest(..)` che esegue il prefix AEM richieste API di GraphQL con il AEM configurato `scheme` e `host`.

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

[È possibile creare nuovi file di configurazione della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) per connettersi a servizi AEM diversi. I valori specifici della build per `AEM_SCHEME` e `AEM_HOST` vengono utilizzati in base alla build selezionata in XCode, con conseguente connessione del client headless AEM personalizzato al servizio AEM corretto.

+++

+++ Esempio Android™

In questo esempio, in base al [esempio AEM app Android™ headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustra come AEM le richieste API di GraphQL possono essere configurate per connettersi a diversi servizi di AEM in base a variabili di configurazione specifiche per la build (o flavors).

Le app Android™ (se scritte in Java™) devono utilizzare i [Client AEM headless per Java™](https://github.com/adobe/aem-headless-client-java) per interagire con AEM API GraphQL. Il client AEM Headless, fornito dal client AEM Headless per Java™, deve essere inizializzato con l&#39;host del servizio AEM a cui si connette.

#### Crea file di configurazione

Le app Android™ definiscono &quot;productFlavors&quot; che vengono utilizzati per creare artefatti per diversi utilizzi.
Questo esempio mostra come è possibile definire due gusti di prodotto Android™, fornendo diversi host di servizio AEM (`AEM_HOST`) valori per lo sviluppo (`dev`) e la produzione (`prod`) utilizza .

Nell’app di `build.gradle` file, un nuovo `flavorDimension` denominato `env` viene creato.

In `env` dimensione, due `productFlavors` sono definiti: `dev` e `prod`. Ogni `productFlavor` utilizza `buildConfigField` per impostare variabili specifiche per la build che definiscono il servizio AEM a cui connettersi.

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

Inizializza `AEMHeadlessClient` builder, fornito dal AEM Headless Client per Java™ con `AEM_HOST` dal `buildConfigField` campo .

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

Quando crei l&#39;app Android™ per diversi utilizzi, specifica la variabile `env` flavor e viene utilizzato il corrispondente valore host AEM.

+++

## URL di immagini AEM

Le richieste di immagini dall’app headless a AEM devono essere configurate per interagire con il servizio AEM corretto, come descritto in [tabella precedente](#managing-aem-hosts).

Adobe consiglia di utilizzare [immagini ottimizzate](../../how-to/images.md) messi a disposizione tramite `_dynamicUrl` in AEM API GraphQL. La `_dynamicUrl` Il campo restituisce un URL senza host che può essere preceduto dall&#39;host del servizio AEM utilizzato per eseguire query AEM API GraphQL. Per `_dynamicUrl` nella risposta GraphQL si presenta così:

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Esempi

Di seguito sono riportati alcuni esempi di come gli URL immagine possono usare il prefisso AEM valore host configurato per vari framework di app headless. Gli esempi presuppongono l&#39;utilizzo di query GraphQL che restituiscono riferimenti di immagine utilizzando `_dynamicUrl` campo .

Ad esempio:

#### Query persistente GraphQL

Questa query GraphQL restituisce un riferimento immagine `_dynamicUrl`. Come visto nella [Risposta GraphQL](#examples-react-graphql-response) che esclude un host.

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

Questa risposta di GraphQL restituisce il riferimento immagine `_dynamicUrl` che esclude un host.

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

+++ React example

In questo esempio, in base al [esempio AEM applicazione Headless React](../../example-apps/react-app.md), illustra come è possibile configurare gli URL immagine per connettersi ai servizi AEM corretti in base a variabili di ambiente.

Questo esempio mostra il prefisso del riferimento all&#39;immagine `_dynamicUrl` campo, con un `REACT_APP_AEM_HOST` Reagisce alla variabile di ambiente.

#### Reazione del file dell’ambiente

Reagisce agli usi [file di ambiente personalizzati](https://create-react-app.dev/docs/adding-custom-environment-variables/)oppure `.env` file memorizzati nella directory principale del progetto per definire valori specifici della build. Ad esempio, il `.env.development` il file contiene i valori utilizzati durante lo sviluppo, mentre `.env.production` contiene i valori utilizzati per le build di produzione.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` file per altri utilizzi [può essere specificato](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) mediante postfissazione `.env` e un descrittore semantico, come `.env.stage` o `.env.production`. Diverso `.env` può essere utilizzato durante l&#39;esecuzione o la creazione dell&#39;app React, impostando `REACT_APP_ENV` prima di eseguire un `npm` comando.

Ad esempio, un’app React `package.json` possono contenere: `scripts` config:

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

Il componente React importa il `REACT_APP_AEM_HOST` e prefissa l&#39;immagine `_dynamicUrl` per fornire un URL immagine completamente risolvibile.

Questo `REACT_APP_AEM_HOST` La variabile di ambiente viene utilizzata per inizializzare il client AEM Headless utilizzato da `useAdventureByPath(..)` gancio useEffect personalizzato utilizzato per recuperare i dati GraphQL da AEM. Usando la stessa variabile per creare la richiesta API di GraphQL come URL immagine, accertati che l’app React interagisca con lo stesso servizio AEM per entrambi i casi d’uso.

+ &quot;src/components/AdventureDetail.js&quot;

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

+++ Esempio iOS™

In questo esempio, in base al [esempio AEM applicazione iOS™ headless](../../example-apps/ios-swiftui-app.md), illustra come AEM URL immagine possono essere configurati per connettersi a diversi host AEM basati su [variabili di configurazione specifiche per la build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Configurazione della build

Il file di configurazione XCode contiene i dettagli di configurazione predefiniti.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Generatore URL immagine

In `Aem.swift`, implementazione client headless personalizzata AEM una funzione personalizzata `imageUrl(..)` prende il percorso dell&#39;immagine come fornito nella `_dynamicUrl` nella risposta GraphQL e lo sostituisce con AEM host. Questa funzione viene quindi richiamata nelle visualizzazioni iOS ogni volta che viene eseguito il rendering di un’immagine.

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

La visualizzazione iOS e il prefix dell&#39;immagine `_dynamicUrl` per fornire un URL immagine completamente risolvibile.

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

[È possibile creare nuovi file di configurazione della build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) per connettersi a servizi AEM diversi. I valori specifici della build per `AEM_SCHEME` e `AEM_HOST` vengono utilizzati in base alla build selezionata in XCode, con conseguente interazione del client AEM headless personalizzato con il servizio AEM corretto.

+++

+++ Esempio Android™

In questo esempio, in base al [esempio AEM app Android™ headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustra come AEM URL immagine possono essere configurati per connettersi a servizi AEM diversi in base a variabili di configurazione specifiche per la build (o flavors).

#### Crea file di configurazione

Le app Android™ definiscono &quot;productFlavors&quot;, utilizzati per creare artefatti per diversi utilizzi.
Questo esempio mostra come è possibile definire due gusti di prodotto Android™, fornendo diversi host di servizio AEM (`AEM_HOST`) valori per lo sviluppo (`dev`) e la produzione (`prod`) utilizza .

Nell’app di `build.gradle` file, un nuovo `flavorDimension` denominato `env` viene creato.

In `env` dimensione, due `productFlavors` sono definiti: `dev` e `prod`. Ogni `productFlavor` utilizza `buildConfigField` per impostare variabili specifiche per la build che definiscono il servizio AEM a cui connettersi.

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

#### Caricamento dell&#39;immagine AEM

Android™ utilizza un `ImageGetter` per recuperare e memorizzare nella cache locale i dati immagine da AEM. In `prepareDrawableFor(..)` l&#39;host del servizio AEM, definito nella configurazione di compilazione attiva, viene utilizzato per prefisso al percorso dell&#39;immagine che crea un URL risolvibile da AEM.

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

La visualizzazione Android™ ottiene i dati immagine tramite `RemoteImagesCache` utilizzando `_dynamicUrl` dalla risposta GraphQL.

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

Quando crei l&#39;app Android™ per diversi utilizzi, specifica la variabile `env` flavor e viene utilizzato il corrispondente valore host AEM.

+++
