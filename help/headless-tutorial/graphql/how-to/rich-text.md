---
title: Utilizzo del testo RTF con AEM Headless
description: Scopri come creare e incorporare contenuti a cui si fa riferimento utilizzando un editor Rich Text su più righe con Frammenti di contenuto di Adobe Experience Manager e come il testo Rich Text viene distribuito dalle API GraphQL di AEM come JSON per essere utilizzato dalle applicazioni headless.
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
duration: 785
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1409'
ht-degree: 0%

---

# Testo avanzato con AEM headless

Il campo di testo su più righe è un tipo di dati Frammenti di contenuto che consente agli autori di creare contenuto in formato Rich Text. I riferimenti ad altri contenuti, come immagini o altri frammenti di contenuto, possono essere inseriti dinamicamente in linea all’interno del flusso del testo. Il campo di testo a riga singola è un altro tipo di dati Frammenti di contenuto che deve essere utilizzato per elementi di testo semplici.

L’API GraphQL di AEM offre una solida funzionalità per restituire testo RTF come HTML, testo normale o come JSON puro. La rappresentazione JSON è potente in quanto offre all’applicazione client il controllo completo su come eseguire il rendering del contenuto.

## Editor multiriga

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

Nell&#39;Editor frammento di contenuto, la barra dei menu del campo di testo su più righe fornisce agli autori funzionalità di formattazione RTF standard, ad esempio **grassetto**, *corsivo* e sottolineatura. L&#39;apertura del campo su più righe in modalità a schermo intero abilita [strumenti di formattazione aggiuntivi come Tipo di paragrafo, Trova e sostituisci, Controllo ortografico e altro ancora](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=it).

>[!NOTE]
>
> I plug-in Rich Text nell’editor multiriga non possono essere personalizzati.

## Tipo di dati testo su più righe {#multi-line-data-type}

Utilizza il tipo di dati **Testo su più righe** quando definisci il modello per frammenti di contenuto per abilitare l&#39;authoring di testo RTF.

![Tipo di dati Rich Text su più righe](assets/rich-text/multi-line-rich-text.png)

È possibile configurare diverse proprietà del campo su più righe.

La proprietà **Rendering come** può essere impostata su:

* Area di testo: esegue il rendering di un singolo campo su più righe
* Campo multiplo: esegue il rendering di più campi riga


**Tipo predefinito** può essere impostato su:

* Testo formattato
* Markdown
* Testo normale

L&#39;opzione **Tipo predefinito** influenza direttamente l&#39;esperienza di modifica e determina se sono presenti gli strumenti Rich Text.

