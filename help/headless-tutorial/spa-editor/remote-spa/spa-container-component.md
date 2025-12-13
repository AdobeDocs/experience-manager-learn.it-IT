---
title: Aggiungere componenti contenitore React modificabili a un’applicazione a pagina singola remota
description: Scopri come aggiungere componenti contenitore modificabili a un’applicazione a pagina singola remota che consentono agli autori di AEM di trascinarvi i componenti.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
duration: 306
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 1%

---

# Componenti contenitore modificabili

{{spa-editor-deprecation}}

[I componenti fissi](./spa-fixed-component.md) offrono una certa flessibilità per l&#39;authoring dei contenuti SPA, tuttavia questo approccio è rigido e richiede agli sviluppatori di definire la composizione esatta dei contenuti modificabili. Per supportare la creazione di esperienze eccezionali da parte degli autori, l’Editor SPA supporta l’utilizzo di componenti contenitore nell’SPA. I componenti contenitore consentono agli autori di trascinare e rilasciare i componenti consentiti nel contenitore e di crearli, come fanno nell’authoring AEM Sites tradizionale.

![Componenti contenitore modificabili](./assets/spa-container-component/intro.png)

In questo capitolo, viene aggiunto un contenitore modificabile alla visualizzazione Home che consente agli autori di comporre e creare il layout di esperienze con contenuti avanzati utilizzando i componenti React modificabili direttamente nell’applicazione a pagina singola.

## Aggiornare l’app WKND

Per aggiungere un componente contenitore alla vista Home:

* Importa il componente `ResponsiveGrid` del componente AEM React Editable
* Importare e registrare componenti React modificabili personalizzati (testo e immagine) da utilizzare nel componente ResponsiveGrid

### Utilizzare il componente ResponsiveGrid

Per aggiungere un&#39;area modificabile alla vista Home:

1. Apri e modifica `react-app/src/components/Home.js`
1. Importare il componente `ResponsiveGrid` da `@adobe/aem-react-editable-components` e aggiungerlo al componente `Home`.
1. Imposta i seguenti attributi sul componente `<ResponsiveGrid...>`
   1. `pagePath = '/content/wknd-app/us/en/home'`
   1. `itemPath = 'root/responsivegrid'`

   Questo indica al componente `ResponsiveGrid` di recuperare il contenuto dalla risorsa AEM:

   1. `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   `itemPath` è mappato al nodo `responsivegrid` definito nel modello AEM `Remote SPA Page` e viene creato automaticamente nelle nuove pagine AEM create dal modello AEM `Remote SPA Page`.

   Aggiornare `Home.js` per aggiungere il componente `<ResponsiveGrid...>`.

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

Il file `Home.js` deve essere simile al seguente:

![Home.js](./assets/spa-container-component/home-js.png)

## Creare componenti modificabili

Ottenere il pieno effetto dei contenitori di esperienza di authoring flessibili forniti nell’editor SPA. È già stato creato un componente Titolo modificabile, ma creiamo alcuni altri che consentono agli autori di utilizzare componenti Testo e Immagine modificabili nel componente ResponsiveGrid appena aggiunto.

I nuovi componenti Text e Image React modificabili vengono creati utilizzando il modello di definizione del componente modificabile esportato in [componenti fissi modificabili](./spa-fixed-component.md).

### Componente testo modificabile

1. Apri il progetto SPA nell’IDE
1. Crea un componente React in `src/components/editable/core/Text.js`
1. Aggiungi il seguente codice a `Text.js`

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. Crea un componente React modificabile in `src/components/editable/EditableText.js`
1. Aggiungi il seguente codice a `EditableText.js`

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

L’implementazione del componente Testo modificabile deve essere simile alla seguente:

![Componente testo modificabile](./assets/spa-container-component/text-js.png)

### Componente immagine

1. Apri il progetto SPA nell’IDE
1. Crea un componente React in `src/components/editable/core/Image.js`
1. Aggiungi il seguente codice a `Image.js`

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. Crea un componente React modificabile in `src/components/editable/EditableImage.js`
1. Aggiungi il seguente codice a `EditableImage.js`

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. Creare un file SCSS `src/components/editable/EditableImage.scss` che fornisce stili personalizzati per `EditableImage.scss`. Questi stili sono destinati alle classi CSS del componente React modificabile.
1. Aggiungi il seguente SCSS a `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importa `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

L’implementazione del componente Immagine modificabile sarà simile alla seguente:

![Componente immagine modificabile](./assets/spa-container-component/image-js.png)


### Importare i componenti modificabili

Nell&#39;applicazione a pagina singola viene fatto riferimento ai componenti React `EditableText` e `EditableImage` appena creati e vengono create dinamicamente in base al JSON restituito da AEM. Per assicurarsi che questi componenti siano disponibili per l&#39;applicazione a pagina singola, creare le relative istruzioni di importazione in `Home.js`

1. Apri il progetto SPA nell’IDE
1. Apri il file in `src/Home.js`
1. Aggiungi istruzioni di importazione per `AEMText` e `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

Il risultato dovrebbe essere simile al seguente:

![Home.js](./assets/spa-container-component/home-js-imports.png)

Se queste importazioni sono _non_ aggiunte, il codice `EditableText` e `EditableImage` non può essere richiamato dall&#39;applicazione a pagina singola e, pertanto, i componenti non sono mappati ai tipi di risorsa forniti.

## Configurazione del contenitore in AEM

I componenti contenitore di AEM utilizzano i criteri per determinare i componenti consentiti. Si tratta di una configurazione critica quando si utilizza l’editor di applicazioni a pagina singola, in quanto solo i componenti di AEM che hanno mappato le controparti dei componenti di applicazioni a pagina singola possono essere sottoposti a rendering dall’applicazione a pagina singola. Assicurati che siano consentiti solo i componenti per i quali abbiamo fornito implementazioni di applicazioni a pagina singola:

