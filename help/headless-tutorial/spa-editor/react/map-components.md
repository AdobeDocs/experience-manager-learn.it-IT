---
title: Mappatura di componenti SPA per AEM componenti | Guida introduttiva all'editor di SPA AEM e React
description: Scopri come mappare i componenti React ai componenti di Adobe Experience Manager (AEM) con l’SDK JS dell’editor di AEM SPA. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA all’interno dell’editor di SPA AEM, in modo simile all’authoring tradizionale AEM. Scoprirai anche come utilizzare i componenti core React AEM pronti all’uso.
sub-product: sites
feature: Editor SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2267'
ht-degree: 1%

---


# Mappatura di componenti SPA per AEM componenti {#map-components}

Scopri come mappare i componenti React ai componenti di Adobe Experience Manager (AEM) con l’SDK JS dell’editor di AEM SPA. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA all’interno dell’editor di SPA AEM, in modo simile all’authoring tradizionale AEM.

Questo capitolo descrive in modo più approfondito l’API del modello JSON AEM e come il contenuto JSON esposto da un componente AEM può essere inserito automaticamente in un componente React come prop.

## Obiettivo

1. Scopri come mappare AEM componenti su SPA componenti.
1. Come un componente React utilizza le proprietà dinamiche passate da AEM.
1. Scopri come utilizzare i [React AEM Core Components](https://github.com/adobe/aem-react-core-wcm-components-examples).

## Cosa verrà creato

Questo capitolo analizzerà come il componente `Text` SPA fornito viene mappato sul componente AEM `Text`. React I componenti core come il componente `Image` SPA verranno utilizzati nella SPA e creati in AEM. Le funzioni predefinite dei criteri **Contenitore di layout** e **Editor modelli** verranno utilizzate anche per creare una visualizzazione con aspetto leggermente più vario.

![Esempio di capitolo di authoring finale](./assets/map-components/final-page.png)

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment). Questo capitolo è una continuazione del capitolo [Integrare il capitolo SPA](integrate-spa.md), tuttavia per seguire tutto ciò di cui hai bisogno è un progetto AEM abilitato SPA.

## Approccio di mappatura

Il concetto di base consiste nel mappare un componente SPA a un componente AEM. AEM componenti, esegui lato server, esporta il contenuto come parte dell’API del modello JSON. Il contenuto JSON viene utilizzato dal SPA, che esegue lato client nel browser. Viene creata una mappatura 1:1 tra SPA componenti e un componente AEM.

![Panoramica di alto livello sulla mappatura di un componente AEM a un componente React](./assets/map-components/high-level-approach.png)

*Panoramica di alto livello sulla mappatura di un componente AEM a un componente React*

## Inspect come componente testo

