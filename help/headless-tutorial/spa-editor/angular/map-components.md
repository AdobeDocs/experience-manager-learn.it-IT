---
title: Mappare i componenti dell’SPA ai componenti dell’AEM | Guida introduttiva dell'Angular e dell'editor SPA dell'AEM
description: Scopri come mappare i componenti Angular ai componenti Adobe Experience Manager (AEM AEM) con l’SDK JS dell’editor dell’SPA. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA nell’Editor SPA dell’AEM, in modo simile all’authoring AEM tradizionale.
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2372'
ht-degree: 1%

---

# Mappare i componenti dell’SPA ai componenti dell’AEM {#map-components}

Scopri come mappare i componenti Angular ai componenti Adobe Experience Manager (AEM AEM) con l’SDK JS dell’editor dell’SPA. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA nell’Editor SPA dell’AEM, in modo simile all’authoring AEM tradizionale.

Questo capitolo approfondisce l’analisi dell’API del modello JSON AEM e di come il contenuto JSON esposto da un componente AEM possa essere inserito automaticamente in un componente Angular come prop.

## Obiettivo

1. Scopri come mappare i componenti dell’AEM ai componenti dell’SPA.
2. Comprendere la differenza tra **Contenitore** componenti e **Contenuto** componenti.
3. Crea un nuovo componente Angular mappato su un componente AEM esistente.

## Cosa verrà creato

In questo capitolo verrà esaminato come `Text` La componente SPA è mappata all&#39;AEM `Text`componente. Una nuova `Image` Viene creata la componente SPA che può essere utilizzata nell’SPA e creata nell’AEM. Funzioni pronte all’uso di **Contenitore di layout** e **Editor modelli** Le policy verranno inoltre utilizzate per creare una visualizzazione con un aspetto leggermente più variabile.

![Creazione finale di esempio di capitolo](./assets/map-components/final-page.png)

## Prerequisiti

Esaminare gli strumenti e le istruzioni necessari per l&#39;impostazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Distribuisci la base di codice in un’istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se si utilizza [AEM 6.x](overview.md#compatibility) aggiungi `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) oppure controllare il codice localmente passando alla filiale `Angular/map-components-solution`.

## Approccio di mappatura

Il concetto di base è quello di mappare un componente SPA a un componente AEM. I componenti AEM, esegui lato server, esportano contenuti come parte dell’API del modello JSON. Il contenuto JSON viene utilizzato dall’SPA, che esegue il lato client nel browser. Viene creata una mappatura 1:1 tra i componenti SPA e un componente AEM.

![Panoramica di alto livello sulla mappatura di un componente AEM in un componente Angular](./assets/map-components/high-level-approach.png)

*Panoramica di alto livello sulla mappatura di un componente AEM in un componente Angular*

## Inspect il componente Testo