È inoltre possibile [abilitare i riferimenti in linea](#insert-fragment-references) ad altri frammenti di contenuto controllando **Consenti riferimento frammento** e configurando **Modelli per frammenti di contenuto consentiti**.

Selezionare la casella **Traducibile** se il contenuto deve essere localizzato. È possibile localizzare solo testo formattato e testo normale. Per ulteriori dettagli, consulta [utilizzo dei contenuti localizzati](./localized-content.md).

## Risposta in formato Rich Text con API GraphQL

Durante la creazione di una query GraphQL, gli sviluppatori possono scegliere tipi di risposta diversi da `html`, `plaintext`, `markdown` e `json` da un campo su più righe.

Gli sviluppatori possono utilizzare [Anteprima JSON](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html?lang=it) nell&#39;editor frammenti di contenuto per visualizzare tutti i valori del frammento di contenuto corrente che possono essere restituiti utilizzando l&#39;API GraphQL.

## Query persistente GraphQL

La selezione del formato di risposta `json` per il campo su più righe offre la massima flessibilità quando si lavora con contenuto in formato Rich Text. Il contenuto Rich Text viene distribuito come array di tipi di nodo JSON che possono essere elaborati in modo univoco in base alla piattaforma client.

Di seguito è riportato un tipo di risposta JSON di un campo con più righe denominato `main` che contiene un paragrafo: &quot;*Questo è un paragrafo che include il contenuto **importante**.*&quot; dove &quot;importante&quot; è contrassegnato come **bold**.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

La variabile `$path` utilizzata nel filtro `_path` richiede il percorso completo del frammento di contenuto (ad esempio `/content/dam/wknd/en/magazine/sample-article`).

**Risposta GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### Altri esempi

Di seguito sono riportati diversi esempi di tipi di risposta di un campo con più righe denominato `main` che contiene un paragrafo: &quot;Questo è un paragrafo che include il contenuto **importante**&quot;. dove &quot;importante&quot; è contrassegnato come **bold**.

+++Esempio HTML

**Query persistente GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**Risposta GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

+++Esempio di Markdown

**Query persistente GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**Risposta GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++Esempio di testo normale

**Query persistente GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**Risposta GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

L&#39;opzione di rendering `plaintext` elimina la formattazione.

+++


## Rendering di una risposta JSON in formato Rich Text {#render-multiline-json-richtext}

La risposta JSON Rich Text del campo su più righe è strutturata come una struttura gerarchica. Ogni oggetto o nodo rappresenta un diverso blocco HTML del testo RTF.

Di seguito è riportato un esempio di risposta JSON di un campo di testo su più righe. Ogni oggetto o nodo include un `nodeType` che rappresenta il blocco HTML dal testo RTF come `paragraph`, `link` e `text`. Ogni nodo contiene facoltativamente `content`, che è una sottomatrice contenente qualsiasi elemento secondario del nodo corrente.

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

Il modo più semplice per eseguire il rendering della risposta `json` su più righe consiste nell&#39;elaborare ogni oggetto, o nodo, nella risposta e quindi gli eventuali elementi secondari del nodo corrente. È possibile utilizzare una funzione ricorsiva per scorrere la struttura JSON.

Di seguito è riportato un codice di esempio che illustra un approccio trasversale ricorsivo. Gli esempi sono basati su JavaScript e utilizzano il [JSX](https://reactjs.org/docs/introducing-jsx.html) di React, tuttavia i concetti di programmazione possono essere applicati a qualsiasi linguaggio.

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

`renderNodeList` è una funzione ricorsiva che accetta una matrice di `childNodes`. Ogni nodo nell&#39;array viene quindi passato a una funzione `renderNode`, che a sua volta chiama `renderNodeList` se il nodo ha elementi secondari.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

La funzione `renderNode` prevede un singolo oggetto denominato `node`. Un nodo può avere elementi figlio elaborati in modo ricorsivo utilizzando la funzione `renderNodeList` descritta sopra. Infine, viene utilizzato un `nodeMap` per eseguire il rendering del contenuto del nodo in base al relativo `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

`nodeMap` è un valore letterale di oggetto JavaScript utilizzato come mappa. Ognuna delle &quot;chiavi&quot; rappresenta un `nodeType` diverso. I parametri di `node` e `children` possono essere trasmessi alle funzioni risultanti che eseguono il rendering del nodo. Il tipo restituito utilizzato in questo esempio è JSX, tuttavia l’approccio potrebbe essere adattato per creare una stringa letterale che rappresenta il contenuto di HTML.

### Esempio di codice completo

Un&#39;utilità di rendering Rich Text riutilizzabile è disponibile nell&#39;esempio di GraphQL React [WKND](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) - utilità riutilizzabile che espone una funzione `mapJsonRichText`. Questa utility può essere utilizzata dai componenti che desiderano eseguire il rendering di una risposta JSON Rich Text come React JSX.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - Componente di esempio che effettua una richiesta GraphQL che include testo RTF. Il componente utilizza l&#39;utility `mapJsonRichText` per eseguire il rendering del testo RTF e di tutti i riferimenti.


## Aggiungere riferimenti in linea al testo RTF {#insert-fragment-references}

Il campo Multiriga consente agli autori di inserire immagini o altre risorse digitali da AEM Assets nel flusso del testo RTF.

![inserisci immagine](assets/rich-text/insert-image.png)

La schermata precedente mostra un&#39;immagine inserita nel campo su più righe utilizzando il pulsante **Inserisci risorsa**.

È inoltre possibile collegare o inserire riferimenti ad altri frammenti di contenuto nel campo su più righe utilizzando il pulsante **Inserisci frammento di contenuto**.

![Inserisci riferimento frammento di contenuto](assets/rich-text/insert-contentfragment.png)

La schermata precedente mostra un altro frammento di contenuto, Ultimate Guide to LA Skate Parks, inserito nel campo su più righe. I tipi di frammenti di contenuto che è possibile inserire nel campo sono controllati dalla configurazione **Modelli per frammenti di contenuto consentiti** nel tipo di dati [su più righe](#multi-line-data-type) nel modello per frammenti di contenuto.

## Eseguire query sui riferimenti in linea con GraphQL

L’API di GraphQL consente agli sviluppatori di creare una query che include proprietà aggiuntive su eventuali riferimenti inseriti in un campo su più righe. La risposta JSON include un oggetto `_references` separato in cui sono elencate queste proprietà aggiuntive. La risposta JSON offre agli sviluppatori il controllo completo su come eseguire il rendering dei riferimenti o dei collegamenti invece di dover gestire HTML con opinioni.

Ad esempio, potrebbe essere utile:

* Includere una logica di indirizzamento personalizzata per la gestione dei collegamenti ad altri frammenti di contenuto durante l’implementazione di un’applicazione a pagina singola, ad esempio utilizzando React Router o Next.js
* Eseguire il rendering di un&#39;immagine in linea utilizzando il percorso assoluto di un ambiente di pubblicazione AEM come valore `src`.
* Determina come eseguire il rendering di un riferimento incorporato in un altro frammento di contenuto con proprietà personalizzate aggiuntive.

Utilizzare il tipo restituito `json` e includere l&#39;oggetto `_references` durante la costruzione di una query GraphQL:

**Query persistente GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }  
    }
  }
}
```

Nella query precedente, il campo `main` viene restituito come JSON. L&#39;oggetto `_references` include frammenti per la gestione di riferimenti di tipo `ImageRef` o `ArticleModel`.

**Risposta JSON:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

La risposta JSON include il punto in cui il riferimento è stato inserito nel testo RTF con `"nodeType": "reference"`. L&#39;oggetto `_references` include quindi ogni riferimento.

## Rendering dei riferimenti in linea nel testo RTF

Per eseguire il rendering dei riferimenti in linea, è possibile espandere l&#39;approccio ricorsivo illustrato in [Rendering di una risposta JSON su più righe](#render-multiline-json-richtext).

Dove `nodeMap` è la mappa che esegue il rendering dei nodi JSON.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

L&#39;approccio di alto livello consiste nel controllare ogni volta che un `nodeType` è uguale a `reference` nella risposta JSON a più righe. È quindi possibile chiamare una funzione di rendering personalizzata che includa l&#39;oggetto `_references` restituito nella risposta di GraphQL.

Il percorso di riferimento in linea può quindi essere confrontato con la voce corrispondente nell&#39;oggetto `_references` ed è possibile chiamare un&#39;altra mappa personalizzata `renderReference`.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

L&#39;oggetto `__typename` dell&#39;oggetto `_references` può essere utilizzato per mappare diversi tipi di riferimento a diverse funzioni di rendering.

### Esempio di codice completo

Un esempio completo di scrittura di un renderer di riferimenti personalizzato è disponibile in [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) come parte del [esempio WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## Esempio completo

>[!VIDEO](https://video.tv.adobe.com/v/3449708?quality=12&learn=on&captions=ita)

>[!NOTE]
>
> Nel video precedente viene utilizzato `_publishUrl` per il rendering del riferimento all&#39;immagine. Preferisci invece `_dynamicUrl` come descritto in [procedure per le immagini ottimizzate per il web](./images.md);


Il video precedente mostra un esempio end-to-end:

1. Aggiornamento del campo di testo su più righe di un modello per frammenti di contenuto per consentire i riferimenti ai frammenti
2. Utilizzo dell’Editor frammento di contenuto per includere un’immagine e un riferimento a un altro frammento in un campo di testo su più righe.
3. Creazione di una query GraphQL che include la risposta di testo su più righe come JSON e qualsiasi `_references` utilizzato.
4. Scrittura di un’applicazione a pagina singola React che esegue il rendering dei riferimenti in linea della risposta in formato Rich Text.