Il [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) fornisce un componente `Text` mappato al componente AEM [Testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Questo è un esempio di un componente **content**, in quanto esegue il rendering di *content* da AEM.

Vediamo come funziona il componente.

### Inspect il modello JSON

1. Prima di passare al codice SPA, è importante comprendere il modello JSON AEM fornito. Passa alla [Libreria di componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) e visualizza la pagina per il componente Testo . La libreria dei componenti core fornisce esempi di tutti i componenti core AEM.
1. Seleziona la scheda **JSON** per uno degli esempi seguenti:

   ![Modello JSON per testo](./assets/map-components/text-json.png)

   Dovresti visualizzare tre proprietà: `text`, `richText` e `:type`.

   `:type` è una proprietà riservata che elenca il  `sling:resourceType` (o percorso) del componente AEM. Il valore di `:type` viene utilizzato per mappare il componente AEM al componente SPA.

   `text` e  `richText` sono proprietà aggiuntive che verranno esposte al componente SPA.

1. Visualizza l&#39;output JSON in [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Dovresti trovare una voce simile a:

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspect il componente Testo SPA

1. Nell’IDE che preferisci, apri il Progetto AEM per l’SPA. Espandi il modulo `ui.frontend` e apri il file `Text.js` in `ui.frontend/src/components/Text/Text.js`.

1. La prima area che esamineremo è la sezione `class Text` a ~linea 40:

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` è un componente React standard. Il componente utilizza `this.props.richText` per determinare se il contenuto di cui eseguire il rendering sarà in formato RTF o testo normale. Il &quot;contenuto&quot; effettivo utilizzato proviene da `this.props.text`.

   Per evitare un potenziale attacco XSS, il testo RTF viene escape tramite `DOMPurify` prima di utilizzare [pericolosamenteSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) per eseguire il rendering del contenuto. Richiama le proprietà `richText` e `text` dal modello JSON precedente nell’esercizio.

1. Dai un&#39;occhiata alla sezione `TextEditConfig` su ~line 29:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Il codice riportato sopra ha la responsabilità di determinare quando eseguire il rendering del segnaposto nell’ambiente di authoring AEM. Se il metodo `isEmpty` restituisce **true**, viene eseguito il rendering del segnaposto.

1. Infine, dai un&#39;occhiata alla chiamata `MapTo` su ~line 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` è fornito dall’SDK JS AEM Editor (`@adobe/aem-react-editable-components`). Il percorso `wknd-spa-react/components/text` rappresenta il `sling:resourceType` del componente AEM. Questo percorso viene confrontato con il `:type` esposto dal modello JSON osservato in precedenza. `MapTo` si occupa di analizzare la risposta del modello JSON e passare i valori corretti  `props` al componente SPA.

   Puoi trovare la definizione del componente AEM `Text` in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Utilizzare i componenti core React

[AEM componenti WCM - React ](https://github.com/adobe/aem-react-core-wcm-components-base) Implementazione di base e  [AEM componenti WCM - Editor Spa - React Implementazione](https://github.com/adobe/aem-react-core-wcm-components-spa) di base. Si tratta di un set di componenti dell’interfaccia utente riutilizzabili che vengono mappati su componenti AEM predefiniti. La maggior parte dei progetti può riutilizzare questi componenti come punto di partenza per la propria implementazione.

1. Nel codice del progetto apri il file `import-components.js` in `ui.frontend/src/components`.
Questo file importa tutti i componenti SPA che vengono mappati su componenti AEM. Data la natura dinamica dell’implementazione dell’editor di SPA, dobbiamo fare riferimento esplicitamente a qualsiasi componente SPA legato a componenti AEM modificabili dall’autore. Questo consente all’autore AEM di scegliere di utilizzare un componente ovunque si desideri nell’applicazione.
1. Le seguenti istruzioni di importazione includono SPA componenti scritti nel progetto:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Sono disponibili diverse altre `imports` tra `@adobe/aem-core-components-react-spa` e `@adobe/aem-core-components-react-base`. Si tratta dell’importazione dei componenti React Core e della loro disponibilità nel progetto corrente. Questi vengono quindi mappati su componenti AEM specifici del progetto utilizzando `MapTo`, proprio come nell&#39;esempio precedente del componente `Text` .

### Aggiorna criteri AEM

I criteri sono una funzione dei modelli AEM che offre agli sviluppatori e agli utenti un controllo granulare sui componenti disponibili per l’utilizzo. I componenti core React sono inclusi nel codice SPA ma devono essere attivati tramite un criterio prima di poter essere utilizzati nell’applicazione.

1. Dalla schermata iniziale AEM passare a **Strumenti** > **Modelli** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Seleziona e apri il modello **SPA Pagina** per la modifica.

1. Seleziona il **Contenitore di layout** e fai clic sull&#39;icona **policy** per modificare il criterio:

   ![criterio contenitore layout](assets/map-components/edit-spa-page-template.png)

1. In **Componenti consentiti** > **WKND SPA React - Content** > controlla **Immagine**, **Teaser** e **Titolo**.

   ![Componenti aggiornati disponibili](assets/map-components/update-components-available.png)

   In **Componenti predefiniti** > **Aggiungi mappatura** e scegli il componente **Immagine - WKND SPA React - Contenuto** :

   ![Imposta componenti predefiniti](./assets/map-components/default-components.png)

   Immetti un **tipo MIME** di `image/*`.

   Fai clic su **Fine** per salvare gli aggiornamenti dei criteri.

1. Nel **Contenitore di layout** fai clic sull&#39;icona **policy** per il componente **Testo** .

   Crea un nuovo criterio denominato **WKND SPA Text**. Alla voce **Plugin** > **Formattazione** > controlla tutte le caselle per abilitare ulteriori opzioni di formattazione:

   ![Abilita formattazione RTE](assets/map-components/enable-formatting-rte.png)

   Alla voce **Plugin** > **Stili di paragrafo** > seleziona la casella per **Abilitare gli stili di paragrafo**:

   ![Abilita stili di paragrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Fai clic su **Fine** per salvare l&#39;aggiornamento dei criteri.

### Contenuto autore

1. Passa alla **Home page** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Ora dovresti essere in grado di utilizzare i componenti aggiuntivi **Immagine**, **Teaser** e **Titolo** sulla pagina.

   ![Componenti aggiuntivi](assets/map-components/additional-components.png)

1. È inoltre necessario poter modificare il componente `Text` e aggiungere stili di paragrafo aggiuntivi in modalità **a schermo intero**.

   ![Modifica RTF a schermo intero](assets/map-components/full-screen-rte.png)

1. È inoltre necessario essere in grado di trascinare e rilasciare un&#39;immagine da **Asset Finder**:

   ![Trascina immagine](assets/map-components/drag-drop-image.png)

1. Esperienza con i componenti **Titolo** e **Teaser** .

1. Aggiungi le tue immagini tramite [AEM Assets](http://localhost:4502/assets.html/content/dam) o installa la base di codice finita per il sito di riferimento standard [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Il [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) include molte immagini che possono essere riutilizzate sul SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect il contenitore di layout

Il supporto per il **Contenitore di layout** viene fornito automaticamente dall’SDK dell’editor di SPA AEM. Il **Contenitore di layout**, come indicato dal nome, è un componente **contenitore**. I componenti contenitore sono componenti che accettano strutture JSON che rappresentano *altri componenti* e le creano in modo dinamico un’istanza.

Esaminiamo ulteriormente il Contenitore di layout.

1. Nel browser passa a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API del modello JSON - Griglia reattiva](./assets/map-components/responsive-grid-modeljson.png)

   Il componente **Contenitore di layout** ha un `sling:resourceType` di `wcm/foundation/components/responsivegrid` ed è riconosciuto dall’editor SPA utilizzando la proprietà `:type` , proprio come i componenti `Text` e `Image` .

   Le stesse funzionalità di ridimensionamento di un componente utilizzando [Modalità layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sono disponibili con l’Editor di SPA.

2. Torna a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Aggiungi altri componenti **Immagine** e prova a ridimensionarli utilizzando l&#39;opzione **Layout** :

   ![Ridimensionare l’immagine utilizzando la modalità Layout](./assets/map-components/responsive-grid-layout-change.gif)

3. Riapri il modello JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e osserva il `columnClassNames` come parte del JSON:

   ![Nomi delle classi cloud](./assets/map-components/responsive-grid-classnames.png)

   Il nome della classe `aem-GridColumn--default--4` indica che il componente deve essere largo 4 colonne in base a una griglia a 12 colonne. Ulteriori dettagli sulla [griglia reattiva sono disponibili qui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Torna all’IDE e nel modulo `ui.apps` è presente una libreria lato client definita in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Aprire il file `less/grid.less`.

   Questo file determina i punti di interruzione (`default`, `tablet` e `phone`) utilizzati dal **Contenitore di layout**. Questo file deve essere personalizzato in base alle specifiche del progetto. Attualmente i punti di interruzione sono impostati su `1200px` e `768px`.

5. Dovresti essere in grado di utilizzare le funzionalità reattive e i criteri di testo RTF aggiornati del componente `Text` per creare una visualizzazione come la seguente:

   ![Esempio di capitolo di authoring finale](assets/map-components/final-page.png)

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato a mappare SPA componenti su componenti AEM e hai utilizzato i componenti core React. Hai anche la possibilità di esplorare le funzionalità reattive del **Contenitore di layout**.

### Passaggi successivi {#next-steps}

[Navigazione e routing](navigation-routing.md) : scopri come è possibile supportare più visualizzazioni nel SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica è implementata utilizzando i componenti core React Router e React.

## (Bonus) Persistere configurazioni al controllo del codice sorgente {#bonus-configs}

In molti casi, specialmente all’inizio di un progetto AEM, è utile mantenere le configurazioni, come i modelli e i relativi criteri dei contenuti, al controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano con lo stesso set di contenuti e configurazioni e possono garantire un’ulteriore coerenza tra gli ambienti. Quando un progetto raggiunge un certo livello di maturità, la gestione dei modelli può essere affidata a un gruppo speciale di utenti.

I passaggi successivi si svolgeranno utilizzando l&#39;IDE di codice di Visual Studio e [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) ma potrebbero essere eseguiti utilizzando qualsiasi strumento e qualsiasi IDE configurato per **pull** o **import** da un&#39;istanza locale di AEM.

1. Nell&#39;IDE di codice di Visual Studio, assicurati di aver installato **VSCode AEM Sync** tramite l&#39;estensione Marketplace:

   ![Sincronizzazione AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Espandi il modulo **ui.content** in Project explorer e passa a `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Fai clic con** il pulsante destro del mouse sulla  `templates` cartella e seleziona  **Importa da AEM server**:

   ![Modello di importazione VSCode](./assets/map-components/import-aem-servervscode.png)

4. Ripeti i passaggi per importare il contenuto, ma seleziona la cartella **policy** che si trova in `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect il file `filter.xml` che si trova in `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   Il file `filter.xml` è responsabile dell&#39;identificazione dei percorsi dei nodi che verranno installati con il pacchetto. Osserva `mode="merge"` su ciascuno dei filtri che indica che il contenuto esistente non verrà modificato, ma che viene aggiunto solo un nuovo contenuto. Poiché gli autori di contenuti possono aggiornare questi percorsi, è importante che una distribuzione di codice sovrascriva il contenuto **non**. Per ulteriori informazioni sull&#39;utilizzo degli elementi filtro, consulta la [documentazione FileVault](https://jackrabbit.apache.org/filevault/filter.html) .

   Confronta `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` per comprendere i diversi nodi gestiti da ciascun modulo.

## (Bonus) Crea componente immagine personalizzato {#bonus-image}

Un componente Immagine SPA è già stato fornito dai componenti React Core. Tuttavia, se desideri una procedura aggiuntiva, crea la tua implementazione React che sia associata al AEM [componente Immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). Il componente `Image` è un altro esempio di un componente **content** .

### Inspect JSON

Prima di passare al codice SPA, controlla il modello JSON fornito da AEM.

1. Passa a [Esempi di immagini nella libreria Componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON del componente core immagine](./assets/map-components/image-json.png)

   Le proprietà di `src`, `alt` e `title` verranno utilizzate per popolare il componente SPA `Image`.

   >[!NOTE]
   >
   > Esistono altre proprietà immagine esposte (`lazyEnabled`, `widths`) che consentono a uno sviluppatore di creare un componente adattivo e a caricamento lento. Il componente creato in questa esercitazione sarà semplice e **non** utilizzerà queste proprietà avanzate.

### Implementare il componente Immagine

1. Quindi, crea una nuova cartella denominata `Image` in `ui.frontend/src/components`.
1. Sotto la cartella `Image` crea un nuovo file denominato `Image.js`.

   ![File Image.js](./assets/map-components/image-js-file.png)

1. Aggiungi le seguenti istruzioni `import` a `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. Quindi aggiungi il `ImageEditConfig` per determinare quando visualizzare il segnaposto in AEM:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   Il segnaposto viene visualizzato se la proprietà `src` non è impostata.

1. Implementa quindi la classe `Image` :

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   Il codice di cui sopra eseguirà il rendering di un `<img>` in base alle proprietà `src`, `alt` e `title` passate dal modello JSON.

1. Aggiungi il codice `MapTo` per mappare il componente React al componente AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   La stringa `wknd-spa-react/components/image` corrisponde alla posizione del componente AEM in `ui.apps` in: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Crea un nuovo file denominato `Image.css` nella stessa directory e aggiungi quanto segue:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. In `Image.js` aggiungi un riferimento al file nella parte superiore sotto le istruzioni `import` :

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Apri il file `ui.frontend/src/components/import-components.js` e aggiungi un riferimento al nuovo componente `Image`:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. In `import-components.js` commenta l’immagine del componente di base React:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   In questo modo verrà utilizzato il componente Immagine personalizzato.

1. Dalla radice del progetto distribuisci il codice SPA per AEM utilizzando Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspect il SPA in AEM. Eventuali componenti Immagine nella pagina continueranno a funzionare. Inspect visualizza l’output di cui è stato effettuato il rendering ed è necessario visualizzare il markup per il componente Immagine personalizzato invece del componente core React.

   *Marcatura del componente Immagine personalizzato*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *Marcatura immagine del componente core React*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   Questa è una buona introduzione all’estensione e all’implementazione dei componenti personalizzati.

