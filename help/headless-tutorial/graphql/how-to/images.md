---
title: Utilizzo di immagini ottimizzate con AEM headless
description: Scopri come richiedere URL di immagini ottimizzati con AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 377
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 5%

---

# Immagini ottimizzate con AEM headless {#images-with-aem-headless}

Le immagini sono un aspetto critico di [sviluppare esperienze AEM headless ricche e coinvolgenti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=it). AEM Headless supporta la gestione delle immagini e la loro distribuzione ottimizzata.

I frammenti di contenuto utilizzati nella modellazione di contenuti headless AEM spesso fanno riferimento a risorse di immagini destinate a essere visualizzate nell’esperienza headless. È possibile scrivere query AEM-GraphQL per fornire URL alle immagini in base alla provenienza dell’immagine.

Il `ImageRef` Il tipo dispone di quattro opzioni URL per i riferimenti ai contenuti:

+ `_path` è il percorso di riferimento nell’AEM e non include un’origine AEM (nome host)
+ `_dynamicUrl` è l’URL completo della risorsa immagine preferita ottimizzata per il web.
   + Il `_dynamicUrl` non include un’origine AEM, pertanto il dominio (servizio di authoring AEM o di pubblicazione AEM) deve essere fornito dall’applicazione client.
+ `_authorUrl` è l’URL completo della risorsa immagine nell’istanza di authoring AEM
   + [Autore AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) può essere utilizzato per fornire un’esperienza di anteprima dell’applicazione headless.
+ `_publishUrl` è l’URL completo della risorsa immagine nella pubblicazione AEM
   + [Pubblicazione AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) è in genere il punto da cui l’implementazione di produzione dell’applicazione headless visualizza le immagini.

Il `_dynamicUrl` è l’URL preferito da utilizzare per le risorse di immagini e deve sostituire l’utilizzo di `_path`, `_authorUrl`, e `_publishUrl` quando possibile.

|                                | AEM as a Cloud Service | AEM AS A CLOUD SERVICE RDE | SDK AEM | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| Supporta le immagini ottimizzate per il web? | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Immagini con AEM headless"
>abstract="Scopri in che modo AEM Headless supporta la gestione delle risorse di immagini e la loro consegna ottimizzata."

## Modello per frammenti di contenuto

Assicurati che il campo Frammento di contenuto contenente il riferimento all’immagine sia del __riferimento contenuto__ tipo di dati.

I tipi di campo vengono esaminati in [Modello per frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), selezionando il campo e controllando __Proprietà__ a destra.

![Modello per frammenti di contenuto con riferimento a un’immagine](./assets/images/content-fragment-model.jpeg)

## Query persistente GraphQL

Nella query GraphQL, restituisce il campo come `ImageRef` digitare e richiedere il `_dynamicUrl` campo. Ad esempio, eseguire una query su un’avventura in [Progetto sito WKND](https://github.com/adobe/aem-guides-wknd) e includendo l’URL dell’immagine per i riferimenti della risorsa immagine nelle relative `primaryImage` , può essere eseguito con una nuova query persistente `wknd-shared/adventure-image-by-path` definito come:

```graphql {highlight="11"}
query($path: String!, $imageFormat: AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int, $imageQuality: Int) {
  adventureByPath(
    _path: $path
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
    }
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### Variabili di query

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "imageFormat": "JPG",
  "imageWidth": 1000,
}
```

Il `$path` variabile utilizzata nel `_path` il filtro richiede il percorso completo del frammento di contenuto (ad esempio `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

Il `_assetTransform` definisce il modo in cui `_dynamicUrl` è stato creato per ottimizzare il rendering dell’immagine trasmessa. Gli URL delle immagini ottimizzate per il web possono essere regolati anche sul client modificando i parametri di query dell’URL.

| Parametro GraphQL | Parametro URL | Descrizione | Obbligatorio | Valori delle variabili GraphQL | Valori dei parametri URL | Esempio di parametro URL |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | N/D | Formato della risorsa immagine. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | N/D | N/D |
| `seoName` | N/D | Nome del segmento di file nell’URL. Se non specificato, viene utilizzato il nome della risorsa immagine. | ✘ | Alfanumerico, `-`, o `_` | N/D | N/D |
| `crop` | `crop` | Il fotogramma ritagliato estratto dall&#39;immagine deve rientrare nelle dimensioni dell&#39;immagine | ✘ | Interi positivi che definiscono un’area di ritaglio entro i limiti delle dimensioni dell’immagine originale | Stringa di coordinate numeriche delimitata da virgole `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | Dimensione dell&#39;immagine di output (sia altezza che larghezza) in pixel. | ✘ | Interi positivi | Interi positivi delimitati da virgole nell’ordine `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | Rotazione dell&#39;immagine in gradi. | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | Capovolgere l&#39;immagine. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | Qualità immagine in percentuale rispetto alla qualità originale. | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | Larghezza dell&#39;immagine di output in pixel. Quando `size` è fornito `width` viene ignorato. | ✘ | Numero intero positivo | Numero intero positivo | `?width=1600` |
| `preferWebP` | `preferwebp` | Se `true` e AEM serve un WebP se il browser lo supporta, indipendentemente dal `format`. | ✘ | `true`, `false` | `true`, `false` | `?preferwebp=true` |

## Risposta GraphQL

La risposta JSON risultante contiene i campi richiesti contenenti l’URL ottimizzato per il web per le risorse dell’immagine.

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&width=1000&quality=80"
        }
      }
    }
  }
}
```

Per caricare nell’applicazione l’immagine ottimizzata per il web dell’immagine di riferimento, utilizzava `_dynamicUrl` del `primaryImage` come URL di origine dell’immagine.

In React, la visualizzazione di un’immagine ottimizzata per il web da AEM Publish ha l’aspetto di:

```jsx
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Ricorda: `_dynamicUrl` non include il dominio AEM, pertanto devi fornire l’origine desiderata per la risoluzione dell’URL dell’immagine.

