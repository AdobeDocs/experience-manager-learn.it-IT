---
title: Mappatura di componenti SPA per AEM componenti | Guida introduttiva all’editor di SPA AEM e all’Angular
description: Scopri come mappare i componenti di Angular ai componenti di Adobe Experience Manager (AEM) con l’SDK JS dell’editor di SPA AEM. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA all’interno dell’editor di SPA AEM, in modo simile all’authoring tradizionale AEM.
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2380'
ht-degree: 1%

---

# Mappatura di componenti SPA per AEM componenti {#map-components}

Scopri come mappare i componenti di Angular ai componenti di Adobe Experience Manager (AEM) con l’SDK JS dell’editor di SPA AEM. La mappatura dei componenti consente agli utenti di apportare aggiornamenti dinamici ai componenti SPA all’interno dell’editor di SPA AEM, in modo simile all’authoring tradizionale AEM.

Questo capitolo descrive in modo più approfondito l’API del modello JSON AEM e come il contenuto JSON esposto da un componente AEM può essere inserito automaticamente in un componente Angular come proprietà.

## Obiettivo

1. Scopri come mappare AEM componenti su SPA componenti.
2. Comprendi la differenza tra i componenti **Contenitore** e i componenti **Contenuto**.
3. Crea un nuovo componente Angular associato a un componente AEM esistente.

## Cosa verrà creato

Questo capitolo analizzerà come il componente `Text` SPA fornito viene mappato sul componente AEM `Text`. Verrà creato un nuovo componente `Image` SPA che può essere utilizzato nel SPA e creato in AEM. Le funzioni predefinite dei criteri **Contenitore di layout** e **Editor modelli** verranno utilizzate anche per creare una visualizzazione con aspetto leggermente più vario.

![Esempio di capitolo di authoring finale](./assets/map-components/final-page.png)

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) o estrarre il codice localmente passando al ramo `Angular/map-components-solution`.

## Approccio di mappatura

Il concetto di base consiste nel mappare un componente SPA a un componente AEM. AEM componenti, esegui lato server, esporta il contenuto come parte dell’API del modello JSON. Il contenuto JSON viene utilizzato dal SPA, che esegue lato client nel browser. Viene creata una mappatura 1:1 tra SPA componenti e un componente AEM.

![Panoramica di alto livello sulla mappatura di un componente AEM a un componente di Angular](./assets/map-components/high-level-approach.png)

*Panoramica di alto livello sulla mappatura di un componente AEM a un componente di Angular*

## Inspect come componente testo

