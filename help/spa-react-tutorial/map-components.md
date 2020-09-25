---
title: Mappatura di componenti SPA su componenti AEM | Guida introduttiva all'editor SPA AEM e React
description: Scoprite come mappare i componenti React ai componenti Adobe Experience Manager (AEM) con l’SDK JS AEM SPA Editor. La mappatura dei componenti consente agli utenti di effettuare aggiornamenti dinamici ai componenti SPA nell’editor AEM SPA, in modo simile all’authoring AEM tradizionale.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '2259'
ht-degree: 1%

---


# Mappatura di componenti SPA su componenti AEM {#map-components}

Scoprite come mappare i componenti React ai componenti Adobe Experience Manager (AEM) con l’SDK JS AEM SPA Editor. La mappatura dei componenti consente agli utenti di effettuare aggiornamenti dinamici ai componenti SPA nell’editor AEM SPA, in modo simile all’authoring AEM tradizionale.

Questo capitolo descrive in modo più approfondito l’API del modello JSON AEM e come il contenuto JSON esposto da un componente AEM può essere inserito automaticamente in un componente React come prop.

## Obiettivo

1. Scoprite come mappare AEM componenti su componenti SPA.
2. Comprendere la differenza tra i componenti **Contenitore** e i componenti **Contenuto** .
3. Crea un nuovo componente React che viene mappato su un componente AEM esistente.

## Cosa verrà creato

Questo capitolo analizza come il componente `Text` SPA fornito viene mappato sul `Text`componente AEM. Verrà creato un nuovo componente `Image` SPA che potrà essere utilizzato nell&#39;SPA e creato in AEM. Le funzioni predefinite dei criteri Contenitore **di** layout e Editor **di** modelli verranno utilizzate anche per creare una visualizzazione di aspetto leggermente più varia.

![Authoring finale di esempio di capitolo](./assets/map-components/final-page.png)

## Prerequisiti

Esaminate le istruzioni e gli strumenti necessari per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale.

### Ottenere il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. Distribuire la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzate [AEM 6.x](overview.md#compatibility) , aggiungete il `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) o estrarre il codice localmente passando al ramo `React/map-components-solution`.

## Approccio di mappatura

Il concetto di base consiste nel mappare un componente SPA su un componente AEM. AEM componenti, eseguite lato server, esportate il contenuto come parte dell&#39;API del modello JSON. Il contenuto JSON viene utilizzato dall&#39;SPA, eseguendo lato client nel browser. Viene creata una mappatura 1:1 tra i componenti SPA e un componente AEM.

![Panoramica di alto livello sulla mappatura di un componente AEM a un componente React](./assets/map-components/high-level-approach.png)

*Panoramica di alto livello sulla mappatura di un componente AEM a un componente React*

##  Inspect il componente Testo

Il [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) fornisce un `Text` componente mappato al componente [AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html)Text. Questo è un esempio di un componente **contenuto** , in quanto esegue il rendering del *contenuto* da AEM.

Vediamo come funziona il componente.

###  Inspect il modello JSON

1. Prima di passare al codice SPA, è importante comprendere il modello JSON AEM fornito. Passare alla libreria [dei componenti](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) core e visualizzare la pagina del componente Testo. La libreria dei componenti core fornisce esempi di tutti i componenti core AEM.
2. Selezionate la scheda **JSON** per uno degli esempi:

   ![Text JSON, modello](./assets/map-components/text-json.png)

   Vengono visualizzate tre proprietà: `text`, `richText`e `:type`.

   `:type` è una proprietà riservata che elenca il `sling:resourceType` (o percorso) del componente AEM. Il valore di `:type` corrisponde a quello utilizzato per mappare il componente AEM sul componente SPA.

   `text` e `richText` sono proprietà aggiuntive che verranno esposte al componente SPA.

###  Inspect il componente Testo

