---
title: Mappare i componenti dell’SPA ai componenti dell’AEM | Guida introduttiva dell’Editor SPA dell’AEM e React
description: Scopri come mappare i componenti React ai componenti Adobe Experience Manager (AEM) con l’SDK JS dell’editor AEM SPA. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA nell’Editor SPA dell’AEM, in modo simile all’authoring AEM tradizionale. Scoprirai anche come utilizzare i componenti core React dell’AEM preconfigurati.
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2257'
ht-degree: 1%

---

# Mappare i componenti dell’SPA ai componenti dell’AEM {#map-components}

Scopri come mappare i componenti React ai componenti Adobe Experience Manager (AEM) con l’SDK JS dell’editor AEM SPA. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA nell’Editor SPA dell’AEM, in modo simile all’authoring AEM tradizionale.

Questo capitolo approfondisce l’analisi dell’API del modello JSON AEM e di come il contenuto JSON esposto da un componente AEM possa essere inserito automaticamente in un componente React come prop.

## Obiettivo

1. Scopri come mappare i componenti dell’AEM ai componenti dell’SPA.
1. Inspect come un componente React utilizza le proprietà dinamiche passate dall’AEM.
1. Scopri come utilizzare con i nuovi servizi [React AEM Componenti core](https://github.com/adobe/aem-react-core-wcm-components-examples).

## Cosa verrà creato

In questo capitolo viene verificato come `Text` La componente SPA è mappata all&#39;AEM `Text`componente. Componenti core React come `Image` La componente SPA viene utilizzata nell’SPA e creata nell’AEM. Funzioni pronte all’uso di **Contenitore di layout** e **Editor modelli** Le policy vengono inoltre utilizzate per creare una vista dall’aspetto leggermente più variabile.

![Creazione finale di esempio di capitolo](./assets/map-components/final-page.png)

## Prerequisiti

Esaminare gli strumenti e le istruzioni necessari per l&#39;impostazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment). Questo capitolo è una continuazione del [Integrare l’SPA](integrate-spa.md) capitolo, tuttavia, seguire lungo tutto ciò che serve è un progetto AEM abilitato per SPA.

## Approccio di mappatura

Il concetto di base è quello di mappare un componente SPA a un componente AEM. I componenti AEM, esegui lato server, esportano contenuti come parte dell’API del modello JSON. Il contenuto JSON viene utilizzato dall’SPA, che esegue il lato client nel browser. Viene creata una mappatura 1:1 tra i componenti SPA e un componente AEM.

![Panoramica ad alto livello della mappatura di un componente AEM su un componente React](./assets/map-components/high-level-approach.png)

*Panoramica ad alto livello della mappatura di un componente AEM su un componente React*

## Inspect il componente Testo

Il [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) fornisce un `Text` componente mappato sull’AEM [Componente testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Questo è un esempio **contenuto** componente, in quanto esegue il rendering *contenuto* dall&#39;AEM.

Vediamo come funziona il componente.

### Inspect il modello JSON

1. Prima di passare al codice SPA, è importante comprendere il modello JSON fornito dall’AEM. Accedi a [Libreria dei componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) e visualizzare la pagina del componente Testo. La libreria dei componenti core fornisce esempi di tutti i componenti core dell’AEM.
1. Seleziona la **JSON** per uno degli esempi:

   ![Modello JSON di testo](./assets/map-components/text-json.png)

   Dovresti vedere tre proprietà: `text`, `richText`, e `:type`.

   `:type` è una proprietà riservata che elenca `sling:resourceType` (o percorso) della componente AEM. Il valore di `:type` è ciò che viene utilizzato per mappare la componente AEM alla componente SPA.

   `text` e `richText` sono proprietà aggiuntive esposte al componente SPA.

1. Visualizza l’output JSON in [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Dovresti essere in grado di trovare una voce simile a:

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

1. Nell&#39;IDE che preferisci, apri il Progetto AEM per l&#39;SPA. Espandi `ui.frontend` e aprire il file `Text.js` in `ui.frontend/src/components/Text/Text.js`.

1. La prima area che esamineremo è `class Text` a ~line 40:

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

   `Text` è un componente React standard. Il componente utilizza `this.props.richText` per determinare se il contenuto da riprodurre sarà testo formattato o testo normale. Il &quot;contenuto&quot; effettivo utilizzato proviene da `this.props.text`.

   Per evitare un potenziale attacco XSS, il testo RTF viene sfuggito tramite `DOMPurify` prima di utilizzare [dangerouslySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) per riprodurre il contenuto. Richiama `richText` e `text` proprietà del modello JSON precedenti nell’esercizio.

1. Avanti, apri `ui.frontend/src/components/import-components.js` dai un&#39;occhiata al `TextEditConfig` a ~line 86:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Il codice di cui sopra è responsabile di determinare quando eseguire il rendering del segnaposto nell’ambiente di authoring dell’AEM. Se il `isEmpty` restituisce il metodo **true** quindi viene eseguito il rendering del segnaposto.

1. Infine, dai un&#39;occhiata al `MapTo` chiama alla riga 94:

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo` viene fornito dall’SDK JS dell’Editor SPA dell’AEM (`@adobe/aem-react-editable-components`). Il percorso `wknd-spa-react/components/text` rappresenta il `sling:resourceType` della componente AEM. Questo percorso viene abbinato al `:type` esposti dal modello JSON osservato in precedenza. `MapTo` si occupa di analizzare la risposta del modello JSON e di trasmettere i valori corretti come `props` alla componente SPA.

   Potete trovare l&#39;AEM `Text` definizione del componente in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Utilizzare i componenti core React

[Componenti WCM AEM - Implementazione React Core](https://github.com/adobe/aem-react-core-wcm-components-base) e [Componenti WCM AEM - Editor SPA - Implementazione React Core](https://github.com/adobe/aem-react-core-wcm-components-spa). Si tratta di un set di componenti riutilizzabili dell’interfaccia utente mappati su componenti AEM predefiniti. La maggior parte dei progetti può riutilizzare questi componenti come punto di partenza per la propria implementazione.

1. Nel codice del progetto apri il file `import-components.js` a `ui.frontend/src/components`.
Questo file importa tutti i componenti SPA mappati ai componenti AEM. Data la natura dinamica dell’implementazione dell’Editor SPA, è necessario fare riferimento esplicito a qualsiasi componente SPA associato ai componenti compatibili con l’AEM. Questo consente all’autore di AEM di scegliere di utilizzare un componente in qualsiasi posizione all’interno dell’applicazione.
1. Le seguenti istruzioni di importazione includono i componenti SPA scritti nel progetto:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Ce ne sono molti altri `imports` da `@adobe/aem-core-components-react-spa` e `@adobe/aem-core-components-react-base`. Questi componenti importano i componenti core React e li rendono disponibili nel progetto corrente. Questi vengono quindi mappati su componenti AEM specifici del progetto utilizzando `MapTo`, proprio come con il `Text` componente precedente.

### Aggiornamento delle politiche AEM

Le policy sono una funzione dei modelli AEM che offre agli sviluppatori e agli utenti avanzati un controllo granulare sui componenti disponibili per l’utilizzo. I componenti core React sono inclusi nel codice SPA, ma devono essere abilitati tramite una policy prima di poter essere utilizzati nell’applicazione.

1. Dalla schermata iniziale dell’AEM, vai a **Strumenti** > **Modelli** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Seleziona e apri la **Pagina SPA** modello per la modifica.

1. Seleziona la **Contenitore di layout** e fai clic su **policy** per modificare il criterio:

   ![criterio contenitore layout](assets/map-components/edit-spa-page-template.png)

1. Sotto **Componenti consentiti** > **WKND SPA React - Contenuto** > spunta **Immagine**, **Teaser**, e **Titolo**.

   ![Componenti aggiornati disponibili](assets/map-components/update-components-available.png)

   Sotto **Componenti predefiniti** > **Aggiungi mappatura** e scegli la **Immagine - React SPA WKND - Contenuto** componente:

   ![Imposta componenti predefiniti](./assets/map-components/default-components.png)

   Immetti un **tipo mime** di `image/*`.

   Clic **Fine** per salvare gli aggiornamenti dei criteri.

1. In **Contenitore di layout** fai clic su **policy** icona per **Testo** componente.

   Crea un nuovo criterio denominato **Testo WKND SPA**. Sotto **Plug-in** > **Formattazione** > seleziona tutte le caselle per abilitare opzioni di formattazione aggiuntive:

   ![Abilita formattazione editor Rich Text](assets/map-components/enable-formatting-rte.png)

   Sotto **Plug-in** > **Stili paragrafo** > seleziona la casella per **Abilita stili di paragrafo**:

   ![Abilita stili di paragrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Clic **Fine** per salvare l&#39;aggiornamento del criterio.

### Contenuto autore

1. Accedi a **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Ora dovresti poter utilizzare i componenti aggiuntivi **Immagine**, **Teaser**, e **Titolo** sulla pagina.

   ![Componenti aggiuntivi](assets/map-components/additional-components.png)

1. Dovresti anche poter modificare il `Text` e aggiungi altri stili di paragrafo in **a schermo intero** modalità.

   ![Modifica Rich Text A Schermo Intero](assets/map-components/full-screen-rte.png)

1. Dovresti anche poter trascinare e rilasciare un’immagine dalla **Asset Finder**:

   ![Trascina e rilascia l&#39;immagine](assets/map-components/drag-drop-image.png)

1. Esperienza con **Titolo** e **Teaser** componenti.

1. Aggiungi le tue immagini tramite [AEM Assets](http://localhost:4502/assets.html/content/dam) o installare la base di codice completata per lo standard [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Il [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) include molte immagini che possono essere riutilizzate sull’SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Gestione pacchetti installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect il contenitore di layout

Supporto per **Contenitore di layout** viene fornito automaticamente dall’SDK dell’editor SPA dell’AEM. Il **Contenitore di layout**, come indicato dal nome, è un **contenitore** componente. I componenti contenitore sono componenti che accettano strutture JSON che rappresentano *altro* e crearne un&#39;istanza dinamica.

Esaminiamo ulteriormente il Contenitore di layout.

1. In un browser passa a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API modello JSON - Griglia reattiva](./assets/map-components/responsive-grid-modeljson.png)

   Il **Contenitore di layout** il componente ha `sling:resourceType` di `wcm/foundation/components/responsivegrid` ed è riconosciuto dall&#39;editor SPA utilizzando `:type` proprietà, proprio come `Text` e `Image` componenti.

   Le stesse funzionalità di ridimensionamento di un componente mediante [Modalità Layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sono disponibili con l’editor SPA.

2. Torna a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Aggiungi ulteriori **Immagine** e provare a ridimensionarli utilizzando **Layout** opzione:

   ![Ridimensionare l’immagine utilizzando la modalità Layout](./assets/map-components/responsive-grid-layout-change.gif)

3. Riapri il modello JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e osservano `columnClassNames` come parte del JSON:

   ![Nomi classi colonna](./assets/map-components/responsive-grid-classnames.png)

   Il nome della classe `aem-GridColumn--default--4` indica che il componente deve avere una larghezza di 4 colonne in base a una griglia a 12 colonne. Maggiori dettagli su [la griglia reattiva si trova qui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Torna all’IDE e nella `ui.apps` è presente una libreria lato client definita in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Apri il file `less/grid.less`.

   Questo file determina i punti di interruzione (`default`, `tablet`, e `phone`) utilizzato da **Contenitore di layout**. Questo file è stato progettato per essere personalizzato in base alle specifiche del progetto. Attualmente i punti di interruzione sono impostati su `1200px` e `768px`.

5. Dovresti essere in grado di utilizzare le funzionalità reattive e i criteri Rich Text aggiornati di `Text` per creare una visualizzazione simile alla seguente:

   ![Creazione finale di esempio di capitolo](assets/map-components/final-page.png)

## Congratulazioni.  {#congratulations}

Congratulazioni, hai imparato a mappare i componenti SPA ai componenti AEM e hai utilizzato i componenti core React. Hai anche la possibilità di esplorare le funzionalità reattive del **Contenitore di layout**.

### Passaggi successivi {#next-steps}

[Navigazione e indirizzamento](navigation-routing.md) - Scopri come è possibile supportare più visualizzazioni nell’SPA mediante la mappatura su pagine AEM con l’SDK dell’Editor SPA. La navigazione dinamica viene implementata utilizzando React Router e React Core Components.

## (Bonus) Mantenere le configurazioni per il controllo del codice sorgente {#bonus-configs}

In molti casi, soprattutto all’inizio di un progetto AEM, è utile mantenere le configurazioni, come i modelli e le relative policy di contenuto, per il controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano sullo stesso set di contenuti e configurazioni e possono garantire ulteriore coerenza tra gli ambienti. Quando un progetto raggiunge un certo livello di maturità, la pratica di gestione dei modelli può essere affidata a uno speciale gruppo di utenti esperti.

I passaggi successivi verranno eseguiti utilizzando l&#39;IDE di Visual Studio Code e [Sincronizzazione AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) ma potrebbe utilizzare qualsiasi strumento e IDE configurato per **tirare** o **importa** contenuti provenienti da un’istanza locale dell’AEM.

1. Nell&#39;IDE di Visual Studio Code verificare di avere **Sincronizzazione AEM VSCode** installato tramite l’estensione Marketplace:

   ![Sincronizzazione AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Espandi **ui.content** in Esplora progetti e passare a `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Clic con il pulsante destro** il `templates` cartella e seleziona **Importa da server AEM**:

   ![Modello di importazione VSCode](./assets/map-components/import-aem-servervscode.png)

4. Ripeti i passaggi per importare il contenuto, ma seleziona la **criteri** cartella situata in `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect `filter.xml` file che si trova in `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Il `filter.xml` Il file è responsabile dell’identificazione dei percorsi dei nodi installati con il pacchetto. Osserva `mode="merge"` in ciascuno dei filtri che indica che il contenuto esistente non verrà modificato, viene aggiunto solo il nuovo contenuto. Poiché gli autori dei contenuti potrebbero aggiornare questi percorsi, è importante che una distribuzione del codice **non** sovrascrivi il contenuto. Consulta la [Documentazione di FileVault](https://jackrabbit.apache.org/filevault/filter.html) per ulteriori dettagli sull’utilizzo degli elementi filtro.

   Confronta `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` per comprendere i diversi nodi gestiti da ciascun modulo.

## (Bonus) Creare un componente immagine personalizzato {#bonus-image}

I componenti core React hanno già fornito un componente Immagine SPA. Tuttavia, se desideri ulteriore pratica, crea la tua implementazione React che mappi all’AEM [Componente immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=it). Il `Image` è un altro esempio di **contenuto** componente.

### Inspect il JSON

Prima di passare al codice SPA, controlla il modello JSON fornito dall’AEM.

1. Accedi a [Esempi di immagini nella libreria dei componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![JSON del componente core immagine](./assets/map-components/image-json.png)

   Proprietà di `src`, `alt`, e `title` sono utilizzati per popolare l’SPA `Image` componente.

   >[!NOTE]
   >
   > Sono esposte altre proprietà immagine (`lazyEnabled`, `widths`) che consentono a uno sviluppatore di creare un componente adattivo e a caricamento lento. Il componente integrato in questa esercitazione è semplice e **non** utilizza queste proprietà avanzate.

### Implementare il componente Immagine

1. Quindi, crea una nuova cartella denominata `Image` in `ui.frontend/src/components`.
1. Sotto `Image` cartella crea un nuovo file denominato `Image.js`.

   ![File Image.js](./assets/map-components/image-js-file.png)

1. Aggiungi quanto segue `import` istruzioni per `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. Quindi aggiungi `ImageEditConfig` per determinare quando visualizzare il segnaposto in AEM:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   Il segnaposto mostra se `src` proprietà non impostata.

1. Avanti implementare `Image` classe:

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

   Il codice riportato sopra restituirà un `<img>` in base alle proprietà `src`, `alt`, e `title` passato dal modello JSON.

1. Aggiungi il `MapTo` codice per mappare il componente React al componente AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Prendi nota della stringa `wknd-spa-react/components/image` corrisponde alla posizione della componente AEM in `ui.apps` a: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Crea un nuovo file denominato `Image.css` nella stessa directory e aggiungi quanto segue:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. In entrata `Image.js` aggiungi un riferimento al file nella parte superiore sotto il `import` istruzioni:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Apri il file `ui.frontend/src/components/import-components.js` e aggiungi un riferimento al nuovo `Image` componente:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. In entrata `import-components.js` aggiungi un commento all’immagine del componente core React:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   In questo modo verrà utilizzato il componente immagine personalizzato.

1. Dalla directory principale del progetto, distribuisci il codice SPA all’AEM utilizzando Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspect l&#39;SPA nell&#39;AEM. Tutti i componenti Immagine sulla pagina continueranno a funzionare. Inspect ha eseguito il rendering dell’output e dovresti visualizzare il markup per il componente immagine personalizzato anziché il componente core React.

   *Markup del componente Immagine personalizzato*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *Markup immagine del componente core React*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   Questa è una buona introduzione per estendere e implementare i propri componenti.
