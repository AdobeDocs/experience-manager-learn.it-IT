---
title: Utilizzo di immagini ottimizzate con AEM Headless
description: Scopri come richiedere URL immagine ottimizzati con AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: 71b2dc0e8ebec1157694ae55118f2426558566e3
workflow-type: tm+mt
source-wordcount: '935'
ht-degree: 6%

---

# Immagini ottimizzate con AEM Headless {#images-with-aem-headless}

Le immagini sono un aspetto fondamentale [sviluppo di esperienze ricche e coinvolgenti AEM senza testa](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=it). AEM Headless supporta la gestione delle risorse di immagini e la loro distribuzione ottimizzata.

I frammenti di contenuto utilizzati nella modellazione AEM contenuto headless fanno spesso riferimento a risorse di immagini destinate alla visualizzazione nell’esperienza headless. AEM le query GraphQL possono essere scritte per fornire URL alle immagini in base a dove si fa riferimento all&#39;immagine.

La `ImageRef` Il tipo dispone di quattro opzioni URL per i riferimenti di contenuto:

+ `_path` è il percorso a cui si fa riferimento in AEM e non include un&#39;origine AEM (nome host)
+ `_dynamicUrl` è l’URL completo della risorsa immagine preferita ottimizzata per il web.
   + La `_dynamicUrl` non include un&#39;origine AEM, pertanto il dominio (servizio AEM Author o AEM Publish) deve essere fornito dall&#39;applicazione client.
+ `_authorUrl` è l’URL completo della risorsa immagine su AEM Author
   + [AEM Author](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) può essere utilizzato per fornire un&#39;esperienza di anteprima dell&#39;applicazione headless.
+ `_publishUrl` è l’URL completo della risorsa immagine su AEM Publish
   + [Pubblicazione AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) è in genere il luogo in cui l&#39;implementazione di produzione dell&#39;applicazione headless visualizza le immagini da.

La `_dynamicUrl` è l’URL preferito da utilizzare per le risorse di immagini e dovrebbe sostituire l’utilizzo di `_path`, `_authorUrl`e `_publishUrl` quando possibile.

|  | AEM as a Cloud Service | AEM RDE as a Cloud Service | SDK AEM | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| Supporta immagini ottimizzate per il web? | ↓ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Immagini con AEM headless"
>abstract="Scopri in che modo AEM Headless supporta la gestione delle risorse di immagini e la loro consegna ottimizzata."

## Modello per frammenti di contenuto

Assicurati che il campo Frammento di contenuto contenente il riferimento all’immagine sia del __riferimento contenuto__ tipo di dati.

I tipi di campo vengono esaminati nella sezione [Modello per frammento di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), selezionando il campo e controllando il __Proprietà__ a destra.

![Modello per frammento di contenuto con riferimento a contenuto per un’immagine](./assets/images/content-fragment-model.jpeg)

## Query persistente GraphQL

Nella query GraphQL, restituisce il campo come `ImageRef` e richiede il `_dynamicUrl` campo . Ad esempio, la query di un&#39;avventura nel [Progetto sito WKND](https://github.com/adobe/aem-guides-wknd) e l’URL dell’immagine per i riferimenti alla risorsa immagine nel relativo `primaryImage` può essere eseguito con una nuova query persistente `wknd-shared/adventure-image-by-path` definito come:

```graphql {highlight="11"}
query($path: String!, $assetTransform: AssetTransform!) {
  adventureByPath(
    _path: $path
    _assetTransform: $assetTransform
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
  "assetTransform": { "format": "JPG", "quality": 80, "preferWebp": true}
}
```