Il [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) fornisce un `Text` componente mappato sull’AEM [Componente testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Questo è un esempio **contenuto** componente, in quanto esegue il rendering *contenuto* dall&#39;AEM.

Vediamo come funziona il componente.

### Inspect il modello JSON

1. Prima di passare al codice SPA, è importante comprendere il modello JSON fornito dall’AEM. Accedi a [Libreria dei componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) e visualizzare la pagina del componente Testo. La libreria dei componenti core fornisce esempi di tutti i componenti core dell’AEM.
2. Seleziona la **JSON** per uno degli esempi:

   ![Modello JSON di testo](./assets/map-components/text-json.png)

   Dovresti vedere tre proprietà: `text`, `richText`, e `:type`.

   `:type` è una proprietà riservata che elenca `sling:resourceType` (o percorso) della componente AEM. Il valore di `:type` è ciò che viene utilizzato per mappare la componente AEM alla componente SPA.

   `text` e `richText` sono proprietà aggiuntive esposte al componente SPA.

### Inspect il componente Testo

1. Apri un nuovo terminale e passa a `ui.frontend` all&#39;interno del progetto. Esegui `npm install` e poi `npm start` per avviare **server di sviluppo webpack**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   Il `ui.frontend` è attualmente configurato per utilizzare il [modello JSON fittizio](./integrate-spa.md#mock-json).

2. Dovresti visualizzare una nuova finestra del browser aperta a [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![Server di sviluppo Webpack con contenuti fittizi](assets/map-components/initial-start.png)

3. Nell’IDE che preferisci, apri il progetto AEM per l’SPA WKND. Espandi `ui.frontend` e aprire il file **text.component.ts** in `ui.frontend/src/app/components/text/text.component.ts`:

   ![Codice sorgente del componente Angular Text.js](assets/map-components/vscode-ide-text-js.png)

4. La prima area da esaminare è `class TextComponent` a ~riga 35:

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input](https://angular.io/api/core/Input) decorator viene utilizzato per dichiarare i campi i cui valori sono impostati tramite l’oggetto JSON mappato, rivisto in precedenza.

   `@HostBinding('innerHtml') get content()` è un metodo che espone il contenuto del testo creato dal valore di `this.text`. Nel caso in cui il contenuto sia un testo RTF (determinato dal `this.richText` flag) La sicurezza integrata dell’Angular viene ignorata. Angular [Strumento di pulizia](https://angular.io/api/platform-browser/DomSanitizer) viene utilizzato per &quot;scorrere&quot; il HTML non elaborato e impedire vulnerabilità cross-site scripting. Il metodo è associato al `innerHtml` proprietà utilizzando [@HostBinding](https://angular.io/api/core/HostBinding) decoratore.

5. Ispezionare quindi `TextEditConfig` a ~riga 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   Il codice di cui sopra è responsabile di determinare quando eseguire il rendering del segnaposto nell’ambiente di authoring dell’AEM. Se il `isEmpty` restituisce il metodo **true** quindi viene eseguito il rendering del segnaposto.

6. Infine, dai un&#39;occhiata al `MapTo` chiama alla riga 53:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** viene fornito dall’SDK JS dell’Editor SPA dell’AEM (`@adobe/cq-angular-editable-components`). Il percorso `wknd-spa-angular/components/text` rappresenta il `sling:resourceType` della componente AEM. Questo percorso viene abbinato al `:type` esposti dal modello JSON osservato in precedenza. **MapTo** analizza la risposta del modello JSON e trasmette i valori corretti al `@Input()` variabili della componente SPA.

   Potete trovare l&#39;AEM `Text` definizione del componente in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Sperimenta modificando il **en.model.json** file in `ui.frontend/src/mocks/json/en.model.json`.

   Alla ~riga 62 aggiorna il primo `Text` valore da utilizzare **`H1`** e **`u`** tag:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Torna al browser per visualizzare gli effetti prodotti da **server di sviluppo webpack**:

   ![Modello di testo aggiornato](assets/map-components/updated-text-model.png)

   Prova a attivare/disattivare `richText` proprietà tra **true** / **false** per visualizzare la logica di rendering in azione.

8. Inspect **text.component.html** a `ui.frontend/src/app/components/text/text.component.html`.

   Questo file è vuoto perché l’intero contenuto del componente è impostato da `innerHTML` proprietà.

9. Inspect **app.module.ts** a `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   Il **TextComponent** non è incluso esplicitamente, ma piuttosto in modo dinamico tramite **AEMResponsiveGridComponent** fornite dall’SDK JS dell’editor SPA dell’AEM. Pertanto deve essere elencato nella sezione **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components) array.

## Creare il componente Immagine

Quindi, crea un&#39; `Image` Componente Angular mappato sull’AEM [Componente immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=it). Il `Image` è un altro esempio di **contenuto** componente.

### Inspect il JSON

Prima di passare al codice SPA, controlla il modello JSON fornito dall’AEM.

1. Accedi a [Esempi di immagini nella libreria dei componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![JSON del componente core immagine](./assets/map-components/image-json.png)

   Proprietà di `src`, `alt`, e `title` sono utilizzati per popolare l’SPA `Image` componente.

   >[!NOTE]
   >
   > Sono esposte altre proprietà immagine (`lazyEnabled`, `widths`) che consentono a uno sviluppatore di creare un componente adattivo e a caricamento lento. Il componente integrato in questa esercitazione è semplice e **non** utilizza queste proprietà avanzate.

2. Torna all’IDE e apri la `en.model.json` a `ui.frontend/src/mocks/json/en.model.json`. Poiché si tratta di un componente nuovo per il nostro progetto, dobbiamo &quot;simulare&quot; il JSON dell’immagine.

   Alla ~riga 70 aggiungi una voce JSON per il `image` modello (non dimenticare la virgola finale) `,` dopo il secondo `text_386303036`) e aggiorna il `:itemsOrder` array.

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   Il progetto include un&#39;immagine di esempio in `/mock-content/adobestock-140634652.jpeg` utilizzato con **server di sviluppo webpack**.

   È possibile visualizzare [en.model.json qui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. Aggiungete una foto d&#39;archivio che verrà visualizzata dal componente.

   Crea una nuova cartella denominata **immagini** sotto `ui.frontend/src/mocks`. Scarica [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) e inseriscilo nella nuova **immagini** cartella. Se lo desideri, puoi usare la tua immagine personale.

### Implementare il componente Immagine

1. Interrompi **server di sviluppo webpack** se avviato.
2. Creare un nuovo componente Immagine eseguendo l’interfaccia della riga di comando Angular `ng generate component` comando da `ui.frontend` cartella:

   ```shell
   $ ng generate component components/image
   ```

3. Nell’IDE, apri **image.component.ts** a `ui.frontend/src/app/components/image/image.component.ts` e aggiornare come segue:

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` è la configurazione per determinare se eseguire il rendering del segnaposto di authoring in AEM, in base al `src` La proprietà è compilata.

   `@Input()` di `src`, `alt`, e `title` sono le proprietà mappate dall’API JSON.

   `hasImage()` è un metodo che determinerà se l’immagine deve essere sottoposta a rendering.

   `MapTo` mappa il componente SPA sul componente AEM che si trova all’indirizzo `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. Apri **image.component.html** e aggiornarla come segue:

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Verrà eseguito il rendering del `<img>` elemento if `hasImage` restituisce **true**.

5. Apri **image.component.scss** e aggiornarla come segue:

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > Il `:host-context` la regola è **critico** affinché il segnaposto dell’editor SPA dell’AEM funzioni correttamente. Questa regola è necessaria almeno per tutti i componenti SPA destinati a essere creati nell’editor di pagine AEM.

6. Apri `app.module.ts` e aggiungi `ImageComponent` al `entryComponents` array:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Mi piace `TextComponent`, il `ImageComponent` viene caricato in modo dinamico e deve essere incluso nel `entryComponents` array.

7. Avvia il **server di sviluppo webpack** per visualizzare `ImageComponent` rendering.

   ```shell
   $ npm run start:mock
   ```

   ![Immagine aggiunta al modello](assets/map-components/image-added-mock.png)

   *Immagine aggiunta all&#39;SPA*

   >[!NOTE]
   >
   > **Sfida bonus**: implementa un nuovo metodo per visualizzare il valore di `title` come didascalia sotto l&#39;immagine.

## Aggiornamento delle politiche in AEM

Il `ImageComponent` è visibile solo nel **server di sviluppo webpack**. Quindi, distribuisci l’SPA aggiornato all’AEM e aggiorna i criteri dei modelli.

1. Interrompi **server di sviluppo webpack** e dal **radice** del progetto, implementa le modifiche all’AEM utilizzando le abilità Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Dalla schermata iniziale dell’AEM, vai a **[!UICONTROL Strumenti]** > **[!UICONTROL Modelli]** > **[WKND ANGULAR SPA](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Seleziona e modifica il **Pagina SPA**:

   ![Modifica modello pagina SPA](assets/map-components/edit-spa-page-template.png)

3. Seleziona la **Contenitore di layout** e fai clic su **policy** per modificare il criterio:

   ![Criterio contenitore layout](./assets/map-components/layout-container-policy.png)

4. Sotto **Componenti consentiti** > **Angular WKND SPA - Contenuto** > controllare la **Immagine** componente:

   ![Componente immagine selezionato](assets/map-components/check-image-component.png)

   Sotto **Componenti predefiniti** > **Aggiungi mappatura** e scegli la **Immagine - Angular WKND SPA - Contenuto** componente:

   ![Imposta componenti predefiniti](assets/map-components/default-components.png)

   Immetti un **tipo mime** di `image/*`.

   Clic **Fine** per salvare gli aggiornamenti dei criteri.

5. In **Contenitore di layout** fai clic su **policy** icona per **Testo** componente:

   ![Icona del componente Testo](./assets/map-components/edit-text-policy.png)

   Crea un nuovo criterio denominato **Testo WKND SPA**. Sotto **Plug-in** > **Formattazione** > seleziona tutte le caselle per abilitare opzioni di formattazione aggiuntive:

   ![Abilita formattazione editor Rich Text](assets/map-components/enable-formatting-rte.png)

   Sotto **Plug-in** > **Stili paragrafo** > seleziona la casella per **Abilita stili di paragrafo**:

   ![Abilita stili di paragrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Clic **Fine** per salvare l&#39;aggiornamento del criterio.

6. Accedi a **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   Dovresti anche poter modificare il `Text` e aggiungi altri stili di paragrafo in **a schermo intero** modalità.

   ![Modifica Rich Text A Schermo Intero](assets/map-components/full-screen-rte.png)

7. Dovresti anche poter trascinare e rilasciare un’immagine dalla **Asset Finder**:

   ![Trascina e rilascia l&#39;immagine](./assets/map-components/drag-drop-image.gif)

8. Aggiungi le tue immagini tramite [AEM Assets](http://localhost:4502/assets.html/content/dam) o installare la base di codice completata per lo standard [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Il [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) include molte immagini che possono essere riutilizzate sull’SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Gestione pacchetti installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect il contenitore di layout

Supporto per **Contenitore di layout** viene fornito automaticamente dall’SDK dell’editor SPA dell’AEM. Il **Contenitore di layout**, come indicato dal nome, è un **contenitore** componente. I componenti contenitore sono componenti che accettano strutture JSON che rappresentano *altro* e crearne un&#39;istanza dinamica.

Esaminiamo ulteriormente il Contenitore di layout.

1. Nell’IDE, apri **responsive-grid.component.ts** a `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   Il `AEMResponsiveGridComponent` è implementato come parte dell’SDK dell’SPA Editor dell’AEM ed è incluso nel progetto tramite `import-components`.

2. In un browser passa a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![API modello JSON - Griglia reattiva](./assets/map-components/responsive-grid-modeljson.png)

   Il **Contenitore di layout** il componente ha `sling:resourceType` di `wcm/foundation/components/responsivegrid` ed è riconosciuto dall&#39;editor SPA utilizzando `:type` proprietà, proprio come `Text` e `Image` componenti.

   Le stesse funzionalità di ridimensionamento di un componente mediante [Modalità Layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sono disponibili con l’editor SPA.

3. Torna a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Aggiungi ulteriori **Immagine** e provare a ridimensionarli utilizzando **Layout** opzione:

   ![Ridimensionare l’immagine utilizzando la modalità Layout](./assets/map-components/responsive-grid-layout-change.gif)

4. Riapri il modello JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) e osservano `columnClassNames` come parte del JSON:

   ![Nomi classi colonna](./assets/map-components/responsive-grid-classnames.png)

   Il nome della classe `aem-GridColumn--default--4` indica che il componente deve avere una larghezza di 4 colonne in base a una griglia a 12 colonne. Maggiori dettagli su [la griglia reattiva si trova qui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Torna all’IDE e nella `ui.apps` è presente una libreria lato client definita in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. Apri il file `less/grid.less`.

   Questo file determina i punti di interruzione (`default`, `tablet`, e `phone`) utilizzato da **Contenitore di layout**. Questo file è stato progettato per essere personalizzato in base alle specifiche del progetto. Attualmente i punti di interruzione sono impostati su `1200px` e `650px`.

6. Dovresti essere in grado di utilizzare le funzionalità reattive e i criteri Rich Text aggiornati di `Text` per creare una visualizzazione simile alla seguente:

   ![Creazione finale di esempio di capitolo](assets/map-components/final-page.png)

## Congratulazioni.  {#congratulations}

Congratulazioni, hai imparato a mappare i componenti SPA ai componenti AEM e hai implementato una nuova `Image` componente. Hai anche la possibilità di esplorare le funzionalità reattive del **Contenitore di layout**.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) oppure controllare il codice localmente passando alla filiale `Angular/map-components-solution`.

### Passaggi successivi {#next-steps}

[Navigazione e indirizzamento](navigation-routing.md) - Scopri come è possibile supportare più visualizzazioni nell’SPA mediante la mappatura su pagine AEM con l’SDK dell’Editor SPA. La navigazione dinamica viene implementata utilizzando il router Angular e aggiunta a un componente Intestazione esistente.

## Bonus: mantenere le configurazioni per il controllo del codice sorgente {#bonus}

In molti casi, soprattutto all’inizio di un progetto AEM, è utile mantenere le configurazioni, come i modelli e le relative policy di contenuto, per il controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano sullo stesso set di contenuti e configurazioni e possono garantire ulteriore coerenza tra gli ambienti. Quando un progetto raggiunge un certo livello di maturità, la pratica di gestione dei modelli può essere affidata a uno speciale gruppo di utenti esperti.

I passaggi successivi verranno eseguiti utilizzando l&#39;IDE di Visual Studio Code e [Sincronizzazione AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) ma potrebbe utilizzare qualsiasi strumento e IDE configurato per **tirare** o **importa** contenuti provenienti da un’istanza locale dell’AEM.

1. Nell&#39;IDE di Visual Studio Code verificare di avere **Sincronizzazione AEM VSCode** installato tramite l’estensione Marketplace:

   ![Sincronizzazione AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Espandi **ui.content** in Esplora progetti e passare a `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Clic con il pulsante destro** il `templates` cartella e seleziona **Importa da server AEM**:

   ![Modello di importazione VSCode](assets/map-components/import-aem-servervscode.png)

4. Ripeti i passaggi per importare il contenuto, ma seleziona la **criteri** cartella situata in `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspect `filter.xml` file che si trova in `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   Il `filter.xml` Il file è responsabile dell’identificazione dei percorsi dei nodi installati con il pacchetto. Osserva `mode="merge"` in ciascuno dei filtri che indica che il contenuto esistente non verrà modificato, viene aggiunto solo il nuovo contenuto. Poiché gli autori dei contenuti potrebbero aggiornare questi percorsi, è importante che una distribuzione del codice **non** sovrascrivi il contenuto. Consulta la [Documentazione di FileVault](https://jackrabbit.apache.org/filevault/filter.html) per ulteriori dettagli sull’utilizzo degli elementi filtro.

   Confronta `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` per comprendere i diversi nodi gestiti da ciascun modulo.