## URL reattivi

L’esempio precedente mostra l’utilizzo di un’immagine in un’unica dimensione. Tuttavia, nelle esperienze web, spesso sono necessari set di immagini reattive. Le immagini reattive possono essere implementate utilizzando [srcset img](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) o [elementi immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). Il seguente frammento di codice mostra come utilizzare `_dynamicUrl` come basato su e aggiungi diversi parametri di larghezza per abilitare diverse viste reattive. Non solo il `width` è possibile utilizzare il parametro di query, ma il client può aggiungere altri parametri di query per ottimizzare ulteriormente la risorsa immagine in base alle sue esigenze.

```javascript
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

## Esempio di React

Creiamo una semplice applicazione React che visualizza le immagini ottimizzate per il web seguendo [modelli di immagine reattiva](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Esistono due modelli principali per le immagini reattive:

+ [Elemento Img con set di immagini](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) per prestazioni migliori
+ [Elemento immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) per il controllo della progettazione

### Elemento Img con set di immagini

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Elementi immagine con set di immagini](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) vengono utilizzati con `sizes` per fornire diverse risorse di immagini per diverse dimensioni di schermo. Le srcset di immagini sono utili quando si forniscono risorse di immagine diverse per schermi di dimensioni diverse.

### Elemento immagine

[Elementi dell&#39;immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) sono utilizzati con più `source` per fornire risorse di immagini diverse per schermi di dimensioni diverse. Gli elementi immagine sono utili quando si forniscono rappresentazioni di immagini diverse per schermi di dimensioni diverse.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### Esempio di codice

Questa semplice app React utilizza [SDK headless per AEM](./aem-headless-sdk.md) per eseguire query sulle API headless dell’AEM per contenuti Adventure e visualizza l’immagine ottimizzata per il web utilizzando [elemento img con srcset](#img-element-with-srcset) e [elemento immagine](#picture-element). Il `srcset` e `sources` utilizzare un `setParams` funzione per aggiungere il parametro di query di consegna ottimizzata per il web alla `_dynamicUrl` dell’immagine, in modo da modificare la rappresentazione dell’immagine consegnata in base alle esigenze del client web.

La query contro l’AEM viene eseguita con l’hook React personalizzato [useAdventureByPath che utilizza l’SDK headless dell’AEM](./aem-headless-sdk.md#graphql-persisted-queries).

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} imageUrl the image URL
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setOptimizedImageUrlParams(imageUrl, params) {
    let url = new URL(imageUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { imageFormat: "JPG" }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  const alt = data.adventureByPath.item.title;
  const imageUrl =  AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setOptimizedImageUrlParams(imageUrl, { width: 1000 })}
        srcSet={
            `${setOptimizedImageUrlParams(imageUrl, { width: 1000 })} 1000w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 1600 })} 1600w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setOptimizedImageUrlParams(imageUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
