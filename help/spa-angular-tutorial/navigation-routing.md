---
title: Aggiunta di navigazione e routing | Guida introduttiva all'editor SPA AEM e Angular
description: Scoprite come sono supportate più viste nell’area SPA utilizzando AEM pagine e l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando route angolari e aggiunta a un componente Intestazione esistente.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '2720'
ht-degree: 1%

---


# Aggiunta di navigazione e routing {#navigation-routing}

Scoprite come sono supportate più viste nell’area SPA utilizzando AEM pagine e l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando route angolari e aggiunta a un componente Intestazione esistente.

## Obiettivo

1. Comprendere le opzioni di routing del modello SPA disponibili quando si utilizza SPA Editor.
2. Scoprite come utilizzare il routing [](https://angular.io/guide/router) angolare per navigare tra le diverse viste dell’area di protezione.
3. Implementa una navigazione dinamica guidata dalla gerarchia di pagine AEM.

## Cosa verrà creato

Questo capitolo aggiunge un menu di navigazione a un `Header` componente esistente. Il menu di navigazione è gestito dalla gerarchia di pagine AEM e utilizza il modello JSON fornito dal componente [core di](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)navigazione.

![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

## Prerequisiti

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale.

### Ottenere il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Distribuire la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzate [AEM 6.x](overview.md#compatibility) , aggiungete il `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installate il pacchetto finito per il sito [di riferimento](https://github.com/adobe/aem-guides-wknd/releases/latest)WKND tradizionale. Le immagini fornite dal sito [di riferimento](https://github.com/adobe/aem-guides-wknd/releases/latest) WKND verranno riutilizzate nell&#39;SPA WKND. Il pacchetto può essere installato tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o estrarre il codice localmente passando al ramo `Angular/navigation-routing-solution`.

## Aggiornamenti  Inspect HeaderComponent {#inspect-header}

Nei capitoli precedenti, il `HeaderComponent` componente veniva aggiunto come componente Angolare puro incluso tramite `app.component.html`. In questo capitolo, il `HeaderComponent` componente viene rimosso dall’app e verrà aggiunto tramite l’Editor [](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)modelli. Questo consente agli utenti di configurare il menu di navigazione del `HeaderComponent` modulo dall&#39;interno del AEM.

>[!NOTE]
>
> Diversi aggiornamenti CSS e JavaScript sono già stati apportati alla base di codice per avviare questo capitolo. Per concentrarsi sui concetti di base, non vengono discusse **tutte** le modifiche del codice. Potete visualizzare le modifiche complete [qui](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. Nell&#39;IDE di vostra scelta, aprite il progetto di avvio SPA per questo capitolo.
2. Sotto il `ui.frontend` modulo ispezionare il file `header.component.ts` : `ui.frontend/src/app/components/header/header.component.ts`.

   Sono stati eseguiti diversi aggiornamenti, tra cui l’aggiunta di un componente `HeaderEditConfig` e di un `MapTo` componente per consentirne la mappatura a un componente AEM `wknd-spa-angular/components/header`.

   ```js
   /* header.component.ts */
   ...
   const HeaderEditConfig = {
       ...
   };
   
   @Component({
   selector: 'app-header',
   templateUrl: './header.component.html',
   styleUrls: ['./header.component.scss']
   })
   export class HeaderComponent implements OnInit {
   @Input() items: object[];
       ...
   }
   ...
   MapTo('wknd-spa-angular/components/header')(withRouter(Header), HeaderEditConfig);
   ```

   Prendete nota dell’ `@Input()` annotazione per `items`. `items` conterrà un array di oggetti di navigazione passati da AEM.

3. Nel `ui.apps` modulo, esamina la definizione del componente del AEM `Header` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   Il `Header` componente AEM erediterà tutte le funzionalità del componente [core di](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) navigazione tramite la `sling:resourceSuperType` proprietà.

## Aggiungere il componente HeaderComponent al modello SPA {#add-header-template}

1. Aprite un browser e accedete a AEM, [http://localhost:4502/](Http://localhost:4502/). La base di codice iniziale dovrebbe essere già distribuita.
2. Andate al modello **[!UICONTROL di pagina]** SPA: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Selezionate il contenitore **[!UICONTROL di layout]** principale più esterno e fate clic sull&#39;icona **[!UICONTROL Criterio]** . Fate attenzione a **non** selezionare il Contenitore **[!UICONTROL di]** layout non bloccato per la creazione.

   ![Selezionate l&#39;icona del criterio contenitore del layout principale](assets/navigation-routing/root-layout-container-policy.png)

4. Copiate il criterio corrente e create un nuovo criterio denominato Struttura **** SPA:

   ![Criteri di struttura SPA](assets/map-components/spa-policy-update.png)

   In Componenti **[!UICONTROL consentiti]** > **[!UICONTROL Generale]** > selezionare il componente Contenitore **[!UICONTROL di]** layout.

   In Componenti **** consentiti > ANGOLARE SPA **[!UICONTROL WKND - STRUTTURA]** > selezionare il componente **[!UICONTROL Intestazione]** :

   ![Seleziona componente intestazione](assets/map-components/select-header-component.png)

   In Componenti **** consentiti > ANGOLARE SPA **[!UICONTROL WKND - Contenuto]** > selezionare i componenti **[!UICONTROL Immagine]** e **[!UICONTROL Testo]** . È necessario selezionare 4 componenti totali.

   Click **[!UICONTROL Done]** to save the changes.

5. **Aggiorna la pagina.** Aggiungete il componente **[!UICONTROL Intestazione]** sopra il contenitore **[!UICONTROL di]** layout non bloccato:

   ![aggiunta del componente Intestazione al modello](./assets/navigation-routing/add-header-component.gif)

6. Selezionate il componente **[!UICONTROL Intestazione]** e fate clic sull&#39;icona **Criterio** corrispondente per modificare il criterio.

   ![Fare clic su Criterio intestazione](assets/navigation-routing/header-policy-icon.png)

7. Create un nuovo criterio con un Titolo **** criterio di **&quot;WKND SPA Header&quot;**.

   In **[!UICONTROL Proprietà]**:

   * Impostare la directory principale **[!UICONTROL di]** navigazione su `/content/wknd-spa-angular/us/en`.
   * Impostate **[!UICONTROL Escludi livelli]** radice su **1**.
   * Deselezionate **[!UICONTROL Raccogli tutte le pagine]** figlie.
   * Impostate la profondità **[!UICONTROL della struttura di]** navigazione su **3**.

   ![Configurare i criteri di intestazione](assets/navigation-routing/header-policy.png)

   Questo raccoglierà i 2 livelli di navigazione in profondità sotto `/content/wknd-spa-angular/us/en`.

8. Dopo aver salvato le modifiche, è necessario visualizzare il popolato `Header` come parte del modello:

   ![Componente intestazione compilata](assets/navigation-routing/populated-header.png)

## Creare pagine figlie

Create quindi altre pagine in AEM che fungano da diverse viste nell’area di protezione. Verranno inoltre analizzati la struttura gerarchica del modello JSON fornito da AEM.

1. Passate alla console **Siti** : [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Selezionate la home page **angolare SPA** WKND e fate clic su **[!UICONTROL Crea]** > **[!UICONTROL Pagina]**:

   ![Crea nuova pagina](assets/navigation-routing/create-new-page.png)

2. In **[!UICONTROL Modello]** selezionate Pagina **** SPA. In **[!UICONTROL Proprietà]** immettete **&quot;Pagina 1&quot;** per il **[!UICONTROL Titolo]** e **&quot;pagina-1&quot;** come nome.

   ![Immettere le proprietà della pagina iniziale](assets/navigation-routing/initial-page-properties.png)

   Fate clic su **[!UICONTROL Crea]** e, nella finestra di dialogo a comparsa, fate clic su **[!UICONTROL Apri]** per aprire la pagina nell’Editor AEM SPA.

3. Aggiungete un nuovo componente **[!UICONTROL Testo]** al contenitore **[!UICONTROL di]** layout principale. Modificate il componente e inserite il testo: **&quot;Pagina 1&quot;** con l’editor Rich Text e l’elemento **H1** (per modificare gli elementi di paragrafo dovrete passare alla modalità a schermo intero)

   ![Contenuto di esempio pagina 1](assets/navigation-routing/page-1-sample-content.png)

   Potete aggiungere contenuti aggiuntivi, come un’immagine.

4. Tornate alla  console AEM Sites e ripetete i passaggi descritti sopra, creando una seconda pagina denominata **&quot;Pagina 2&quot;** come pari a **Pagina 1**. Aggiungete il contenuto alla **pagina 2** in modo da facilitarne l’identificazione.
5. Infine, create una terza pagina, **&quot;Pagina 3&quot;** ma come **figlia** di **Pagina 2**. Una volta completata la gerarchia del sito dovrebbe essere simile a quella riportata di seguito:

   ![Gerarchia del sito di esempio](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. In una nuova scheda, apri l&#39;API del modello JSON fornita da AEM: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Questo contenuto JSON viene richiesto al primo caricamento dell&#39;SPA. La struttura esterna si presenta come segue:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {},
       "/content/wknd-spa-angular/us/en/home/page-2/page-3": {}
       }
   }
   ```

   In `:children` questa sezione viene visualizzata una voce per ciascuna pagina creata. Il contenuto per tutte le pagine è in questa richiesta JSON iniziale. Una volta implementato il routing di navigazione, le viste successive dell&#39;SPA verranno caricate rapidamente, dal momento che il contenuto è già disponibile sul lato client.

   Non è saggio caricare **TUTTO** il contenuto di un&#39;app SPA nella richiesta JSON iniziale, in quanto ciò rallenterebbe il caricamento iniziale della pagina. Nella sezione che segue viene illustrato come vengono raccolte le profondità di pagine con l’ereditarietà.

7. Andate al modello **SPA Root** in: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Fare clic sul menu **[!UICONTROL Proprietà]** pagina > Criterio **** pagina:

   ![Aprire i criteri di pagina per SPA Root](assets/navigation-routing/open-page-policy.png)

8. Il modello **SPA Root** dispone di una scheda Struttura **** gerarchica aggiuntiva per controllare il contenuto JSON raccolto. La profondità **** struttura determina la profondità nella gerarchia del sito per la raccolta delle pagine figlie sotto la **radice**. È inoltre possibile utilizzare il campo Pattern **** struttura per filtrare ulteriori pagine in base a un&#39;espressione regolare.

   Aggiorna la profondità **[!UICONTROL della]** struttura su **&quot;2&quot;**:

   ![Aggiorna profondità struttura](assets/navigation-routing/update-structure-depth.png)

   Fate clic su **[!UICONTROL Fine]** per salvare le modifiche al criterio.

9. Riaprite il modello JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {}
       }
   }
   ```

   Il percorso **Pagina 3** è stato rimosso: `/content/wknd-spa-angular/us/en/home/page-2/page-3` dal modello JSON iniziale.

   In seguito verrà illustrato come l’SDK dell’editor SPA AEM possa caricare in modo dinamico contenuti aggiuntivi.

## Implementare la navigazione

Implementate quindi il menu di navigazione con un nuovo `NavigationComponent`. Potremmo aggiungere il codice direttamente in `header.component.html` ma una procedura migliore è evitare componenti di grandi dimensioni. Al contrario, implementate un `NavigationComponent` che potrebbe essere riutilizzato in un secondo momento.

1. Rivedete il JSON esposto dal `Header` componente AEM all’indirizzo [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-angular/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-angular/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA Angular Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-angular/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-angular/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-angular/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-angular/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-angular/components/header"
   ```

   La natura gerarchica delle pagine AEM è modellata nel JSON che può essere utilizzato per compilare un menu di navigazione. Ricorda che il `Header` componente eredita tutte le funzionalità del componente [core di](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html) navigazione e che il contenuto esposto tramite JSON verrà automaticamente mappato sull’ `@Input` annotazione angolare.

2. Aprite una nuova finestra del terminale e andate alla `ui.frontend` cartella del progetto SPA. Create un nuovo `NavigationComponent` utilizzando lo strumento CLI angolare:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Create quindi una classe denominata `NavigationLink` utilizzando l&#39;interfaccia CLI angolare nella nuova `components/navigation` directory creata:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Tornate all&#39;IDE di vostra scelta e aprite il file all&#39; `navigation-link.ts` indirizzo `/src/app/components/navigation/navigation-link.ts`.

   ![Apri file navigation-link.ts](assets/navigation-routing/ide-navigation-link-file.png)

5. Compilate `navigation-link.ts` con le seguenti opzioni:

   ```js
   export class NavigationLink {
   
       title: string;
       path: string;
       url: string;
       level: number;
       children: NavigationLink[];
       active: boolean;
   
       constructor(data) {
           this.path = data.path;
           this.title = data.title;
           this.url = data.url;
           this.level = data.level;
           this.active = data.active;
           this.children = data.children.map( item => {
               return new NavigationLink(item);
           });
       }
   }
   ```

   Si tratta di una classe semplice che rappresenta un singolo collegamento di navigazione. Nel costruttore di classe ci aspettiamo `data` di essere l&#39;oggetto JSON passato da AEM. Questa classe verrà utilizzata sia all&#39;interno `NavigationComponent` che `HeaderComponent` per comporre facilmente la struttura di navigazione.

   Non viene eseguita alcuna trasformazione di dati, questa classe viene creata principalmente per digitare fortemente il modello JSON. Si noti che `this.children` viene digitato come `NavigationLink[]` e che il costruttore crea in modo ricorsivo nuovi `NavigationLink` oggetti per ciascuno degli elementi dell&#39; `children` array. Ricordate che il modello JSON per l&#39; `Header` oggetto è gerarchico.

6. Aprire il file `navigation-link.spec.ts`. Questo è il file di prova per la `NavigationLink` classe. Aggiornalo con i seguenti elementi:

   ```js
   import { NavigationLink } from './navigation-link';
   
   describe('NavigationLink', () => {
       it('should create an instance', () => {
           const data = {
               children: [],
               level: 1,
               active: false,
               path: '/content/wknd-spa-angular/us/en/home/page-1',
               description: null,
               url: '/content/wknd-spa-angular/us/en/home/page-1.html',
               lastModified: 1589429385100,
               title: 'Page 1'
           };
           expect(new NavigationLink(data)).toBeTruthy();
       });
   });
   ```

   Si noti che `const data` segue lo stesso modello JSON precedentemente esaminato per un singolo collegamento. Non si tratta di un solido test di unità, ma dovrebbe essere sufficiente testare il costruttore di `NavigationLink`.

7. Aprire il file `navigation.component.ts`. Aggiornalo con i seguenti elementi:

   ```js
   import { Component, OnInit, Input } from '@angular/core';
   import { NavigationLink } from './navigation-link';
   
   @Component({
   selector: 'app-navigation',
   templateUrl: './navigation.component.html',
   styleUrls: ['./navigation.component.scss']
   })
   export class NavigationComponent implements OnInit {
   
       @Input() items: object[];
   
       constructor() { }
   
       get navigationLinks(): NavigationLink[] {
   
           if (this.items && this.items.length > 0) {
               return this.items.map(item => {
                   return new NavigationLink(item);
               });
           }
   
           return null;
       }
   
       ngOnInit() {}
   
   }
   ```

   `NavigationComponent` prevede un `object[]` nome `items` che sia il modello JSON da AEM. Questa classe espone un singolo metodo `get navigationLinks()` che restituisce un array di `NavigationLink` oggetti.

8. Aprite il file `navigation.component.html` e aggiornatelo con i seguenti elementi:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Viene generato un valore iniziale `<ul>` e il `get navigationLinks()` metodo viene chiamato da `navigation.component.ts`. Un oggetto `<ng-container>` viene utilizzato per effettuare una chiamata a un modello denominato `recursiveListTmpl` e lo trasmette `navigationLinks` come variabile denominata `links`.

   Aggiungi il `recursiveListTmpl` successivo:

   ```html
   <ng-template #recursiveListTmpl let-links="links">
       <li *ngFor="let link of links" class="{{'navigation__item navigation__item--' + link.level}}">
           <a [routerLink]="link.url" class="navigation__item-link" [title]="link.title" [attr.aria-current]="link.active">
               {{link.title}}
           </a>
           <ul *ngIf="link.children && link.children.length > 0">
               <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: link.children }"></ng-container>
           </ul>
       </li>
   </ng-template>
   ```

   Qui viene implementato il resto del rendering per il collegamento di navigazione. Tenere presente che la variabile `link` è di tipo `NavigationLink` e che sono disponibili tutti i metodi/proprietà creati da tale classe. [`[routerLink]`](https://angular.io/api/router/RouterLink) viene utilizzato al posto dell&#39; `href` attributo normale. Questo ci consente di collegarci a percorsi specifici nell&#39;app, senza un aggiornamento a pagina intera.

   La parte ricorsiva della navigazione viene implementata anche creando un&#39;altra `<ul>` se la corrente `link` ha una matrice non vuota `children` .

9. Aggiornamento `navigation.component.spec.ts` per aggiungere il supporto per `RouterTestingModule`:

   ```diff
    ...
   + import { RouterTestingModule } from '@angular/router/testing';
    ...
    beforeEach(async(() => {
       TestBed.configureTestingModule({
   +   imports: [ RouterTestingModule ],
       declarations: [ NavigationComponent ]
       })
       .compileComponents();
    }));
    ...
   ```

   L’aggiunta `RouterTestingModule` è obbligatoria perché il componente utilizza `[routerLink]`.

10. Aggiornate `navigation.component.scss` per aggiungere alcuni stili di base al `NavigationComponent`:

   ```scss
   @import "~src/styles/variables";
   
   $link-color: $black;
   $link-hover-color: $white;
   $link-background: $black;
   
   :host-context {
       display: block;
       width: 100%;
   }
   
   .navigation__item {
       list-style: none;
   }
   
   .navigation__item-link {
       color: $link-color;
       font-size: $font-size-large;
       text-transform: uppercase;
       padding: $gutter-padding;
       display: flex;
       border-bottom: 1px solid $gray;
   
       &:hover {
           background: $link-background;
           color: $link-hover-color;
       }
   
   }
   ```

## Aggiornare il componente dell’intestazione

Ora che `NavigationComponent` è stato implementato, il `HeaderComponent` sistema deve essere aggiornato per farvi riferimento.

1. Aprite un terminale e andate alla `ui.frontend` cartella all&#39;interno del progetto SPA. Avviare il server **di dev** webpack:

   ```shell
   $ npm start
   ```

2. Open a browser tab and navigate to [http://localhost:4200/](Http://localhost:4200/).

   Il server **di sviluppo** webpack deve essere configurato per il proxy del modello JSON da un&#39;istanza locale di AEM (`ui.frontend/proxy.conf.json`). Questo ci consentirà di eseguire la codifica direttamente rispetto al contenuto creato in AEM da una precedente esercitazione.

   ![menu attivato](./assets/navigation-routing/nav-toggle-static.gif)

   Al `HeaderComponent` momento la funzionalità di attivazione/disattivazione del menu è già implementata. Quindi, aggiungete il componente di navigazione.

3. Tornate all’IDE di vostra scelta e aprite il file `header.component.ts` in `ui.frontend/src/app/components/header/header.component.ts`.
4. Aggiornare il `setHomePage()` metodo per rimuovere la stringa codificata e utilizzare i prop dinamici passati dal componente AEM:

   ```js
   /* header.component.ts */
   import { NavigationLink } from '../navigation/navigation-link';
   ...
    setHomePage() {
       if (this.hasNavigation) {
           const rootNavigationLink: NavigationLink = new NavigationLink(this.items[0]);
           this.isHome = rootNavigationLink.path === this.route.snapshot.data.path;
           this.homePageUrl = rootNavigationLink.url;
       }
   }
   ...
   ```

   Viene `NavigationLink` creata una nuova istanza di `items[0]`, la radice del modello JSON di navigazione passata da AEM. `this.route.snapshot.data.path` restituisce il percorso della route Angular corrente. Questo valore viene utilizzato per determinare se il percorso corrente è la **home page**. `this.homePageUrl` viene utilizzato per compilare il collegamento di ancoraggio sul **logo**.

5. Aprite `header.component.html` e sostituite il segnaposto statico per la navigazione con un riferimento alla nuova creazione `NavigationComponent`:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` l&#39;attributo passa `@Input() items` da `HeaderComponent` a `NavigationComponent` dove verrà creata la navigazione.

6. Aprite `header.component.spec.ts` e aggiungete una dichiarazione per `NavigationComponent`:

   ```diff
       /* header.component.spect.ts */
   +   import { NavigationComponent } from '../navigation/navigation.component';
   
       describe('HeaderComponent', () => {
       let component: HeaderComponent;
       let fixture: ComponentFixture<HeaderComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           imports: [ RouterTestingModule ],
   +       declarations: [ HeaderComponent, NavigationComponent ]
           })
           .compileComponents();
       }));
   ```

   Poiché l’apparecchio `NavigationComponent` è ora utilizzato come parte del `HeaderComponent` letto di prova, deve essere dichiarato come parte del letto di prova.

7. Salvate le modifiche apportate ai file aperti e tornate al server **di sviluppo** webpack: [http://localhost:4200/](Http://localhost:4200/)

   ![Navigazione intestazione completata](assets/navigation-routing/completed-header.png)

   Aprite la navigazione facendo clic sul menu e visualizzate i collegamenti di navigazione compilati. È necessario essere in grado di passare a diverse viste dell&#39;SPA.

## Comprendere il ciclo SPA

Ora che la navigazione è stata implementata, ispezionare il routing in AEM.

1. Nell&#39;IDE aprire il file `app-routing.module.ts` in `ui.frontend/src/app`.

   ```js
   /* app-routing.module.ts */
   import { AemPageDataResolver, AemPageRouteReuseStrategy } from '@adobe/cq-angular-editable-components';
   import { NgModule } from '@angular/core';
   import { RouteReuseStrategy, RouterModule, Routes, UrlMatchResult, UrlSegment } from '@angular/router';
   import { PageComponent } from './components/page/page.component';
   
   export function AemPageMatcher(url: UrlSegment[]): UrlMatchResult {
       if (url.length) {
           return {
               consumed: url,
               posParams: {
                   path: url[url.length - 1]
               }
           };
       }
   }
   
   const routes: Routes = [
       {
           matcher: AemPageMatcher,
           component: PageComponent,
           resolve: {
               path: AemPageDataResolver
           }
       }
   ];
   @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule],
       providers: [
           AemPageDataResolver,
           {
           provide: RouteReuseStrategy,
           useClass: AemPageRouteReuseStrategy
           }
       ]
   })
   export class AppRoutingModule {}
   ```

   L&#39; `routes: Routes = [];` array definisce i percorsi o i percorsi di navigazione per le mappature dei componenti Angular.

   `AemPageMatcher` è un router Angular personalizzato [UrlMatcher](https://angular.io/api/router/UrlMatcher), che corrisponde a qualsiasi cosa &quot;sembra&quot; una pagina in AEM che fa parte di questa applicazione Angular.

   `PageComponent` è il componente angolare che rappresenta una pagina in AEM e verranno richiamate le route corrispondenti. Il `PageComponent` controllo sarà ulteriormente effettuato.

   `AemPageDataResolver`, fornito dall’SDK JS AEM SPA Editor, è un Risolutore [router](https://angular.io/api/router/Resolve) angolare personalizzato utilizzato per trasformare l’URL di route, che è il percorso in AEM inclusa l’estensione .html, nel percorso della risorsa in AEM, che è il percorso della pagina meno l’estensione.

   Ad esempio, `AemPageDataResolver` trasforma l&#39;URL di una route `content/wknd-spa-angular/us/en/home.html` in un percorso di `/content/wknd-spa-angular/us/en/home`. Viene utilizzato per risolvere il contenuto della pagina in base al percorso nell&#39;API del modello JSON.

   `AemPageRouteReuseStrategy`, fornito dall’SDK JS di AEM SPA Editor, è un [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) personalizzato che impedisce il riutilizzo dei `PageComponent` diversi route. In caso contrario, il contenuto della pagina &quot;A&quot; potrebbe venire visualizzato quando si passa alla pagina &quot;B&quot;.

2. Open the file `page.component.ts` at `ui.frontend/src/app/components/page/`.

   ```js
   ...
   export class PageComponent {
       items;
       itemsOrder;
       path;
   
       constructor(
           private route: ActivatedRoute,
           private modelManagerService: ModelManagerService
       ) {
           this.modelManagerService
           .getData({ path: this.route.snapshot.data.path })
           .then(data => {
               this.path = data[Constants.PATH_PROP];
               this.items = data[Constants.ITEMS_PROP];
               this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
           });
       }
   }
   ```

   Il `PageComponent` file JSON è necessario per elaborare il JSON recuperato da AEM e viene utilizzato come componente Angular per eseguire il rendering dei percorsi.

   `ActivatedRoute`, fornito dal modulo Router angolare, contiene lo stato che indica quale contenuto JSON di AEM pagina deve essere caricato in questa istanza del componente Pagina angolare.

   `ModelManagerService`, ottiene i dati JSON in base al percorso e li mappa sulle variabili di classe `path`, `items`, `itemsOrder`. Questi verranno quindi passati ad [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Open the file `page.component.html` at `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` include [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). Le variabili `path`, `items`, e `itemsOrder` vengono trasmesse al `AEMPageComponent`. L’SDK JavaScript `AemPageComponent`, fornito tramite SPA Editor, eseguirà quindi un’iterazione su tali dati e creerà in modo dinamico un’istanza di componenti Angular basati sui dati JSON come mostrato nell’esercitazione [](./map-components.md)Mappa componenti.

   Il `PageComponent` è davvero solo un proxy per il `AEMPageComponent` ed è il `AEMPageComponent` che fa la maggior parte del sollevamento pesante per mappare correttamente il modello JSON ai componenti Angular.

##  Inspect il routing SPA in AEM

1. Aprire un terminale e arrestare il server **di sviluppo** webpack, se avviato. Andate alla directory principale del progetto e distribuite il progetto per AEM utilizzando le vostre abilità di Paradiso:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Il progetto Angular ha alcune regole di lint molto rigide abilitate. Se la build Maven non riesce, controllate l&#39;errore e cercate gli errori **Lint trovati nei file elencati.**. Correggete eventuali problemi riscontrati dalla linter ed eseguite nuovamente il comando Maven.

2. Andate alla pagina iniziale SPA in AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) e aprire gli strumenti di sviluppo del browser. Le schermate riportate di seguito sono acquisite dal browser Google Chrome.

   Aggiorna la pagina e dovresti vedere una richiesta XHR a `/content/wknd-spa-angular/us/en.model.json`, che è la directory principale dell&#39;SPA. Si noti che solo tre pagine figlie sono incluse in base alla configurazione della profondità gerarchica del modello SPA Root creata in precedenza nell&#39;esercitazione. Questa operazione non include la **pagina 3**.

   ![Richiesta JSON iniziale - SPA Root](assets/navigation-routing/initial-json-request.png)

3. Con gli strumenti di sviluppo aperti, andate a **pagina 3**:

   ![Navigazione pagina 3](assets/navigation-routing/page-three-navigation.png)

   Tenere presente che una nuova richiesta XHR è eseguita per: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Pagina tre richiesta XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager è consapevole del fatto che il contenuto JSON della **pagina 3** non è disponibile e attiva automaticamente la richiesta XHR aggiuntiva.

4. Continuate a navigare nell’area SPA utilizzando i vari collegamenti di navigazione. Osservate che non vengono effettuate ulteriori richieste XHR e che non si verificano aggiornamenti di pagina completi. Questo rende l&#39;SPA veloce per l&#39;utente finale e riduce le richieste non necessarie al AEM.

   ![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

5. Sperimenta con collegamenti profondi navigando direttamente a: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Tenere presente che il pulsante Indietro del browser continua a funzionare.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato in che modo è possibile supportare più viste nell’SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica è stata implementata utilizzando l&#39;indirizzamento angolare e aggiunta al `Header` componente.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o estrarre il codice localmente passando al ramo `Angular/navigation-routing-solution`.

### Passaggi successivi {#next-steps}

[Creazione di un componente](custom-component.md) personalizzato - Scoprite come creare un componente personalizzato da utilizzare con AEM SPA Editor. Scoprite come sviluppare finestre di dialogo degli autori e modelli Sling per estendere il modello JSON e compilare un componente personalizzato.
