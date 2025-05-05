---
title: Utilizzo di immagini ottimizzate con AEM Headless
description: Scopri come richiedere URL immagine ottimizzati con AEM Headless.
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 300
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 4%

---

# Immagini ottimizzate con AEM headless {#images-with-aem-headless}

Le immagini sono un aspetto critico dello [sviluppo di esperienze AEM headless complesse e coinvolgenti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=it). AEM Headless supporta la gestione delle risorse di immagini e la loro distribuzione ottimizzata.

I frammenti di contenuto utilizzati nella modellazione di contenuti AEM Headless spesso fanno riferimento a risorse di immagini destinate a essere visualizzate nell’esperienza headless. È possibile scrivere le query GraphQL di AEM per fornire URL alle immagini in base alla provenienza dell’immagine.

Il tipo `ImageRef` dispone di quattro opzioni URL per i riferimenti al contenuto:

+ `_path` è il percorso di riferimento in AEM e non include un&#39;origine AEM (nome host)
+ `_dynamicUrl` è l&#39;URL di per la consegna ottimizzata per il web della risorsa immagine.
   + `_dynamicUrl` non include un&#39;origine AEM, pertanto il dominio (servizio AEM Author o AEM Publish) deve essere fornito dall&#39;applicazione client.
+ `_authorUrl` è l&#39;URL completo della risorsa immagine in AEM Author
   + [AEM Author](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=it) può essere utilizzato per fornire un&#39;anteprima dell&#39;applicazione headless.
+ `_publishUrl` è l&#39;URL completo della risorsa immagine in AEM Publish
   + [Pubblicazione AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=it) è in genere il luogo in cui la distribuzione di produzione dell&#39;applicazione headless visualizza le immagini da.

`_dynamicUrl` è l&#39;URL consigliato da utilizzare per la consegna delle risorse immagine e dovrebbe sostituire l&#39;utilizzo di `_path`, `_authorUrl` e `_publishUrl` quando possibile.

|                                | AEM as a Cloud Service | AEM AS A CLOUD SERVICE RDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| Supporta le immagini ottimizzate per il web? | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Immagini con AEM headless"
>abstract="Scopri in che modo AEM Headless supporta la gestione delle risorse di immagini e la loro consegna ottimizzata."

## Modello per frammenti di contenuto

Verifica che il campo Frammento di contenuto contenente il riferimento all&#39;immagine sia del tipo di dati __riferimento contenuto__.

I tipi di campo vengono esaminati nel [Modello per frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=it), selezionando il campo e controllando la scheda __Proprietà__ a destra.

![Modello per frammenti di contenuto con riferimento a un&#39;immagine](./assets/images/content-fragment-model.jpeg)

## Query persistente GraphQL

Nella query GraphQL, restituisce il campo come tipo `ImageRef` e richiede il campo `_dynamicUrl`. Ad esempio, è possibile eseguire una query su un&#39;avventura nel [progetto del sito WKND](https://github.com/adobe/aem-guides-wknd) e includere l&#39;URL dell&#39;immagine per i riferimenti alla risorsa immagine nel relativo campo `primaryImage` con una nuova query persistente `wknd-shared/adventure-image-by-path` definita come:

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

La variabile `$path` utilizzata nel filtro `_path` richiede il percorso completo del frammento di contenuto (ad esempio `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

`_assetTransform` definisce il modo in cui `_dynamicUrl` è costruito per ottimizzare il rendering dell&#39;immagine fornita. Gli URL delle immagini ottimizzate per il web possono essere regolati anche sul client modificando i parametri di query dell’URL.

| Parametro GraphQL | Descrizione | Obbligatorio | Valori delle variabili GraphQL |
|-------------------|------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------|
| `format` | Formato della risorsa immagine. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`, `WEBP`, `WEBPLL`, `WEBPLY` |
| `seoName` | Nome del segmento di file nell’URL. Se non specificato, viene utilizzato il nome della risorsa immagine. | ✘ | Alfanumerico, `-` o `_` |
| `crop` | Il fotogramma ritagliato estratto dall&#39;immagine deve rientrare nelle dimensioni dell&#39;immagine | ✘ | Interi positivi che definiscono un’area di ritaglio entro i limiti delle dimensioni dell’immagine originale |
| `size` | Dimensione dell&#39;immagine di output (sia altezza che larghezza) in pixel. | ✘ | Interi positivi |
| `rotation` | Rotazione dell&#39;immagine in gradi. | ✘ | `R90`, `R180`, `R270` |
| `flip` | Capovolgere l&#39;immagine. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` |
| `quality` | Qualità immagine in percentuale rispetto alla qualità originale. | ✘ | 1-100 |
| `width` | Larghezza dell&#39;immagine di output in pixel. Quando viene fornito `size`, `width` viene ignorato. | ✘ | Numero intero positivo |
| `preferWebP` | Se `true` e AEM servono un WebP se il browser lo supporta, indipendentemente da `format`. | ✘ | `true`, `false` |


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

Per caricare nell&#39;applicazione l&#39;immagine ottimizzata per il web dell&#39;immagine di riferimento, ha utilizzato `_dynamicUrl` di `primaryImage` come URL di origine dell&#39;immagine.

In React, la visualizzazione di un’immagine ottimizzata per il web da AEM Publish ha l’aspetto di:

```jsx
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Ricorda che `_dynamicUrl` non include il dominio AEM, pertanto devi fornire l&#39;origine desiderata per la risoluzione dell&#39;URL dell&#39;immagine.

## URL reattivi

L’esempio precedente mostra l’utilizzo di un’immagine in un’unica dimensione. Tuttavia, nelle esperienze web, spesso sono necessari set di immagini reattive. Le immagini reattive possono essere implementate utilizzando [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) o [elementi immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). Il seguente frammento di codice mostra come utilizzare `_dynamicUrl` come base. `width` è un parametro URL che puoi aggiungere a `_dynamicUrl` per attivare diverse visualizzazioni reattive.

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

Creiamo una semplice applicazione React che visualizza immagini ottimizzate per il web seguendo [modelli di immagine reattiva](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Esistono due modelli principali per le immagini reattive:

+ [Elemento Img con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) per prestazioni migliorate
+ [Elemento immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) per il controllo struttura

### Elemento Img con set di immagini

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Gli elementi immagine con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) vengono utilizzati con l&#39;attributo `sizes` per fornire risorse immagine diverse per schermi di dimensioni diverse. Le srcset di immagini sono utili quando si forniscono risorse di immagine diverse per schermi di dimensioni diverse.

### Elemento immagine

[Gli elementi immagine](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) vengono utilizzati con più elementi `source` per fornire risorse immagine diverse per schermi di dimensioni diverse. Gli elementi immagine sono utili quando si forniscono rappresentazioni di immagini diverse per schermi di dimensioni diverse.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### Esempio di codice

Questa app React semplice utilizza [AEM Headless SDK](./aem-headless-sdk.md) per eseguire query sulle API AEM Headless per contenuti Adventure e visualizza l&#39;immagine ottimizzata per il web utilizzando l&#39;elemento [img con srcset](#img-element-with-srcset) e l&#39;elemento [picture](#picture-element). `srcset` e `sources` utilizzano una funzione `setParams` personalizzata per aggiungere il parametro della query di consegna ottimizzata per il Web all&#39;elemento `_dynamicUrl` dell&#39;immagine, pertanto modificare il rendering dell&#39;immagine consegnata in base alle esigenze del client Web.

La query su AEM viene eseguita nell&#39;hook React personalizzato [useAdventureByPath che utilizza AEM Headless SDK](./aem-headless-sdk.md#graphql-persisted-queries).

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