* `EditableTitle` mappato a `wknd-app/components/title`
* `EditableText` mappato a `wknd-app/components/text`
* `EditableImage` mappato a `wknd-app/components/image`

Per configurare il contenitore reponsivegrid del modello Pagina applicazione a pagina singola remota:

1. Accedi ad AEM Author
1. Passa a __Strumenti > Generale > Modelli > App WKND__
1. Modifica __Pagina SPA report__

   ![Criteri griglia reattiva](./assets/spa-container-component/templates-remote-spa-page.png)

1. Seleziona __Struttura__ nel commutatore modalità in alto a destra
1. Tocca per selezionare il __Contenitore di layout__
1. Tocca l&#39;icona __Criterio__ nella barra a comparsa

   ![Criteri griglia reattiva](./assets/spa-container-component/templates-policies-action.png)

1. A destra, nella scheda __Componenti consentiti__, espandi __APP WKND - CONTENUTO__
1. Accertati che siano selezionati solo i seguenti elementi:
   1. Immagine
   1. Testo
   1. Titolo

   ![Pagina applicazione a pagina singola remota](./assets/spa-container-component/templates-allowed-components.png)

1. Tocca __Fine__

## Authoring del contenitore in AEM

Dopo l&#39;aggiornamento dell&#39;applicazione a pagina singola per incorporare `<ResponsiveGrid...>`, i wrapper per tre componenti React modificabili (`EditableTitle`, `EditableText` e `EditableImage`) e l&#39;aggiornamento di AEM con un criterio di modello corrispondente, è possibile iniziare a creare contenuti nel componente contenitore.

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND__
1. Tocca __Home__ e seleziona __Modifica__ dalla barra delle azioni superiore
   1. Viene visualizzato un componente Testo &quot;Hello World&quot;, che veniva aggiunto automaticamente durante la generazione del progetto dall’archetipo del progetto AEM
1. Seleziona __Modifica__ dal selettore modalità in alto a destra nell&#39;Editor pagina
1. Individua l&#39;area modificabile __Contenitore di layout__ sotto il titolo
1. Apri la barra laterale dell&#39;__Editor pagine__ e seleziona la __visualizzazione Componenti__
1. Trascina i seguenti componenti nel __Contenitore di layout__
   1. Immagine
   1. Titolo
1. Trascina i componenti per riordinarli nell’ordine seguente:
   1. Titolo
   1. Immagine
   1. Testo
1. __Crea__ il componente __Titolo__
   1. Tocca il componente Titolo, quindi tocca l&#39;icona __chiave inglese__ per __modificare__ il componente Titolo
   1. Aggiungere il testo seguente:
      1. Titolo: __L&#39;estate sta arrivando, sfruttiamola al massimo!__
      1. Tipo: __H1__
   1. Tocca __Fine__
1. __Crea__ il componente __Immagine__
   1. Trascina un’immagine in dalla barra laterale (dopo il passaggio alla vista Assets) sul componente Immagine
   1. Tocca il componente Immagine, quindi tocca l&#39;icona __chiave inglese__ per modificare
   1. Seleziona la casella di controllo __L&#39;immagine è decorativa__
   1. Tocca __Fine__
1. __Crea__ il componente __Testo__
   1. Modifica il componente Testo toccando il componente Testo e toccando l&#39;icona __chiave inglese__
   1. Aggiungere il testo seguente:
      1. _Al momento, puoi ottenere il 15% su tutte le avventure di 1 settimana e il 20% su tutte le avventure che durano 2 settimane o più! Al momento del pagamento, aggiungi il codice della campagna SUMMERISCOMING per ottenere i tuoi sconti!_
   1. Tocca __Fine__

1. I componenti ora sono creati, ma si sovrappongono in verticale.

   ![Componenti creati](./assets/spa-container-component/authored-components.png)

   Utilizza la modalità Layout di AEM per regolare le dimensioni e il layout dei componenti.

1. Passa a __Modalità layout__ utilizzando il selettore modalità in alto a destra
1. __Ridimensiona__ i componenti Immagine e Testo in modo che siano affiancati
   1. Il componente __Immagine__ deve avere __8 colonne__
   1. Il componente __Testo__ deve avere __3 colonne__

   ![Componenti layout](./assets/spa-container-component/layout-components.png)

1. __Visualizza in anteprima__ le modifiche nell&#39;Editor pagina di AEM
1. Aggiorna l&#39;app WKND in esecuzione localmente su [http://localhost:3000](http://localhost:3000) per visualizzare le modifiche create.

   ![Componente contenitore nell&#39;applicazione a pagina singola](./assets/spa-container-component/localhost-final.png)


## Congratulazioni.

Hai aggiunto un componente contenitore che consente agli autori di aggiungere componenti modificabili all’app WKND. Ora sai come:

* Utilizzare il componente `ResponsiveGrid` del componente modificabile AEM React nell&#39;applicazione a pagina singola
* Creare e registrare componenti React modificabili (testo e immagine) da utilizzare nell’applicazione a pagina singola tramite il componente contenitore
* Configura il modello Pagina applicazione a pagina singola remota per consentire i componenti abilitati per l’applicazione a pagina singola
* Aggiungere componenti modificabili al componente contenitore
* Componenti di authoring e layout nell’editor SPA

## Passaggi successivi

Il passaggio successivo utilizza la stessa tecnica per [aggiungere un componente modificabile a un percorso Dettagli avventura](./spa-dynamic-routes.md) nell&#39;applicazione a pagina singola.
