---
title: Utilizzo del testo RTF con AEM headless
description: Scopri come creare e incorporare contenuti a cui si fa riferimento utilizzando un editor Rich Text su più righe con Frammenti di contenuto di Adobe Experience Manager e come il testo RTF viene distribuito dalle API GraphQL dell’AEM come JSON per essere utilizzato dalle applicazioni headless.
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '1465'
ht-degree: 0%

---

# Testo avanzato con AEM headless

Il campo di testo su più righe è un tipo di dati Frammenti di contenuto che consente agli autori di creare contenuto in formato Rich Text. I riferimenti ad altri contenuti, come immagini o altri frammenti di contenuto, possono essere inseriti dinamicamente in linea all’interno del flusso del testo. Il campo di testo a riga singola è un altro tipo di dati Frammenti di contenuto che deve essere utilizzato per elementi di testo semplici.

L’API GraphQL dell’AEM offre una solida funzionalità per restituire il testo RTF come HTML, testo normale o come JSON puro. La rappresentazione JSON è potente in quanto offre all’applicazione client il controllo completo su come eseguire il rendering del contenuto.

## Editor multiriga

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

Nell’Editor frammento di contenuto, la barra dei menu del campo di testo su più righe fornisce agli autori funzionalità di formattazione RTF standard, ad esempio **grassetto**, *corsivo* e sottolineano. L’apertura del campo su più righe in modalità a schermo intero consente di [strumenti di formattazione aggiuntivi come Tipo di paragrafo, Trova e sostituisci, Controllo ortografico e altro ancora](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=it).

>[!NOTE]
>
> I plug-in Rich Text nell’editor multiriga non possono essere personalizzati.

## Tipo di dati testo su più righe {#multi-line-data-type}

Utilizza il **Testo su più righe** tipo di dati durante la definizione del modello per frammenti di contenuto per abilitare l’authoring di testo RTF.

![Tipo di dati Rich Text su più righe](assets/rich-text/multi-line-rich-text.png)

È possibile configurare diverse proprietà del campo su più righe.

Il **Rendering come** può essere impostata su:

* Area di testo: esegue il rendering di un singolo campo su più righe
* Campo multiplo: esegue il rendering di più campi riga


Il **Tipo predefinito** può essere impostato su:

* Testo formattato
* Markdown
* Testo normale

Il **Tipo predefinito** influisce direttamente sull’esperienza di modifica e determina se sono presenti gli strumenti rich text.

