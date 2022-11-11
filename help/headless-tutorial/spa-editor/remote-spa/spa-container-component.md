---
title: Aggiungere componenti contenitore React modificabili a un SPA remoto
description: Scopri come aggiungere componenti contenitore modificabili a un SPA remoto che consente AEM autori di trascinarvi componenti.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 2%

---

# Componenti contenitore modificabili

[Componenti fissi](./spa-fixed-component.md) offre una certa flessibilità per l’authoring dei contenuti SPA, tuttavia questo approccio è rigido e richiede agli sviluppatori di definire con precisione la composizione dei contenuti modificabili. Per supportare la creazione di esperienze eccezionali da parte degli autori, SPA Editor supporta l’utilizzo di componenti contenitore nella SPA. I componenti contenitore consentono agli autori di trascinare e rilasciare i componenti consentiti nel contenitore e di crearli, proprio come nelle tradizionali funzioni di authoring di AEM Sites.

![Componenti contenitore modificabili](./assets/spa-container-component/intro.png)

In questo capitolo, aggiungiamo un contenitore modificabile alla vista home che consente agli autori di comporre e impostare il layout di esperienze di contenuti avanzati utilizzando i componenti React modificabili direttamente nel SPA.

## Aggiornare l’app WKND

Per aggiungere un componente contenitore alla vista Home:

+ Importa i componenti AEM React Modificabili `ResponsiveGrid` component
+ Importare e registrare componenti React modificabili personalizzati (Testo e Immagine) da utilizzare nel componente ResponsiveGrid

### Utilizzare il componente ResponsiveGrid

Per aggiungere un’area modificabile alla vista Home:

