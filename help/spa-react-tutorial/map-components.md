---
title: Mappatura di componenti SPA su componenti AEM | Guida introduttiva dell’Editor SPA di AEM e React
description: Scopri come mappare i componenti React ai componenti di Adobe Experience Manager (AEM) con l’SDK JS per AEM SPA Editor. La mappatura dei componenti consente agli utenti di effettuare aggiornamenti dinamici ai componenti SPA all’interno dell’Editor SPA di AEM, in modo simile alla tradizionale creazione di AEM.
sub-product: sites
feature: Editor SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2264'
ht-degree: 2%

---


# Mappatura di componenti SPA su componenti AEM {#map-components}

Scopri come mappare i componenti React ai componenti di Adobe Experience Manager (AEM) con l’SDK JS per AEM SPA Editor. La mappatura dei componenti consente agli utenti di effettuare aggiornamenti dinamici ai componenti SPA all’interno dell’Editor SPA di AEM, in modo simile alla tradizionale creazione di AEM.

Questo capitolo illustra in modo più approfondito l’API del modello JSON di AEM e il modo in cui il contenuto JSON esposto da un componente AEM può essere inserito automaticamente in un componente React come prop.

## Obiettivo

1. Scopri come mappare i componenti AEM su componenti SPA.
2. Comprendi la differenza tra i componenti **Contenitore** e i componenti **Contenuto**.
3. Crea un nuovo componente React da mappare a un componente AEM esistente.

## Cosa verrà creato

Questo capitolo analizza come il componente `Text` SPA fornito viene mappato sul componente AEM `Text`. Verrà creato un nuovo componente `Image` SPA che può essere utilizzato nell’applicazione a pagina singola e creato in AEM. Le funzioni predefinite dei criteri **Contenitore di layout** e **Editor modelli** verranno utilizzate anche per creare una visualizzazione con aspetto leggermente più vario.

![Esempio di capitolo di authoring finale](./assets/map-components/final-page.png)

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. Distribuisci la base di codice in un’istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) o estrarre il codice localmente passando al ramo `React/map-components-solution`.

## Approccio di mappatura

Il concetto di base è quello di mappare un componente SPA su un componente AEM. I componenti AEM, esegui lato server, esportano il contenuto come parte dell’API del modello JSON. Il contenuto JSON viene utilizzato dall’applicazione a pagina singola, che esegue lato client nel browser. Viene creata una mappatura 1:1 tra i componenti SPA e un componente AEM.

![Panoramica di alto livello sulla mappatura di un componente AEM su un componente React](./assets/map-components/high-level-approach.png)

*Panoramica di alto livello sulla mappatura di un componente AEM su un componente React*

## Ispezionare il componente testo

Il [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) fornisce un componente `Text` mappato al componente di testo AEM [a4/>. ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html) Questo è un esempio di un componente **content**, in quanto esegue il rendering di *content* da AEM.

Vediamo come funziona il componente.

### Ispezionare il modello JSON

1. Prima di passare al codice SPA, è importante comprendere il modello JSON fornito da AEM. Passa alla [Libreria di componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) e visualizza la pagina per il componente Testo . La libreria dei componenti core fornisce esempi di tutti i componenti core di AEM.
2. Seleziona la scheda **JSON** per uno degli esempi seguenti:

   ![Modello JSON per testo](./assets/map-components/text-json.png)

   Dovresti visualizzare tre proprietà: `text`, `richText` e `:type`.

   `:type` è una proprietà riservata che elenca il  `sling:resourceType` (o percorso) del componente AEM. Il valore di `:type` viene utilizzato per mappare il componente AEM sul componente SPA.

   `text` e  `richText` sono proprietà aggiuntive che saranno esposte al componente SPA.

### Ispezionare il componente Testo