È inoltre possibile [abilita riferimenti in linea](#insert-fragment-references) ad altri frammenti di contenuto selezionando la **Consenti riferimento frammento** e la configurazione **Modelli per frammenti di contenuto consentiti**.

Controlla la **Traducibile** , se il contenuto deve essere localizzato. È possibile localizzare solo testo formattato e testo normale. Consulta [utilizzo dei contenuti localizzati per ulteriori dettagli](./localized-content.md).

## Risposta in formato Rich Text con API GraphQL

Durante la creazione di una query GraphQL, gli sviluppatori possono scegliere tipi di risposta diversi da `html`, `plaintext`, `markdown`, e `json` da un campo con più righe.

Gli sviluppatori possono utilizzare [Anteprima JSON](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) nell’editor dei frammenti di contenuto per mostrare tutti i valori del frammento di contenuto corrente che possono essere restituiti utilizzando l’API GraphQL.

## Query persistente GraphQL

Selezione del `json` il formato di risposta per il campo su più righe offre la massima flessibilità quando si lavora con contenuti rich text. Il contenuto Rich Text viene distribuito come array di tipi di nodo JSON che possono essere elaborati in modo univoco in base alla piattaforma client.

Di seguito è riportato un tipo di risposta JSON di un campo con più righe denominato `main` che contiene un paragrafo: &quot;*Questo paragrafo include **importante**contenuto.*&quot; dove &quot;importante&quot; è contrassegnato come **grassetto**.

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

Il `$path` variabile utilizzata nel `_path` il filtro richiede il percorso completo del frammento di contenuto (ad esempio `/content/dam/wknd/en/magazine/sample-article`).

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

Di seguito sono riportati diversi esempi di tipi di risposta di un campo con più righe denominato `main` che contiene un paragrafo: &quot;Questo è un paragrafo che include **importante** contenuti.&quot; dove &quot;importante&quot; è contrassegnato come **grassetto**.

+++HTML esempio

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

Il `plaintext` l’opzione di rendering elimina la formattazione.

+++


## Rendering di una risposta JSON in formato Rich Text {#render-multiline-json-richtext}

La risposta JSON Rich Text del campo su più righe è strutturata come una struttura gerarchica. Ogni oggetto o nodo rappresenta un diverso blocco HTML del testo RTF.

Di seguito è riportato un esempio di risposta JSON di un campo di testo su più righe. Ogni oggetto o nodo include un `nodeType` che rappresenta il blocco HTML dal testo RTF come `paragraph`, `link`, e `text`. Ogni nodo contiene facoltativamente `content` che è un sottoarray contenente qualsiasi elemento secondario del nodo corrente.

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

Il modo più semplice per eseguire il rendering di più righe `json` la risposta consiste nell&#39;elaborare ogni oggetto, o nodo, nella risposta e quindi gli eventuali elementi secondari del nodo corrente. È possibile utilizzare una funzione ricorsiva per scorrere la struttura JSON.

Di seguito è riportato un codice di esempio che illustra un approccio trasversale ricorsivo. Gli esempi sono basati su JavaScript e utilizzano React [JSX](https://reactjs.org/docs/introducing-jsx.html)Tuttavia, i concetti di programmazione possono essere applicati a qualsiasi linguaggio.

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

`renderNodeList` è una funzione ricorsiva che accetta un array di `childNodes`. Ogni nodo nell’array viene quindi passato a una funzione `renderNode`, che a sua volta chiama `renderNodeList` se il nodo ha elementi secondari.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

Il `renderNode` La funzione prevede un singolo oggetto denominato `node`. Un nodo può avere elementi secondari elaborati in modo ricorsivo utilizzando `renderNodeList` funzione descritta in precedenza. Infine, un’ `nodeMap` viene utilizzato per eseguire il rendering del contenuto del nodo in base ai `nodeType`.

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

Il `nodeMap` è un valore letterale di oggetto JavaScript utilizzato come mappa. Ognuna delle &quot;chiavi&quot; rappresenta una `nodeType`. Parametri di `node` e `children` può essere trasmesso alle funzioni risultanti che eseguono il rendering del nodo. Il tipo restituito utilizzato in questo esempio è JSX, tuttavia l’approccio potrebbe essere adattato per creare una stringa letterale che rappresenta il contenuto HTML.

### Esempio di codice completo

Un&#39;utilità di rendering Rich Text riutilizzabile è disponibile nel [Esempio di reazione WKND GraphQL](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) - utilità riutilizzabile che espone una funzione `mapJsonRichText`. Questa utility può essere utilizzata dai componenti che desiderano eseguire il rendering di una risposta JSON Rich Text come React JSX.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - Componente di esempio che effettua una richiesta GraphQL che include testo RTF. Il componente utilizza `mapJsonRichText` per il rendering del testo RTF e di tutti i riferimenti.


## Aggiungere riferimenti in linea al testo RTF {#insert-fragment-references}

Il campo Multiriga consente agli autori di inserire immagini o altre risorse digitali da AEM Assets nel flusso del testo RTF.

![inserisci immagine](assets/rich-text/insert-image.png)

La schermata precedente rappresenta un’immagine inserita nel campo su più righe utilizzando **Inserisci risorsa** pulsante.

È inoltre possibile collegare o inserire nel campo su più righe riferimenti ad altri frammenti di contenuto utilizzando **Inserisci frammento di contenuto** pulsante.

![Inserisci riferimento frammento di contenuto](assets/rich-text/insert-contentfragment.png)

La schermata precedente mostra un altro frammento di contenuto, Ultimate Guide to LA Skate Parks, inserito nel campo su più righe. I tipi di frammenti di contenuto che possono essere inseriti nel campo sono controllati da **Modelli per frammenti di contenuto consentiti** configurazione in [tipo di dati su più righe](#multi-line-data-type) nel modello per frammenti di contenuto.

## Eseguire query sui riferimenti in linea con GraphQL

L’API di GraphQL consente agli sviluppatori di creare una query che include proprietà aggiuntive su eventuali riferimenti inseriti in un campo su più righe. La risposta JSON include un `_references` oggetto che elenca queste proprietà aggiuntive. La risposta JSON offre agli sviluppatori il controllo completo su come eseguire il rendering dei riferimenti o dei collegamenti invece di dover trattare con HTML categorici.

Ad esempio, potrebbe essere utile:

* Includere una logica di indirizzamento personalizzata per la gestione dei collegamenti ad altri frammenti di contenuto durante l’implementazione di un’applicazione a pagina singola, ad esempio utilizzando React Router o Next.js
* Eseguire il rendering di un’immagine in linea utilizzando il percorso assoluto di un ambiente di pubblicazione AEM come `src` valore.
* Determina come eseguire il rendering di un riferimento incorporato in un altro frammento di contenuto con proprietà personalizzate aggiuntive.

Utilizza il `json` tipo restituito e include `_references` durante la costruzione di una query GraphQL:

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

Nella query precedente, il `main` viene restituito come JSON. Il `_references` L&#39;oggetto include frammenti per la gestione di riferimenti di tipo `ImageRef` o di tipo `ArticleModel`.

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

La risposta JSON include dove è stato inserito il riferimento nel testo RTF con `"nodeType": "reference"`. Il `_references` L&#39;oggetto include quindi ogni riferimento.

## Rendering dei riferimenti in linea nel testo RTF

Per eseguire il rendering dei riferimenti in linea, l&#39;approccio ricorsivo illustrato nella [Rendering di una risposta JSON su più righe](#render-multiline-json-richtext) può essere espanso.

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

L&#39;approccio di alto livello consiste nell&#39;ispezionare ogni `nodeType` è uguale a `reference` nella risposta JSON a più righe. È quindi possibile chiamare una funzione di rendering personalizzata che include `_references` oggetto restituito nella risposta di GraphQL.

Il percorso di riferimento in linea può quindi essere confrontato con la voce corrispondente nella `_references` oggetto e un&#39;altra mappa personalizzata `renderReference` può essere chiamato.

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

Il `__typename` del `_references` L’oggetto può essere utilizzato per mappare diversi tipi di riferimento a diverse funzioni di rendering.

### Esempio di codice completo

Un esempio completo di scrittura di un renderer di riferimenti personalizzato è disponibile in [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) nell&#39;ambito del [Esempio di reazione WKND GraphQL](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## Esempio completo

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> Il video precedente utilizza `_publishUrl` per eseguire il rendering del riferimento immagine. Preferisci invece `_dynamicUrl` come spiegato nella [procedure per le immagini ottimizzate per il web](./images.md);


Il video precedente mostra un esempio end-to-end:

1. Aggiornamento del campo di testo su più righe di un modello per frammenti di contenuto per consentire i riferimenti ai frammenti
2. Utilizzo dell’Editor frammento di contenuto per includere un’immagine e un riferimento a un altro frammento in un campo di testo su più righe.
3. Creazione di una query GraphQL che include la risposta di testo su più righe come JSON e qualsiasi `_references` utilizzato.
4. Scrittura di un SPA React che esegue il rendering dei riferimenti in linea della risposta Rich Text.