1. Apri e modifica `react-app/src/components/Home.js`
1. Importa `ResponsiveGrid` componente da `@adobe/aem-react-editable-components` e aggiungilo al `Home` componente.
1. Imposta i seguenti attributi nel `<ResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Questo istruisce il `ResponsiveGrid` per recuperare il relativo contenuto dalla risorsa AEM:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   La `itemPath` mappe `responsivegrid` nodo definito nel `Remote SPA Page` Modello AEM e viene creato automaticamente sulle nuove AEM Pagine create da `Remote SPA Page` Modello AEM.

   Aggiorna `Home.js` per aggiungere `<ResponsiveGrid...>` componente.

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

La `Home.js` dovrebbe essere simile a:

![Home.js](./assets/spa-container-component/home-js.png)

## Creare componenti modificabili

Per ottenere l’effetto completo dei contenitori di esperienza di authoring flessibili forniti in SPA Editor. È già stato creato un componente Titolo modificabile, ma ne sono disponibili alcuni altri che consentono agli autori di utilizzare i componenti Testo e Immagine modificabili nel componente Griglia dinamica appena aggiunto.

I nuovi componenti Testo e Reazione immagine modificabili vengono creati utilizzando il pattern di definizione del componente modificabile esportato in [componenti fissi modificabili](./spa-fixed-component.md).

### Componente testo modificabile

1. Apri il progetto SPA nell’IDE
1. Crea un componente React in `src/components/editable/core/Text.js`
1. Aggiungi il codice seguente a `Text.js`

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
1. Aggiungi il codice seguente a `EditableText.js`

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

L’implementazione del componente Testo modificabile avrà l’aspetto seguente:

![Componente testo modificabile](./assets/spa-container-component/text-js.png)

### Componente immagine

1. Apri il progetto SPA nell’IDE
1. Crea un componente React in `src/components/editable/core/Image.js`
1. Aggiungi il codice seguente a `Image.js`

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
1. Aggiungi il codice seguente a `EditableImage.js`

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

L’implementazione del componente Immagine modificabile avrà l’aspetto seguente:

![Componente immagine modificabile](./assets/spa-container-component/image-js.png)


### Importare i componenti modificabili

La nuova creazione `EditableText` e `EditableImage` I componenti React sono indicati in SPA e vengono istanziati dinamicamente in base al JSON restituito da AEM. Per garantire che questi componenti siano disponibili per l’SPA, crea istruzioni di importazione per tali componenti in `Home.js`

1. Apri il progetto SPA nell’IDE
1. Aprire il file `src/Home.js`
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

Se queste importazioni sono _not_ inoltre, `EditableText` e `EditableImage` Il codice non viene richiamato da SPA, pertanto i componenti non sono mappati ai tipi di risorse specificati.

## Configurazione del contenitore in AEM

I componenti contenitore AEM utilizzano i criteri per dettare i componenti consentiti. Si tratta di una configurazione critica quando si utilizza SPA Editor, in quanto solo i componenti AEM che hanno mappato SPA controparti dei componenti possono essere sottoposti a rendering dal SPA. Assicurati che siano consentiti solo i componenti per i quali abbiamo fornito SPA implementazioni:

+ `EditableTitle` mappato su `wknd-app/components/title`
+ `EditableText` mappato su `wknd-app/components/text`
+ `EditableImage` mappato su `wknd-app/components/image`

Per configurare il contenitore reponsivegrid del modello di pagina SPA remota:

1. Accedi ad AEM Author
1. Passa a __Strumenti > Generale > Modelli > App WKND__
1. Modifica __Pagina Report SPA__

   ![Criteri della griglia reattiva](./assets/spa-container-component/templates-remote-spa-page.png)

1. Seleziona __Struttura__ nel commutatore di modalità in alto a destra
1. Tocca per selezionare la __Contenitore di layout__
1. Tocca __Criterio__ icona nella barra a comparsa

   ![Criteri della griglia reattiva](./assets/spa-container-component/templates-policies-action.png)

1. Sulla destra, sotto il __Componenti consentiti__ scheda, espandi __APP WKND - CONTENUTO__
1. Assicurati che siano selezionati solo i seguenti elementi:
   + Immagine
   + Testo
   + Titolo

   ![Pagina SPA remota](./assets/spa-container-component/templates-allowed-components.png)

1. Toccate __Chiudi__

## Creazione del contenitore in AEM

Dopo l’aggiornamento del SPA per incorporare il `<ResponsiveGrid...>`, wrapper per tre componenti React modificabili (`EditableTitle`, `EditableText`e `EditableImage`) e AEM viene aggiornato con un criterio Modello corrispondente, possiamo iniziare a creare contenuto nel componente contenitore .

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND__
1. Tocca __Pagina principale__ e seleziona __Modifica__ dalla barra delle azioni superiore
   + Viene visualizzato un componente Testo &quot;Ciao a tutti&quot;, che viene aggiunto automaticamente durante la generazione del progetto dall’archetipo AEM progetto
1. Seleziona __Modifica__ dal selettore modalità in alto a destra dell’Editor pagina
1. Individua il __Contenitore di layout__ area modificabile sotto il titolo
1. Apri __Barra laterale dell’Editor pagina__, quindi seleziona la __Vista Componenti__
1. Trascina i seguenti componenti nel __Contenitore di layout__
   + Immagine
   + Titolo
1. Trascina i componenti per riordinarli nell’ordine seguente:
   1. Titolo
   1. Immagine
   1. Testo
1. __Autore__ la __Titolo__ component
   1. Tocca il componente Titolo e tocca il __chiave__ icona a __modifica__ il componente Titolo
   1. Aggiungi il testo seguente:
      + Titolo: __L&#39;estate sta arrivando, sfruttiamo al massimo!__
      + Tipo: __H1__
   1. Toccate __Chiudi__
1. __Autore__ la __Immagine__ component
   1. Trascina un’immagine dalla barra laterale (dopo il passaggio alla visualizzazione Risorse) sul componente Immagine
   1. Tocca il componente Immagine e tocca il __chiave__ icona da modificare
   1. Controlla la __L&#39;immagine è decorativa__ spunta
   1. Toccate __Chiudi__
1. __Autore__ la __Testo__ component
   1. Per modificare il componente Testo , toccate il componente Testo e toccate il pulsante __chiave__ icona
   1. Aggiungi il testo seguente:
      + _In questo momento, è possibile ottenere il 15% su tutte le avventure di 1 settimana, e il 20% di sconto su tutte le avventure che sono 2 settimane o più! Al momento del pagamento, aggiungi il codice della campagna SOMMERISVENING per ottenere i tuoi sconti!_
   1. Toccate __Chiudi__

1. I componenti vengono ora creati, ma vengono sovrapposti verticalmente.

   ![Componenti creati](./assets/spa-container-component/authored-components.png)

Utilizza AEM modalità Layout per modificare le dimensioni e il layout dei componenti.

1. Passa a __Modalità Layout__ utilizzo del selettore modalità in alto a destra
1. __Ridimensiona__ i componenti Immagine e Testo, in modo che siano affiancati
   + __Immagine__ dovrebbe essere __8 colonne__
   + __Testo__ dovrebbe essere __3 colonne__

   ![Componenti di layout](./assets/spa-container-component/layout-components.png)

1. __Anteprima__ le modifiche in AEM Editor pagina
1. Aggiorna l’app WKND in esecuzione localmente su [http://localhost:3000](Http://localhost:3000) per visualizzare le modifiche create!

   ![Componente contenitore in SPA](./assets/spa-container-component/localhost-final.png)


## Congratulazioni. 

Hai aggiunto un componente contenitore che consente agli autori di aggiungere componenti modificabili all’app WKND. Ora sai come:

+ Utilizza i componenti AEM React Modificabili `ResponsiveGrid` nel SPA
+ Creare e registrare componenti React modificabili (Testo e Immagine) da utilizzare nel SPA tramite il componente contenitore
+ Configurare il modello Pagina SPA remota per consentire i componenti abilitati per SPA
+ Aggiungere componenti modificabili al componente contenitore
+ Componenti di authoring e layout nell’Editor SPA

## Passaggi successivi

Il passaggio successivo utilizza la stessa tecnica per [aggiungi un componente modificabile a un percorso Dettagli avventura](./spa-dynamic-routes.md) nel SPA.