1. Apri un nuovo terminale e passa alla cartella `ui.frontend` all’interno del progetto. Esegui `npm install` e quindi `npm start` per avviare il **webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   Il modulo `ui.frontend` è attualmente configurato per utilizzare il modello [JSON di simulazione](./integrate-spa.md#mock-json).

2. Dovresti visualizzare una nuova finestra del browser aperta su [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Server di sviluppo Webpack con contenuti fittizi](./assets/map-components/initial-start.png)

3. Nell’IDE che preferisci, apri il progetto AEM per l’applicazione a pagina singola WKND. Espandi il modulo `ui.frontend` e apri il file `Text.js` in `ui.frontend/src/components/Text/Text.js`:

   ![Codice sorgente del componente React di Text.js](./assets/map-components/vscode-ide-text-js.png)

4. La prima area che esamineremo è la sezione `class Text` a ~linea 40:

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

   `Text` è un componente React standard. Il componente utilizza `this.props.richText` per determinare se il contenuto di cui eseguire il rendering sarà in formato RTF o testo normale. Il &quot;contenuto&quot; effettivo utilizzato proviene da `this.props.text`. Per evitare un potenziale attacco XSS, il testo RTF viene escape tramite `DOMPurify` prima di utilizzare [pericolosamenteSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) per eseguire il rendering del contenuto. Richiama le proprietà `richText` e `text` dal modello JSON precedente nell’esercizio.

5. Dai un&#39;occhiata alla sezione `TextEditConfig` su ~line 29:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Il codice di cui sopra è responsabile per determinare quando eseguire il rendering del segnaposto nell’ambiente di authoring di AEM. Se il metodo `isEmpty` restituisce **true**, viene eseguito il rendering del segnaposto.

6. Infine, dai un&#39;occhiata alla chiamata `MapTo` su ~line 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` è fornito dall’SDK JS di AEM SPA Editor (`@adobe/aem-react-editable-components`). Il percorso `wknd-spa-react/components/text` rappresenta il `sling:resourceType` del componente AEM. Questo percorso viene confrontato con il `:type` esposto dal modello JSON osservato in precedenza. `MapTo` si occupa di analizzare la risposta del modello JSON e passare i valori corretti  `props` al componente SPA.

   Puoi trovare la definizione del componente AEM `Text` in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

7. Prova a modificare il file `mock.model.json` in `ui.frontend/public/mock-content/mock.model.json`. Alla riga 62 ~aggiorna il primo valore `Text` per utilizzare un tag **`H1`** e **`u`**:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   Passa a [http://localhost:3000](Http://localhost:3000) per visualizzare gli effetti:

   ![Modello di testo aggiornato](./assets/map-components/updated-text-model.png)

   Prova a attivare/disattivare la proprietà `richText` tra **true** / **false** per visualizzare la logica di rendering in azione.

8. Ispeziona `Text.scss` in `ui.frontend/src/components/Text/Text.scss`.

   Questo file è stato aggiunto dalla base di codice iniziale per questo capitolo e utilizza la funzione [Sass](https://sass-lang.com/) aggiunta nel capitolo precedente. Tieni presente le variabili a cui si fa riferimento da `ui.frontend/src/styles/_variables.scss`.

## Creare il componente Immagine

Quindi, crea un componente `Image` React mappato al componente AEM [Immagine](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/components/image.html). Il componente `Image` è un altro esempio di un componente **content** .

### Ispezionare il JSON

Prima di passare al codice SPA, controlla il modello JSON fornito da AEM.

1. Passa a [Esempi di immagini nella libreria Componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON del componente core immagine](./assets/map-components/image-json.png)

   Le proprietà di `src`, `alt` e `title` verranno utilizzate per popolare il componente SPA `Image`.

   >[!NOTE]
   >
   > Esistono altre proprietà immagine esposte (`lazyEnabled`, `widths`) che consentono a uno sviluppatore di creare un componente adattivo e a caricamento lento. Il componente creato in questa esercitazione sarà semplice e **non** utilizzerà queste proprietà avanzate.

2. Torna all’IDE e apri `mock.model.json` in `ui.frontend/public/mock-content/mock.model.json`. Poiché si tratta di un nuovo componente di rete per il nostro progetto, dobbiamo &quot;deridere&quot; l’immagine JSON.

   A ~riga 70 aggiungi una voce JSON per il modello `image` (non dimenticare la virgola finale `,` dopo il secondo `text_23828680`) e aggiorna l’array `:itemsOrder`.

   ```json
   ...
   ":items": {
               ...
               "text_23828680": {
                   "text": "<p>Mock Model JSON!</p>",
                   "richText": true,
                   ":type": "wknd-spa-react/components/text"
               },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mock-content/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-react/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_23828680",
               "image"
           ],
   ```

   Il progetto include un&#39;immagine di esempio in `/mock-content/adobestock-140634652.jpeg` che verrà utilizzata con il **webpack-dev-server**.

   È possibile visualizzare l&#39;intero [mock.model.json qui](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json).

### Implementare il componente Immagine

1. Quindi, crea una nuova cartella denominata `Image` in `ui.frontend/src/components`.
2. Sotto la cartella `Image` crea un nuovo file denominato `Image.js`.

   ![File Image.js](./assets/map-components/image-js-file.png)

3. Aggiungi le seguenti istruzioni `import` a `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. Quindi aggiungi il `ImageEditConfig` per determinare quando mostrare il segnaposto in AEM:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   Il segnaposto viene visualizzato se la proprietà `src` non è impostata.

5. Implementa quindi la classe `Image` :

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

6. Aggiungi il codice `MapTo` per mappare il componente React al componente AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   La stringa `wknd-spa-react/components/image` corrisponde alla posizione del componente AEM in `ui.apps` in: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. Crea un nuovo file denominato `Image.scss` nella stessa directory e aggiungi quanto segue:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. In `Image.js` aggiungi un riferimento al file nella parte superiore sotto le istruzioni `import` :

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   Puoi visualizzare il file [Image.js completato qui](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js).

9. Apri il file `ui.frontend/src/components/import-components.js` e aggiungi un riferimento al nuovo componente `Image`:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. Se non è già stato avviato, avviare il **webpack-dev-server**. Vai a [http://localhost:3000](Http://localhost:3000) e dovresti vedere il rendering dell&#39;immagine:

   ![Immagine aggiunta al simulacro](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **Sfida** bonus: Implementa un nuovo metodo in  `Image.js` per visualizzare il valore di  `this.props.title` come didascalia sotto l’immagine.

## Aggiornare i criteri in AEM

Il componente `Image` è visibile solo nel **webpack-dev-server**. Quindi, distribuisci l’applicazione a pagina singola aggiornata in AEM e aggiorna i criteri dei modelli.

1. Interrompi il **webpack-dev-server** e dalla directory principale del progetto distribuisci le modifiche ad AEM utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Dalla schermata iniziale di AEM, passa a **Strumenti** > **Modelli** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

   Seleziona e modifica la pagina **SPA**:

   ![Modifica modello di pagina SPA](./assets/map-components/edit-spa-page-template.png)

3. Seleziona il **Contenitore di layout** e fai clic sull&#39;icona **policy** per modificare il criterio:

   ![Criterio contenitore di layout](./assets/map-components/layout-container-policy.png)

4. Alla voce **Componenti consentiti** > **Reazione SPA WKND - Contenuto** > controlla il componente **Immagine**:

   ![Componente immagine selezionato](./assets/map-components/check-image-component.png)

   In **Componenti predefiniti** > **Aggiungi mappatura** e scegli il componente **Immagine - Reazione SPA WKND - Contenuto** :

   ![Imposta componenti predefiniti](./assets/map-components/default-components.png)

   Immetti un **tipo MIME** di `image/*`.

   Fai clic su **Fine** per salvare gli aggiornamenti dei criteri.

5. Nel **Contenitore di layout** fai clic sull&#39;icona **policy** per il componente **Testo**:

   ![Icona del criterio del componente testo](./assets/map-components/edit-text-policy.png)

   Crea un nuovo criterio denominato **Testo SPA WKND**. Alla voce **Plugin** > **Formattazione** > controlla tutte le caselle per abilitare ulteriori opzioni di formattazione:

   ![Abilita formattazione RTE](assets/map-components/enable-formatting-rte.png)

   Alla voce **Plugin** > **Stili di paragrafo** > seleziona la casella per **Abilitare gli stili di paragrafo**:

   ![Abilita stili di paragrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Fai clic su **Fine** per salvare l&#39;aggiornamento dei criteri.

6. Passa alla **Home page** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

   È inoltre necessario poter modificare il componente `Text` e aggiungere stili di paragrafo aggiuntivi in modalità **a schermo intero**.

   ![Modifica RTF a schermo intero](assets/map-components/full-screen-rte.png)

7. È inoltre necessario essere in grado di trascinare e rilasciare un&#39;immagine da **Asset Finder**:

   ![Trascina immagine](./assets/map-components/drag-drop-image.gif)

8. Aggiungi le tue immagini tramite [AEM Assets](http://localhost:4502/assets.html/content/dam) o installa la base di codice finita per il sito di riferimento standard [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Il [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) include molte immagini che possono essere riutilizzate nell&#39;applicazione a pagina singola WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti di AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Ispezionare il contenitore di layout

Il supporto per il **Contenitore di layout** viene fornito automaticamente dall’SDK dell’Editor SPA di AEM. Il **Contenitore di layout**, come indicato dal nome, è un componente **contenitore**. I componenti contenitore sono componenti che accettano strutture JSON che rappresentano *altri componenti* e le creano in modo dinamico un’istanza.

Esaminiamo ulteriormente il Contenitore di layout.

1. Nel browser passa a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API del modello JSON - Griglia reattiva](./assets/map-components/responsive-grid-modeljson.png)

   Il componente **Contenitore di layout** ha un `sling:resourceType` di `wcm/foundation/components/responsivegrid` ed è riconosciuto dall’Editor SPA utilizzando la proprietà `:type` , proprio come i componenti `Text` e `Image` .

   Le stesse funzionalità di ridimensionamento di un componente utilizzando [Modalità layout](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sono disponibili con l’Editor SPA.

2. Torna a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Aggiungi altri componenti **Immagine** e prova a ridimensionarli utilizzando l&#39;opzione **Layout** :

   ![Ridimensionare l’immagine utilizzando la modalità Layout](./assets/map-components/responsive-grid-layout-change.gif)

3. Riapri il modello JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e osserva il `columnClassNames` come parte del JSON:

   ![Nomi delle classi cloud](./assets/map-components/responsive-grid-classnames.png)

   Il nome della classe `aem-GridColumn--default--4` indica che il componente deve essere largo 4 colonne in base a una griglia a 12 colonne. Ulteriori dettagli sulla [griglia reattiva sono disponibili qui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Torna all’IDE e nel modulo `ui.apps` è presente una libreria lato client definita in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Aprire il file `less/grid.less`.

   Questo file determina i punti di interruzione (`default`, `tablet` e `phone`) utilizzati dal **Contenitore di layout**. Questo file deve essere personalizzato in base alle specifiche del progetto. Attualmente i punti di interruzione sono impostati su `1200px` e `650px`.

5. Dovresti essere in grado di utilizzare le funzionalità reattive e i criteri di testo RTF aggiornati del componente `Text` per creare una visualizzazione come la seguente:

   ![Esempio di capitolo di authoring finale](./assets/map-components/final-page.png)

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato a mappare i componenti SPA sui componenti AEM e hai implementato un nuovo componente `Image`. Hai anche la possibilità di esplorare le funzionalità reattive del **Contenitore di layout**.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) o estrarre il codice localmente passando al ramo `React/map-components-solution`.

### Passaggi successivi {#next-steps}

[Navigazione e routing](navigation-routing.md) : scopri come è possibile supportare più visualizzazioni nell’applicazione a pagina singola mappando le pagine AEM con l’SDK per l’editor di applicazioni a pagina singola. La navigazione dinamica viene implementata utilizzando React Router e aggiunta a un componente Header esistente.

## Bonus - Configurazioni permanenti al controllo del codice sorgente {#bonus}

In molti casi, specialmente all’inizio di un progetto AEM, è utile mantenere le configurazioni, come modelli e relativi criteri dei contenuti, al controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano con lo stesso set di contenuti e configurazioni e possono garantire un’ulteriore coerenza tra gli ambienti. Quando un progetto raggiunge un certo livello di maturità, la gestione dei modelli può essere affidata a un gruppo speciale di utenti.

I passaggi successivi si svolgeranno utilizzando l&#39;IDE di codice di Visual Studio e [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) ma potrebbero essere eseguiti utilizzando qualsiasi strumento e qualsiasi IDE configurato per **pull** o **import** da un&#39;istanza locale di AEM.

1. Nell&#39;IDE di codice di Visual Studio, assicurati di aver installato **VSCode AEM Sync** tramite l&#39;estensione Marketplace:

   ![Sincronizzazione AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Espandi il modulo **ui.content** in Project explorer e passa a `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Fai clic con il pulsante destro** del mouse sulla  `templates` cartella e seleziona  **Importa da AEM Server**:

   ![Modello di importazione VSCode](./assets/map-components/import-aem-servervscode.png)

4. Ripeti i passaggi per importare il contenuto, ma seleziona la cartella **policy** che si trova in `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Ispeziona il file `filter.xml` che si trova in `ui.content/src/main/content/META-INF/vault/filter.xml`.

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