1. Aprite un nuovo terminale e individuate la `ui.frontend` cartella all’interno del progetto. Eseguire `npm install` e quindi `npm start` avviare il **webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   Il `ui.frontend` modulo è attualmente configurato per utilizzare il modello [JSON](./integrate-spa.md#mock-json)fittizio.

2. Viene visualizzata una nuova finestra del browser che si apre su [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Server di sviluppo webpack con contenuto fittizio](./assets/map-components/initial-start.png)

3. Nell’IDE di vostra scelta, aprite il progetto AEM per l’area SPA WKND. Espandete il `ui.frontend` modulo e aprite il file `Text.js` in `ui.frontend/src/components/Text/Text.js`:

   ![Codice sorgente componente React Text.js](./assets/map-components/vscode-ide-text-js.png)

4. La prima area che ispezioneremo è la `class Text` a ~line 40:

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

   `Text` è un componente React standard. Il componente utilizza `this.props.richText` per determinare se il contenuto di cui eseguire il rendering sarà RTF o testo normale. Il &quot;contenuto&quot; effettivo utilizzato viene da `this.props.text`. Per evitare un potenziale attacco XSS, il testo RTF viene salvato tramite escape `DOMPurify` prima di utilizzare [pericolosamenteSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) per eseguire il rendering del contenuto. Ricordare le proprietà `richText` e `text` dal modello JSON prima dell&#39;esercizio.

5. Date un&#39;occhiata alla `TextEditConfig` ~line 29:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Il codice riportato sopra ha il compito di determinare quando eseguire il rendering del segnaposto nell’ambiente di authoring AEM. Se il `isEmpty` metodo restituisce **true** , viene eseguito il rendering del segnaposto.

6. Infine, guarda la `MapTo` chiamata a ~line 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` è fornito dall’SDK JS AEM SPA Editor (`@adobe/aem-react-editable-components`). Il percorso `wknd-spa-react/components/text` rappresenta il `sling:resourceType` componente AEM. Questo percorso viene confrontato con quello `:type` esposto dal modello JSON osservato in precedenza. `MapTo` si occupa di analizzare la risposta del modello JSON e passare i valori corretti `props` al componente SPA.

   È possibile trovare la definizione del `Text` componente AEM in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

7. Provate modificando il `mock.model.json` file in `ui.frontend/public/mock-content/mock.model.json`. In ~riga 62 aggiornare il primo `Text` valore per utilizzare un **`H1`** e **`u`** i tag:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   Andate a [http://localhost:3000](Http://localhost:3000) per vedere gli effetti:

   ![Modello di testo aggiornato](./assets/map-components/updated-text-model.png)

   Provate a alternare la `richText` proprietà tra **true** e **false** per visualizzare la logica di rendering in azione.

8.  Inspect `Text.scss` a `ui.frontend/src/components/Text/Text.scss`.

   Questo file è stato aggiunto dalla base di codice iniziale per questo capitolo e utilizza la funzione [Sass](https://sass-lang.com/) aggiunta nel capitolo precedente. Prendete nota delle variabili cui si fa riferimento da `ui.frontend/src/styles/_variables.scss`.

## Creare il componente Immagine

Quindi, create un componente `Image` React che viene mappato sul componente [AEM](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/components/image.html)Immagine. Il `Image` componente è un altro esempio di un componente **contenuto** .

###  Inspect JSON

Prima di passare al codice SPA, ispezionare il modello JSON fornito da AEM.

1. Andate agli esempi di [immagini nella libreria](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html)Componenti di base.

   ![JSON componente di base immagine](./assets/map-components/image-json.png)

   Le proprietà di `src`, `alt`e `title` verranno utilizzate per compilare il componente SPA `Image` .

   >[!NOTE]
   >
   > Esistono altre proprietà Immagine esposte (`lazyEnabled`, `widths`) che consentono a uno sviluppatore di creare un componente adattivo e a caricamento lento. Il componente creato in questa esercitazione sarà semplice e **non** utilizzerà queste proprietà avanzate.

2. Return to your IDE and open up the `mock.model.json` at `ui.frontend/public/mock-content/mock.model.json`. Poiché questo è un nuovo componente per il nostro progetto, dobbiamo &quot;prendere in giro&quot; il JSON Immagine.

   In ~riga 70 aggiungete una voce JSON per il `image` modello (non dimenticate la virgola finale `,` dopo la seconda `text_23828680`) e aggiornate l&#39; `:itemsOrder` array.

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

   Puoi visualizzare il file [mock.model.json completo qui](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json).

### Implementare il componente Immagine

1. Quindi, create una nuova cartella denominata `Image` in `ui.frontend/src/components`.
2. Beneath the `Image` folder create a new file named `Image.js`.

   ![Image.js, file](./assets/map-components/image-js-file.png)

3. Aggiungete le seguenti `import` istruzioni a `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. Quindi aggiungete `ImageEditConfig` per determinare quando visualizzare il segnaposto in AEM:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   Il segnaposto viene visualizzato se la `src` proprietà non è impostata.

5. Implementare quindi la `Image` classe:

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

   Il codice riportato sopra eseguirà il rendering di un `<img>` oggetto basato sulle proprietà `src`, `alt`e `title` trasmesso dal modello JSON.

6. Aggiungete il `MapTo` codice per mappare il componente React al componente AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   La stringa `wknd-spa-react/components/image` corrisponde alla posizione del componente AEM in `ui.apps` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. Create un nuovo file denominato `Image.scss` nella stessa directory e aggiungete quanto segue:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. In `Image.js` aggiungere un riferimento al file nella parte superiore sotto le `import` istruzioni:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   È possibile visualizzare [Image.js completato qui](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js).

9. Apri il file `ui.frontend/src/components/import-components.js` e aggiungi un riferimento al nuovo `Image` componente:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. Se non è già stato avviato, avviare il **webpack-dev-server**. Andate a [http://localhost:3000](Http://localhost:3000) e vedrete il rendering dell&#39;immagine:

   ![Immagine aggiunta al cursore](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **Sfida** bonus: Implementate un nuovo metodo `Image.js` per visualizzare il valore di `this.props.title` come didascalia sotto l’immagine.

## Aggiorna criteri in AEM

Il `Image` componente è visibile solo nel **webpack-dev-server**. Quindi, distribuite l&#39;app SPA aggiornata per AEM e aggiornare i criteri dei modelli.

1. Arrestate il **webpack-dev-server** e dal livello principale del progetto, implementate le modifiche per AEM utilizzando le vostre abilità Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Dalla schermata iniziale AEM passare a **Strumenti** > **Modelli** > Reazione SPA **[WKND](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

   Selezionate e modificate la pagina **** SPA:

   ![Modifica modello pagina SPA](./assets/map-components/edit-spa-page-template.png)

3. Selezionate il Contenitore **di** layout e fate clic sull&#39;icona del relativo **criterio** per modificare il criterio:

   ![Criteri contenitore layout](./assets/map-components/layout-container-policy.png)

4. In Componenti **** consentiti > Reazione SPA **WKND - Contenuto** > controllate il componente **Immagine** :

   ![Componente immagine selezionato](./assets/map-components/check-image-component.png)

   In Componenti **** predefiniti > **Aggiungi mappatura** e scegli **Immagine - Reazione SPA WKND - Componente contenuto** :

   ![Impostazione di componenti predefiniti](./assets/map-components/default-components.png)

   Immettete un tipo **** mime di `image/*`.

   Fate clic su **Fine** per salvare gli aggiornamenti dei criteri.

5. Nel Contenitore **di** layout fate clic sull&#39;icona del **criterio** relativa al componente **Testo** :

   ![Icona del criterio del componente testo](./assets/map-components/edit-text-policy.png)

   Create un nuovo criterio denominato Testo **SPA** WKND. In **Plugins** > **Formattazione** > selezionare tutte le caselle per abilitare ulteriori opzioni di formattazione:

   ![Attiva formattazione RTE](assets/map-components/enable-formatting-rte.png)

   In **Plug-in** > Stili **di** paragrafo > selezionate la casella di controllo **Abilita stili** di paragrafo:

   ![Abilita stili di paragrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Fate clic su **Fine** per salvare l&#39;aggiornamento del criterio.

6. Passate alla **homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

   È inoltre necessario poter modificare il `Text` componente e aggiungere altri stili di paragrafo in modalità **a schermo** intero.

   ![Modifica RTF a schermo intero](assets/map-components/full-screen-rte.png)

7. È inoltre necessario essere in grado di trascinare un’immagine da **Asset Finder**:

   ![Trascinamento dell’immagine](./assets/map-components/drag-drop-image.gif)

8. Aggiungete le vostre immagini tramite [AEM Assets](http://localhost:4502/assets.html/content/dam) o installate la base di codice finita per il sito [di riferimento](https://github.com/adobe/aem-guides-wknd/releases/latest)WKND standard. Il sito [di riferimento](https://github.com/adobe/aem-guides-wknd/releases/latest) WKND include molte immagini che possono essere riutilizzate nell&#39;SPA WKND. Il pacchetto può essere installato tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

##  Inspect il Contenitore di layout

Il supporto per il Contenitore **di** layout viene fornito automaticamente dall’SDK dell’editor SPA AEM. Il Contenitore **di** layout, come indicato dal nome, è un componente **contenitore** . I componenti contenitore sono componenti che accettano strutture JSON che rappresentano *altri* componenti e le istanziano in modo dinamico.

Esaminiamo ulteriormente il Contenitore di layout.

1. In un browser, andate a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API modello JSON - Griglia reattiva](./assets/map-components/responsive-grid-modeljson.png)

   Il componente Contenitore **** di layout è dotato `sling:resourceType` di `wcm/foundation/components/responsivegrid` e viene riconosciuto dall’editor SPA utilizzando la `:type` proprietà, proprio come i `Text` componenti e `Image` .

   Con l’editor SPA sono disponibili le stesse funzionalità di ridimensionamento di un componente in modalità [](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) Layout.

2. Tornate a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Aggiungete altri componenti **Immagine** e provate a ridimensionarli utilizzando l’opzione **Layout** :

   ![Ridimensionare l&#39;immagine utilizzando la modalità Layout](./assets/map-components/responsive-grid-layout-change.gif)

3. Riaprite il modello JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e osservate `columnClassNames` come parte del JSON:

   ![Nomi classe cloud](./assets/map-components/responsive-grid-classnames.png)

   Il nome della classe `aem-GridColumn--default--4` indica che il componente deve avere una larghezza di 4 colonne in base a una griglia di 12 colonne. More details about the [responsive grid can be found here](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Tornare all&#39;IDE e nel `ui.apps` modulo è presente una libreria lato client definita in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Aprire il file `less/grid.less`.

   Questo file determina i punti di interruzione (`default`, `tablet`e `phone`) utilizzati dal Contenitore **di** layout. Questo file è destinato a essere personalizzato in base alle specifiche del progetto. Attualmente i punti di interruzione sono impostati su `1200px` e `650px`.

5. Per creare una visualizzazione come segue, è necessario poter utilizzare le funzionalità reattive e i criteri di RTF aggiornati del `Text` componente:

   ![Authoring finale di esempio di capitolo](./assets/map-components/final-page.png)

## Congratulazioni! {#congratulations}

Complimenti, hai imparato a mappare i componenti SPA su AEM componenti e hai implementato un nuovo `Image` componente. È inoltre possibile esplorare le funzionalità reattive del Contenitore **di** layout.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) o estrarre il codice localmente passando al ramo `React/map-components-solution`.

### Passaggi successivi {#next-steps}

[Navigazione e routing](navigation-routing.md) - Scoprite come è possibile supportare più viste nell’area SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando React Router e aggiunta a un componente Intestazione esistente.

## Bonus - Configurazioni persistenti per il controllo del codice sorgente {#bonus}

In molti casi, specialmente all&#39;inizio di un progetto AEM, è utile mantenere configurazioni, come modelli e relativi criteri di contenuto, al controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano con lo stesso set di contenuti e configurazioni e possono garantire un&#39;ulteriore coerenza tra gli ambienti. Una volta raggiunto un certo livello di maturità, la gestione dei modelli può essere affidata a un gruppo speciale di utenti.

I passaggi successivi verranno eseguiti utilizzando Visual Studio Code IDE e [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) , ma potrebbero essere eseguiti utilizzando qualsiasi strumento e qualsiasi IDE configurato per **estrarre** o **importare** contenuto da un&#39;istanza locale di AEM.

1. Nell&#39;IDE di codice di Visual Studio, assicurarsi che sia installato **VSCode AEM Sync** tramite l&#39;estensione Marketplace:

   ![AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Espandete il modulo **ui.content** in Project Explorer e passate a `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Fate clic con il pulsante destro del mouse** sulla `templates` cartella e selezionate **Importa da AEM server**:

   ![Modello di importazione VSCode](./assets/map-components/import-aem-servervscode.png)

4. Ripetere i passaggi per importare il contenuto, ma selezionare la cartella **policy** in cui si trova `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5.  Inspect il `filter.xml` file che si trova in `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Il `filter.xml` file è responsabile dell&#39;identificazione dei percorsi dei nodi che verranno installati con il pacchetto. Notate `mode="merge"` che su ciascuno dei filtri viene indicato che il contenuto esistente non verrà modificato, ma viene aggiunto solo nuovo contenuto. Poiché gli autori dei contenuti potrebbero aggiornare questi percorsi, è importante che la distribuzione del codice **non** sovrascriva il contenuto. Per ulteriori informazioni sull&#39;utilizzo degli elementi del filtro, consulta la documentazione [](https://jackrabbit.apache.org/filevault/filter.html) FileVault.

   Confrontare `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` comprendere i diversi nodi gestiti da ciascun modulo.