Il [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) fornisce un componente `Text` mappato al componente AEM [Testo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Questo è un esempio di un componente **content**, in quanto esegue il rendering di *content* da AEM.

Vediamo come funziona il componente.

### Inspect il modello JSON

1. Prima di passare al codice SPA, è importante comprendere il modello JSON AEM fornito. Passa alla [Libreria di componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) e visualizza la pagina per il componente Testo . La libreria dei componenti core fornisce esempi di tutti i componenti core AEM.
2. Seleziona la scheda **JSON** per uno degli esempi seguenti:

   ![Modello JSON per testo](./assets/map-components/text-json.png)

   Dovresti visualizzare tre proprietà: `text`, `richText` e `:type`.

   `:type` è una proprietà riservata che elenca il  `sling:resourceType` (o percorso) del componente AEM. Il valore di `:type` viene utilizzato per mappare il componente AEM al componente SPA.

   `text` e  `richText` sono proprietà aggiuntive che verranno esposte al componente SPA.

### Inspect il componente Testo

1. Apri un nuovo terminale e passa alla cartella `ui.frontend` all’interno del progetto. Esegui `npm install` e quindi `npm start` per avviare il server di sviluppo del **webpack**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   Il modulo `ui.frontend` è attualmente configurato per utilizzare il modello [JSON di simulazione](./integrate-spa.md#mock-json).

2. Dovresti visualizzare una nuova finestra del browser aperta su [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![Server di sviluppo Webpack con contenuti fittizi](assets/map-components/initial-start.png)

3. Nell’IDE che preferisci, apri Progetto AEM per il SPA WKND. Espandi il modulo `ui.frontend` e apri il file **text.component.ts** in `ui.frontend/src/app/components/text/text.component.ts`:

   ![Codice sorgente del componente di Angular Text.js](assets/map-components/vscode-ide-text-js.png)

4. La prima area da ispezionare è la `class TextComponent` alla ~riga 35:

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

   [@Input()](https://angular.io/api/core/Input) decorator viene utilizzato per dichiarare i campi i cui valori sono impostati tramite l&#39;oggetto JSON mappato, rivisto in precedenza.

   `@HostBinding('innerHtml') get content()` è un metodo che espone il contenuto di testo creato dal valore di  `this.text`. Se il contenuto è in formato RTF (determinato dal flag `this.richText` ), la sicurezza integrata dell’Angular viene bypassata. Angular [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) viene utilizzato per &quot;scorrere&quot; l&#39;HTML non elaborato ed evitare vulnerabilità di vulnerabilità cross-site scripting. Il metodo è associato alla proprietà `innerHtml` utilizzando il decoratore [@HostBinding](https://angular.io/api/core/HostBinding).

5. Ispeziona quindi il `TextEditConfig` in ~linea 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   Il codice riportato sopra ha la responsabilità di determinare quando eseguire il rendering del segnaposto nell’ambiente di authoring AEM. Se il metodo `isEmpty` restituisce **true**, viene eseguito il rendering del segnaposto.

6. Infine, dai un&#39;occhiata alla chiamata `MapTo` su ~line 53:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **** MapTois fornito dall’SDK JS AEM Editor (`@adobe/cq-angular-editable-components`). Il percorso `wknd-spa-angular/components/text` rappresenta il `sling:resourceType` del componente AEM. Questo percorso viene confrontato con il `:type` esposto dal modello JSON osservato in precedenza. **** MapAnalizza la risposta del modello JSON e passa i valori corretti alle  `@Input()` variabili del componente SPA.

   Puoi trovare la definizione del componente AEM `Text` in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Prova a modificare il file **en.model.json** in `ui.frontend/src/mocks/json/en.model.json`.

   Alla riga 62 ~aggiorna il primo valore `Text` per utilizzare un tag **`H1`** e **`u`**:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Torna al browser per vedere gli effetti forniti dal server di sviluppo del **webpack**:

   ![Modello di testo aggiornato](assets/map-components/updated-text-model.png)

   Prova a attivare/disattivare la proprietà `richText` tra **true** / **false** per visualizzare la logica di rendering in azione.

8. Inspect **text.component.html** in `ui.frontend/src/app/components/text/text.component.html`.

   Questo file è vuoto perché l&#39;intero contenuto del componente verrà impostato dalla proprietà `innerHTML` .

9. Inspect il **app.module.ts** in `ui.frontend/src/app/app.module.ts`.

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

   Il **TextComponent** non è incluso esplicitamente, ma piuttosto dinamicamente tramite **AEMResponsiveGridComponent** fornito dall&#39;SDK JS dell&#39;editor di SPA AEM. Pertanto deve essere elencato nella matrice **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components).

## Creare il componente Immagine

Quindi, crea un componente di Angular `Image` mappato sul AEM [componente Immagine](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). Il componente `Image` è un altro esempio di un componente **content** .

### Inspect JSON

Prima di passare al codice SPA, controlla il modello JSON fornito da AEM.

1. Passa a [Esempi di immagini nella libreria Componenti core](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON del componente core immagine](./assets/map-components/image-json.png)

   Le proprietà di `src`, `alt` e `title` verranno utilizzate per popolare il componente SPA `Image`.

   >[!NOTE]
   >
   > Esistono altre proprietà immagine esposte (`lazyEnabled`, `widths`) che consentono a uno sviluppatore di creare un componente adattivo e a caricamento lento. Il componente creato in questa esercitazione sarà semplice e **non** utilizzerà queste proprietà avanzate.

2. Torna all’IDE e apri `en.model.json` in `ui.frontend/src/mocks/json/en.model.json`. Poiché si tratta di un nuovo componente di rete per il nostro progetto, dobbiamo &quot;deridere&quot; l’immagine JSON.

   A ~riga 70 aggiungi una voce JSON per il modello `image` (non dimenticare la virgola finale `,` dopo il secondo `text_386303036`) e aggiorna l’array `:itemsOrder`.

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

   Il progetto include un&#39;immagine di esempio in `/mock-content/adobestock-140634652.jpeg` che verrà utilizzata con il **webpack dev server**.

   Puoi visualizzare il file [en.model.json completo qui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. Aggiungi una foto stock da visualizzare dal componente.

   Crea una nuova cartella denominata **images** sotto `ui.frontend/src/mocks`. Scarica [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) e inseriscilo nella cartella **images** appena creata. Se lo desideri, puoi usare la tua immagine.

### Implementare il componente Immagine

1. Arrestare il **webpack dev server** se avviato.
2. Crea un nuovo componente Immagine eseguendo il comando CLI di Angular `ng generate component` dalla cartella `ui.frontend`:

   ```shell
   $ ng generate component components/image
   ```

3. Nell&#39;IDE, apri **image.component.ts** in `ui.frontend/src/app/components/image/image.component.ts` e aggiorna come segue:

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

   `ImageEditConfig` è la configurazione per determinare se eseguire il rendering del segnaposto autore in AEM, in base a se la  `src` proprietà è compilata.

   `@Input()` di  `src`,  `alt` e  `title` sono le proprietà mappate dall’API JSON.

   `hasImage()` è un metodo che determina se l&#39;immagine deve essere sottoposta a rendering.

   `MapTo` mappa il componente SPA al componente AEM in  `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. Apri **image.component.html** e aggiornalo come segue:

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Questo renderà l&#39;elemento `<img>` se `hasImage` restituisce **true**.

5. Apri **image.component.scss** e aggiornalo come segue:

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
   > La regola `:host-context` è **critica** perché il segnaposto dell&#39;editor SPA AEM funzioni correttamente. Per tutti i componenti SPA destinati all’authoring nell’editor di pagine AEM questa regola è richiesta almeno.

6. Apri `app.module.ts` e aggiungi `ImageComponent` alla matrice `entryComponents`:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Come il `TextComponent`, il `ImageComponent` viene caricato in modo dinamico e deve essere incluso nella matrice `entryComponents`.

7. Avviare il **webpack dev server** per visualizzare il rendering `ImageComponent`.

   ```shell
   $ npm run start:mock
   ```

   ![Immagine aggiunta al simulacro](assets/map-components/image-added-mock.png)

   *Immagine aggiunta al SPA*

   >[!NOTE]
   >
   > **Sfida** bonus: Implementa un nuovo metodo per visualizzare il valore di  `title` come didascalia sotto l’immagine.

## Aggiorna criteri in AEM

Il componente `ImageComponent` è visibile solo nel server di sviluppo del **webpack**. Quindi, distribuisci il SPA aggiornato per AEM e aggiornare i criteri dei modelli.

1. Arresta il **server di sviluppo del webpack** e dalla **radice** del progetto, distribuisci le modifiche a AEM utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Dalla schermata iniziale AEM passare a **[!UICONTROL Strumenti]** > **[!UICONTROL Modelli]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Seleziona e modifica la **SPA pagina**:

   ![Modifica modello di pagina SPA](assets/map-components/edit-spa-page-template.png)

3. Seleziona il **Contenitore di layout** e fai clic sull&#39;icona **policy** per modificare il criterio:

   ![Criterio contenitore di layout](./assets/map-components/layout-container-policy.png)

4. In **Componenti consentiti** > **Angular SPA WKND - Contenuto** > controlla il componente **Immagine**:

   ![Componente immagine selezionato](assets/map-components/check-image-component.png)

   In **Componenti predefiniti** > **Aggiungi mappatura** e scegli il componente **Immagine - WKND SPA Angular - Contenuto**:

   ![Imposta componenti predefiniti](assets/map-components/default-components.png)

   Immetti un **tipo MIME** di `image/*`.

   Fai clic su **Fine** per salvare gli aggiornamenti dei criteri.

5. Nel **Contenitore di layout** fai clic sull&#39;icona **policy** per il componente **Testo**:

   ![Icona del criterio del componente testo](./assets/map-components/edit-text-policy.png)

   Crea un nuovo criterio denominato **WKND SPA Text**. Alla voce **Plugin** > **Formattazione** > controlla tutte le caselle per abilitare ulteriori opzioni di formattazione:

   ![Abilita formattazione RTE](assets/map-components/enable-formatting-rte.png)

   Alla voce **Plugin** > **Stili di paragrafo** > seleziona la casella per **Abilitare gli stili di paragrafo**:

   ![Abilita stili di paragrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Fai clic su **Fine** per salvare l&#39;aggiornamento dei criteri.

6. Passa alla **Home page** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   È inoltre necessario poter modificare il componente `Text` e aggiungere stili di paragrafo aggiuntivi in modalità **a schermo intero**.

   ![Modifica RTF a schermo intero](assets/map-components/full-screen-rte.png)

7. È inoltre necessario essere in grado di trascinare e rilasciare un&#39;immagine da **Asset Finder**:

   ![Trascina immagine](./assets/map-components/drag-drop-image.gif)

8. Aggiungi le tue immagini tramite [AEM Assets](http://localhost:4502/assets.html/content/dam) o installa la base di codice finita per il sito di riferimento standard [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Il [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) include molte immagini che possono essere riutilizzate sul SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect il contenitore di layout

Il supporto per il **Contenitore di layout** viene fornito automaticamente dall’SDK dell’editor di SPA AEM. Il **Contenitore di layout**, come indicato dal nome, è un componente **contenitore**. I componenti contenitore sono componenti che accettano strutture JSON che rappresentano *altri componenti* e le creano in modo dinamico un’istanza.

Esaminiamo ulteriormente il Contenitore di layout.

1. Nell&#39;IDE apri **responsive-grid.component.ts** in `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   Il `AEMResponsiveGridComponent` viene implementato come parte dell’SDK dell’editor di SPA AEM e viene incluso nel progetto tramite `import-components`.

2. Nel browser passa a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![API del modello JSON - Griglia reattiva](./assets/map-components/responsive-grid-modeljson.png)

   Il componente **Contenitore di layout** ha un `sling:resourceType` di `wcm/foundation/components/responsivegrid` ed è riconosciuto dall’editor SPA utilizzando la proprietà `:type` , proprio come i componenti `Text` e `Image` .

   Le stesse funzionalità di ridimensionamento di un componente utilizzando [Modalità layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sono disponibili con l’Editor di SPA.

3. Torna a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Aggiungi altri componenti **Immagine** e prova a ridimensionarli utilizzando l&#39;opzione **Layout** :

   ![Ridimensionare l’immagine utilizzando la modalità Layout](./assets/map-components/responsive-grid-layout-change.gif)

4. Riapri il modello JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) e osserva il `columnClassNames` come parte del JSON:

   ![Nomi delle classi cloud](./assets/map-components/responsive-grid-classnames.png)

   Il nome della classe `aem-GridColumn--default--4` indica che il componente deve essere largo 4 colonne in base a una griglia a 12 colonne. Ulteriori dettagli sulla [griglia reattiva sono disponibili qui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Torna all’IDE e nel modulo `ui.apps` è presente una libreria lato client definita in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. Aprire il file `less/grid.less`.

   Questo file determina i punti di interruzione (`default`, `tablet` e `phone`) utilizzati dal **Contenitore di layout**. Questo file deve essere personalizzato in base alle specifiche del progetto. Attualmente i punti di interruzione sono impostati su `1200px` e `650px`.

6. Dovresti essere in grado di utilizzare le funzionalità reattive e i criteri di testo RTF aggiornati del componente `Text` per creare una visualizzazione come la seguente:

   ![Esempio di capitolo di authoring finale](assets/map-components/final-page.png)

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato a mappare SPA componenti su componenti AEM e hai implementato un nuovo componente `Image`. Hai anche la possibilità di esplorare le funzionalità reattive del **Contenitore di layout**.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) o estrarre il codice localmente passando al ramo `Angular/map-components-solution`.

### Passaggi successivi {#next-steps}

[Navigazione e routing](navigation-routing.md) : scopri come è possibile supportare più visualizzazioni nel SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica viene implementata tramite Router Angular e aggiunta a un componente Header esistente.

## Bonus - Configurazioni permanenti al controllo del codice sorgente {#bonus}

In molti casi, specialmente all’inizio di un progetto AEM, è utile mantenere le configurazioni, come i modelli e i relativi criteri dei contenuti, al controllo del codice sorgente. In questo modo tutti gli sviluppatori lavorano con lo stesso set di contenuti e configurazioni e possono garantire un’ulteriore coerenza tra gli ambienti. Quando un progetto raggiunge un certo livello di maturità, la gestione dei modelli può essere affidata a un gruppo speciale di utenti.

I passaggi successivi si svolgeranno utilizzando l&#39;IDE di codice di Visual Studio e [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) ma potrebbero essere eseguiti utilizzando qualsiasi strumento e qualsiasi IDE configurato per **pull** o **import** da un&#39;istanza locale di AEM.

1. Nell&#39;IDE di codice di Visual Studio, assicurati di aver installato **VSCode AEM Sync** tramite l&#39;estensione Marketplace:

   ![Sincronizzazione AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Espandi il modulo **ui.content** in Project explorer e passa a `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Fai clic con** il pulsante destro del mouse sulla  `templates` cartella e seleziona  **Importa da AEM server**:

   ![Modello di importazione VSCode](assets/map-components/import-aem-servervscode.png)

4. Ripeti i passaggi per importare il contenuto, ma seleziona la cartella **policy** che si trova in `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspect il file `filter.xml` che si trova in `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Il file `filter.xml` è responsabile dell&#39;identificazione dei percorsi dei nodi che verranno installati con il pacchetto. Osserva `mode="merge"` su ciascuno dei filtri che indica che il contenuto esistente non verrà modificato, ma che viene aggiunto solo un nuovo contenuto. Poiché gli autori di contenuti possono aggiornare questi percorsi, è importante che una distribuzione di codice sovrascriva il contenuto **non**. Per ulteriori informazioni sull&#39;utilizzo degli elementi filtro, consulta la [documentazione FileVault](https://jackrabbit.apache.org/filevault/filter.html) .

   Confronta `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` per comprendere i diversi nodi gestiti da ciascun modulo.