La `$path` nella variabile `_path` Il filtro richiede il percorso completo del frammento di contenuto (ad esempio `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

La `_assetTransform` definisce come `_dynamicUrl` è progettato per ottimizzare il rendering delle immagini servite. Gli URL delle immagini ottimizzate per il web possono essere regolati anche sul client modificando i parametri di query dell&#39;URL.

| Parametro GraphQL | Parametro URL | Descrizione | Obbligatorio | Valori variabili GraphQL | Valori dei parametri URL | Esempio di variabile GraphQL | Esempio di parametro URL |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:---|:--|
| `format` | `format` | Il formato della risorsa immagine. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | N/D | `{ format: JPG }` | N/D |
| `seoName` | N/D | Nome del segmento di file nell’URL. Se non viene fornito, viene utilizzato il nome della risorsa immagine. | ✘ | Alfanumerico, `-`oppure `_` | N/D | `{ seoName: "bali-surf-camp" }` | N/D |
| `crop` | `crop` | Il fotogramma di ritaglio estratto dall&#39;immagine deve essere delle dimensioni dell&#39;immagine | ✘ | Numeri interi positivi che definiscono un’area di ritaglio entro i limiti delle dimensioni dell’immagine originale | Stringa delimitata da virgole di coordinate numeriche `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `{ crop: { xOrigin: 10, yOrigin: 20, width: 300, height: 400} }` | `?crop=10,20,300,400` |
| `size` | `size` | Dimensioni dell&#39;immagine di output (sia in altezza che in larghezza) in pixel. | ✘ | Numeri interi positivi | Numeri interi positivi delimitati da virgole nell’ordine `<WIDTH>,<HEIGHT>` | `{ size: { width: 1200, height: 800 } }` | `?size=1200,800` |
| `rotation` | `rotate` | Rotazione dell&#39;immagine in gradi. | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `{ rotation: R90 }` | `?rotate=90` |
| `flip` | `flip` | Capovolgi l&#39;immagine. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `{ flip: horizontal }` | `?flip=h` |
| `quality` | `quality` | Qualità dell&#39;immagine in percentuale della qualità originale. | ✘ | 1-100 | 1-100 | `{ quality: 80 }` | `?quality=80` |
| `width` | `width` | Larghezza dell&#39;immagine di output in pixel. Quando `size` è fornito `width` viene ignorato. | ✘ | Numero intero positivo | Numero intero positivo | `{ width: 1600 }` | `?width=1600` |
| `preferWebP` | `preferwebp` | Se `true` e AEM un WebP se il browser lo supporta, indipendentemente dal `format`. | ✘ | `true`, `false` | `true`, `false` | `{ preferWebp: true }` | `?preferwebp=true` |

## Risposta GraphQL

La risposta JSON risultante contiene i campi richiesti contenenti l’URL ottimizzato per il Web per le risorse immagine.

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&quality=80"
        }
      }
    }
  }
}
```

Per caricare nell&#39;applicazione l&#39;immagine ottimizzata per il web dell&#39;immagine a cui si fa riferimento, è stato utilizzato il `_dynamicUrl` del `primaryImage` come URL sorgente dell’immagine.

In React, la visualizzazione di un’immagine ottimizzata per il web da AEM Publish ha l’aspetto seguente:

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Ricorda, `_dynamicUrl` non include il dominio AEM, pertanto devi fornire l’origine desiderata per la risoluzione dell’URL immagine.

### URL reattivi

L’esempio precedente mostra l’utilizzo di un’immagine a dimensione singola. Tuttavia, nelle esperienze web, i set di immagini reattivi sono spesso necessari. Le immagini reattive possono essere implementate utilizzando [srcset img](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) o [elementi dell&#39;immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). Il seguente frammento di codice mostra come utilizzare il `_dynamicUrl` in base a e aggiungi diversi parametri di larghezza per alimentare diverse viste reattive. Non solo è possibile `width` è possibile utilizzare il parametro query, ma altri parametri di query possono essere aggiunti dal client per ottimizzare ulteriormente la risorsa immagine in base alle sue esigenze.

```javascript
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${${dynamicUrl}&width=1000}"
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

### React example

Creiamo una semplice applicazione React che visualizza le immagini ottimizzate per il Web [pattern di immagine reattivi](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Esistono due pattern principali per le immagini reattive:

+ [Elemento Img con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) per migliorare le prestazioni
+ [Elemento dell&#39;immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) per il controllo della progettazione

#### Elemento Img con srcset

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Elementi Img con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) vengono utilizzati con `sizes` per fornire risorse di immagine diverse per schermi di diverse dimensioni. Le sequenze di immagini sono utili quando si forniscono risorse di immagini diverse per schermi di diverse dimensioni.

#### Elemento dell&#39;immagine

[Elementi dell&#39;immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) sono utilizzati con più `source` per fornire risorse di immagine diverse per schermi di diverse dimensioni. Gli elementi dell&#39;immagine sono utili quando si forniscono rappresentazioni di immagini diverse per schermi di diverse dimensioni.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

#### Esempio di codice

Questa semplice app React utilizza [AEM Headless SDK](./aem-headless-sdk.md) per eseguire query AEM API headless per un contenuto Avventura e visualizza l&#39;immagine ottimizzata per il web utilizzando [elemento img con srcset](#img-element-with-srcset) e [elemento dell&#39;immagine](#picture-element). La `srcset` e `sources` utilizza un `setParams` per aggiungere il parametro di query di consegna ottimizzato per il web al `_dynamicUrl` dell’immagine, quindi modifica il rendering dell’immagine consegnato in base alle esigenze del client web.

L&#39;operazione di Query rispetto a AEM viene eseguita nel gancio React personalizzato [useAdventureByPath che utilizza l&#39;SDK AEM Headless](./aem-headless-sdk.md#graphql-persisted-queries).

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
        { assetTransform: { format: "JPG", preferWebp: true } }
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
