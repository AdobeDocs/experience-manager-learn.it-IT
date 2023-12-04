---
title: Aggiungi navigazione e indirizzamento | Guida introduttiva dell'Angular e dell'editor SPA dell'AEM
description: Scopri come sono supportate più visualizzazioni nell’SPA utilizzando le pagine AEM e l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando i percorsi Angular e aggiunta a un componente Intestazione esistente.
feature: SPA Editor
version: Cloud Service
jira: KT-5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 197a0c1f-4d0a-4b99-ba89-cdff2e6ac4ec
duration: 936
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2531'
ht-degree: 0%

---

# Aggiungi navigazione e indirizzamento {#navigation-routing}

Scopri come sono supportate più visualizzazioni nell’SPA utilizzando le pagine AEM e l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando i percorsi Angular e aggiunta a un componente Intestazione esistente.

## Obiettivo

1. Comprendi le opzioni di indirizzamento del modello SPA disponibili quando utilizzi l’Editor SPA.
2. Scopri come utilizzare [Angular di routing](https://angular.io/guide/router) per navigare tra le diverse visioni del SPA.
3. Implementa una navigazione dinamica guidata dalla gerarchia di pagine AEM.

## Cosa verrà creato

Questo capitolo aggiunge un menu di navigazione a un `Header` componente. Il menu di navigazione è guidato dalla gerarchia di pagine dell’AEM e utilizza il modello JSON fornito da [Componente core Navigazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

## Prerequisiti

Esaminare gli strumenti e le istruzioni necessari per l&#39;impostazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Distribuisci la base di codice in un’istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se si utilizza [AEM 6.x](overview.md#compatibility) aggiungi `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installare il pacchetto finito per il tradizionale [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite da [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) sono riutilizzati nel WKND SPA. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Gestione pacchetti installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) oppure controllare il codice localmente passando alla filiale `Angular/navigation-routing-solution`.

## Aggiornamenti Inspect HeaderComponent {#inspect-header}

Nei capitoli precedenti, il `HeaderComponent` il componente è stato aggiunto come componente di puro Angular incluso tramite `app.component.html`. In questo capitolo, la `HeaderComponent` viene rimosso dall&#39;app e aggiunto tramite il [Editor modelli](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=it). Questo consente agli utenti di configurare il menu di navigazione del `HeaderComponent` dall&#39;AEM.

>[!NOTE]
>
> Per iniziare questo capitolo, sono già stati apportati diversi aggiornamenti CSS e JavaScript alla base di codice. Concentrarsi sui concetti di base e non **tutto** delle modifiche apportate al codice. Puoi visualizzare le modifiche complete [qui](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. Nell’IDE che preferisci aprire il progetto iniziale SPA per questo capitolo.
2. Sotto `ui.frontend` il modulo ispeziona il file `header.component.ts` a: `ui.frontend/src/app/components/header/header.component.ts`.

   Sono stati apportati diversi aggiornamenti, tra cui l’aggiunta di un’ `HeaderEditConfig` e un `MapTo` per consentire la mappatura del componente su un componente AEM `wknd-spa-angular/components/header`.

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

   Osserva `@Input()` annotazione per `items`. `items` conterrà un array di oggetti di navigazione trasmessi dall’AEM.

3. In `ui.apps` verificare la definizione dei componenti dell’AEM `Header` componente: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   L&#39;AEM `Header` erediterà tutte le funzionalità del [Componente core Navigazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) tramite `sling:resourceSuperType` proprietà.

## Aggiungere il componente Header al modello SPA {#add-header-template}

1. Apri un browser e accedi all’AEM, [http://localhost:4502/](http://localhost:4502/). La base di codice iniziale deve essere già distribuita.
2. Accedi a **[!UICONTROL Modello pagina SPA]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Seleziona il più esterno **[!UICONTROL Contenitore di layout principale]** e fai clic sul relativo **[!UICONTROL Policy]** icona. Stai attento **non** per selezionare **[!UICONTROL Contenitore di layout]** non bloccato per l’authoring.

   ![Seleziona l’icona dei criteri del contenitore di layout principale](assets/navigation-routing/root-layout-container-policy.png)

4. Copia il criterio corrente e crea un nuovo criterio denominato **[!UICONTROL Struttura dell’SPA]**:

   ![Politica strutturale dell&#39;SPA](assets/map-components/spa-policy-update.png)

   Sotto **[!UICONTROL Componenti consentiti]** > **[!UICONTROL Generale]** > seleziona la **[!UICONTROL Contenitore di layout]** componente.

   Sotto **[!UICONTROL Componenti consentiti]** > **[!UICONTROL WKND SPA ANGULAR - STRUTTURA]** > seleziona la **[!UICONTROL Intestazione]** componente:

   ![Seleziona componente intestazione](assets/map-components/select-header-component.png)

   Sotto **[!UICONTROL Componenti consentiti]** > **[!UICONTROL Angular WKND SPA - Contenuto]** > seleziona la **[!UICONTROL Immagine]** e **[!UICONTROL Testo]** componenti. Dovresti aver selezionato 4 componenti totali.

   Per salvare le modifiche, fai clic su **[!UICONTROL Completati]**.

5. **Aggiorna** la pagina. Aggiungi il **[!UICONTROL Intestazione]** componente sopra lo sbloccato **[!UICONTROL Contenitore di layout]**:

   ![aggiungi componente Intestazione al modello](./assets/navigation-routing/add-header-component.gif)

6. Seleziona la **[!UICONTROL Intestazione]** e fare clic sul relativo **Policy** per modificare il criterio.

   ![Fai clic su Criterio intestazione](assets/navigation-routing/header-policy-icon.png)

7. Creare un nuovo criterio con un **[!UICONTROL Titolo criterio]** di **&quot;WKND SPA Header&quot;**.

   Sotto **[!UICONTROL Proprietà]**:

   * Imposta il **[!UICONTROL Directory principale di navigazione]** a `/content/wknd-spa-angular/us/en`.
   * Imposta il **[!UICONTROL Escludi livelli di navigazione principali]** a **1**.
   * Deseleziona **[!UICONTROL Raccogli tutte le pagine figlie]**.
   * Imposta il **[!UICONTROL Annidamento struttura di navigazione]** a **3**.

   ![Configura criterio intestazione](assets/navigation-routing/header-policy.png)

   Questo raccoglierà la navigazione 2 livelli più in basso `/content/wknd-spa-angular/us/en`.

8. Dopo aver salvato le modifiche, dovresti visualizzare il `Header` come parte del modello:

   ![Componente intestazione compilata](assets/navigation-routing/populated-header.png)

## Creare pagine figlie

Quindi, crea ulteriori pagine in AEM che fungeranno da diverse visualizzazioni nell&#39;SPA. Esamineremo anche la struttura gerarchica del modello JSON fornito dall’AEM.

1. Accedi a **Sites** console: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Seleziona la **Home page dell’Angular WKND SPA** e fai clic su **[!UICONTROL Crea]** > **[!UICONTROL Pagina]**:

   ![Crea nuova pagina](assets/navigation-routing/create-new-page.png)

2. Sotto **[!UICONTROL Modello]** seleziona **[!UICONTROL Pagina SPA]**. Sotto **[!UICONTROL Proprietà]** Invio **&quot;Pagina 1&quot;** per **[!UICONTROL Titolo]** e **&quot;page-1&quot;** come nome.

   ![Immetti le proprietà della pagina iniziale](assets/navigation-routing/initial-page-properties.png)

   Clic **[!UICONTROL Crea]** e nella finestra a comparsa, fai clic su **[!UICONTROL Apri]** per aprire la pagina nell’Editor SPA dell’AEM.

3. Aggiungi un nuovo **[!UICONTROL Testo]** Componente principale **[!UICONTROL Contenitore di layout]**. Modifica il componente e immetti il testo: **&quot;Pagina 1&quot;** utilizzando l’editor Rich Text e **H1** (per modificare gli elementi paragrafo è necessario passare alla modalità a schermo intero)

   ![Contenuto di esempio pagina 1](assets/navigation-routing/page-1-sample-content.png)

   Puoi aggiungere altri contenuti, come un’immagine.

4. Torna alla console AEM Sites e ripeti i passaggi precedenti, creando una seconda pagina denominata **&quot;Pagina 2&quot;** come pari livello di **Pagina 1**. Aggiungi contenuto a **Pagina 2** in modo da essere facilmente identificabile.
5. Infine, crea una terza pagina. **&quot;Pagina 3&quot;** ma come **secondario** di **Pagina 2**. Una volta completato, la gerarchia del sito avrà l’aspetto seguente:

   ![Gerarchia siti di esempio](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. In una nuova scheda, apri l’API del modello JSON fornita dall’AEM: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Questo contenuto JSON viene richiesto al primo caricamento dell’SPA. La struttura esterna si presenta come segue:

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

   Sotto `:children` dovresti visualizzare una voce per ciascuna delle pagine create. Il contenuto di tutte le pagine si trova in questa richiesta JSON iniziale. Una volta implementato il routing di navigazione, le visualizzazioni successive dell’SPA vengono caricate rapidamente, poiché il contenuto è già disponibile lato client.

   Non è saggio caricare **TUTTI** del contenuto di un SPA nella richiesta JSON iniziale, in quanto ciò rallenterebbe il caricamento della pagina iniziale. Quindi, vediamo come viene raccolta la profondità gerarchica delle pagine.

7. Accedi a **Radice SPA** modello in: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Fai clic su **[!UICONTROL Menu delle proprietà della pagina]** > **[!UICONTROL Criterio pagina]**:

   ![Apri i criteri di pagina per la radice SPA](assets/navigation-routing/open-page-policy.png)

8. Il **Radice SPA** il modello ha un valore **[!UICONTROL Struttura gerarchica]** per controllare il contenuto JSON raccolto. Il **[!UICONTROL Annidamento struttura]** determina la profondità nella gerarchia del sito per raccogliere le pagine figlie sotto il **radice**. È inoltre possibile utilizzare **[!UICONTROL Modelli struttura]** per filtrare ulteriori pagine in base a un’espressione regolare.

   Aggiornare il **[!UICONTROL Annidamento struttura]** a **2&quot;**:

   ![Aggiorna profondità struttura](assets/navigation-routing/update-structure-depth.png)

   Clic **[!UICONTROL Fine]** per salvare le modifiche apportate al criterio.

9. Riapri il modello JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

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

   Tieni presente che **Pagina 3** percorso rimosso: `/content/wknd-spa-angular/us/en/home/page-2/page-3` dal modello JSON iniziale.

   In seguito osserveremo come l’SDK dell’Editor SPA dell’AEM può caricare in modo dinamico contenuti aggiuntivi.

## Implementare la navigazione

Quindi, implementa il menu di navigazione con una nuova `NavigationComponent`. Potremmo aggiungere il codice direttamente in `header.component.html` ma una procedura migliore consiste nell’evitare i componenti di grandi dimensioni. Piuttosto, implementa una `NavigationComponent` che potrebbero essere riutilizzate in un secondo momento.

1. Rivedere il JSON esposto dall’AEM `Header` componente in [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   La natura gerarchica delle pagine AEM è modellata nel JSON e può essere utilizzata per popolare un menu di navigazione. Ricorda che `Header` eredita tutte le funzionalità del componente [Componente core Navigazione](https://www.aemcomponents.dev/content/core-components-examples/library/core-structure/navigation.html) e il contenuto esposto tramite JSON viene mappato automaticamente sull’Angular `@Input` annotazione.

2. Apri una nuova finestra del terminale e passa alla `ui.frontend` cartella del progetto SPA. Crea un nuovo `NavigationComponent` utilizzando lo strumento CLI Angular:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Creare quindi una classe denominata `NavigationLink` utilizzo di Angular CLI nella nuova `components/navigation` directory:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Torna all’IDE che preferisci e apri il file in `navigation-link.ts` a `/src/app/components/navigation/navigation-link.ts`.

   ![Apri il file navigation-link.ts](assets/navigation-routing/ide-navigation-link-file.png)

5. Popolare `navigation-link.ts` con le seguenti caratteristiche:

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

   Si tratta di una classe semplice che rappresenta un singolo collegamento di navigazione. Nel costruttore di classe è previsto `data` essere l’oggetto JSON trasmesso dall’AEM. Questa classe viene utilizzata in entrambi `NavigationComponent` e `HeaderComponent` per popolare facilmente la struttura di navigazione.

   Non viene eseguita alcuna trasformazione dei dati, questa classe viene creata principalmente per digitare fortemente il modello JSON. Tieni presente che `this.children` è digitato come `NavigationLink[]` e che il costruttore crea in modo ricorsivo `NavigationLink` oggetti per ciascuno degli elementi nel `children` array. Ricorda quel modello JSON per `Header` è gerarchico.

6. Apri il file `navigation-link.spec.ts`. Questo è il file di test per `NavigationLink` classe. Aggiornalo con quanto segue:

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

   Tieni presente che `const data` segue lo stesso modello JSON ispezionato in precedenza per un singolo collegamento. Questo è lungi dall&#39;essere un solido unit test, tuttavia dovrebbe essere sufficiente per testare il costruttore di `NavigationLink`.

7. Apri il file `navigation.component.ts`. Aggiornalo con quanto segue:

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

   `NavigationComponent` prevede un `object[]` denominato `items` questo è il modello JSON dell’AEM. Questa classe espone un singolo metodo `get navigationLinks()` che restituisce un array di `NavigationLink` oggetti.

8. Apri il file `navigation.component.html` e aggiornarlo con quanto segue:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Viene generata una `<ul>` e chiama `get navigationLinks()` metodo da `navigation.component.ts`. Un `<ng-container>` viene utilizzato per effettuare una chiamata a un modello denominato `recursiveListTmpl` e la trasmette `navigationLinks` come variabile denominata `links`.

   Aggiungi il `recursiveListTmpl` avanti:

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

   Qui viene implementato il resto del rendering per il collegamento di navigazione. La variabile `link` è di tipo `NavigationLink` e tutti i metodi/proprietà creati da tale classe sono disponibili. [`[routerLink]`](https://angular.io/api/router/RouterLink) viene utilizzato al posto del normale `href` attributo. Questo ci permette di collegarci a specifici percorsi nell’app, senza un aggiornamento a pagina intera.

   La parte ricorsiva della navigazione viene implementata anche creando un’altra `<ul>` se l&#39;attuale `link` ha un valore non vuoto `children` array.

9. Aggiorna `navigation.component.spec.ts` per aggiungere supporto per `RouterTestingModule`:

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

   Aggiunta di `RouterTestingModule` è richiesto perché il componente utilizza `[routerLink]`.

10. Aggiorna `navigation.component.scss` per aggiungere alcuni stili di base al `NavigationComponent`:

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

## Aggiornare il componente intestazione

Ora che il `NavigationComponent` è stato implementato, il `HeaderComponent` deve essere aggiornato per farvi riferimento.

1. Apri un terminale e passa a `ui.frontend` all&#39;interno del progetto SPA. Avvia il **server di sviluppo webpack**:

   ```shell
   $ npm start
   ```

2. Apri una scheda del browser e passa a [http://localhost:4200/](http://localhost:4200/).

   Il **server di sviluppo webpack** deve essere configurato per fungere da proxy del modello JSON da un&#39;istanza locale di AEM (`ui.frontend/proxy.conf.json`). Questo ci consentirà di codificare direttamente in base al contenuto creato in AEM dall’esercitazione precedente.

   ![attivazione/disattivazione menu](./assets/navigation-routing/nav-toggle-static.gif)

   Il `HeaderComponent` al momento la funzionalità di attivazione/disattivazione menu è già implementata. Quindi, aggiungi il componente Navigazione.

3. Torna all’IDE desiderato e apri il file `header.component.ts` a `ui.frontend/src/app/components/header/header.component.ts`.
4. Aggiornare il `setHomePage()` metodo per rimuovere l’elemento String hardcoded e utilizzare le proprietà dinamiche passate dal componente AEM:

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

   Una nuova istanza di `NavigationLink` viene creato in base a `items[0]`, radice del modello JSON di navigazione trasmesso dall’AEM. `this.route.snapshot.data.path` restituisce il percorso della route di Angular corrente. Questo valore viene utilizzato per determinare se la route corrente è **Home page**. `this.homePageUrl` viene utilizzato per popolare il collegamento di ancoraggio sulla **logo**.

5. Apri `header.component.html` e sostituisci il segnaposto statico per la navigazione con un riferimento al nuovo `NavigationComponent`:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` l&#39;attributo supera `@Input() items` dal `HeaderComponent` al `NavigationComponent` dove verrà compilata la navigazione.

6. Apri `header.component.spec.ts` e aggiungi una dichiarazione per `NavigationComponent`:

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

   Dal momento che `NavigationComponent` viene ora utilizzato come parte del `HeaderComponent` deve essere dichiarato come parte del banco di prova.

7. Salva le modifiche apportate ai file aperti e torna a **server di sviluppo webpack**: [http://localhost:4200/](http://localhost:4200/)

   ![Navigazione intestazione completata](assets/navigation-routing/completed-header.png)

   Apri la navigazione facendo clic sull’interruttore del menu; dovresti visualizzare i collegamenti di navigazione compilati. Dovresti essere in grado di passare a diverse visualizzazioni dell’SPA.

## Informazioni sul routing dell’SPA

Ora che la navigazione è stata implementata, ispeziona il routing in AEM.

1. Nell’IDE apri il file `app-routing.module.ts` a `ui.frontend/src/app`.

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

   Il `routes: Routes = [];` array definisce le route o i percorsi di navigazione per le mappature dei componenti Angular.

   `AemPageMatcher` è un router di Angular personalizzato [UrlMatcher](https://angular.io/api/router/UrlMatcher): corrisponde a qualsiasi elemento che &quot;assomiglia&quot; a una pagina nell’AEM che fa parte di questa applicazione di Angular.

   `PageComponent` è il componente Angular che rappresenta una pagina nell’AEM e viene utilizzato per eseguire il rendering dei percorsi corrispondenti. Il `PageComponent` viene rivisto più avanti nell’esercitazione.

   `AemPageDataResolver`, fornito dall&#39;SDK JS dell&#39;editor SPA dell&#39;AEM, è un [Angular Router Resolver](https://angular.io/api/router/Resolve) utilizzato per trasformare l’URL del percorso, che è il percorso in AEM inclusa l’estensione.html, nel percorso della risorsa in AEM, che è il percorso della pagina meno l’estensione.

   Ad esempio, il `AemPageDataResolver` trasforma l’URL di una route in `content/wknd-spa-angular/us/en/home.html` in un percorso di `/content/wknd-spa-angular/us/en/home`. Viene utilizzato per risolvere il contenuto della pagina in base al percorso nell’API del modello JSON.

   `AemPageRouteReuseStrategy`, fornito dall&#39;SDK JS dell&#39;editor SPA dell&#39;AEM, è un [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) che impedisce il riutilizzo del `PageComponent` tra percorsi. In caso contrario, il contenuto della pagina &quot;A&quot; potrebbe apparire quando si passa alla pagina &quot;B&quot;.

2. Apri il file `page.component.ts` a `ui.frontend/src/app/components/page/`.

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

   Il `PageComponent` è necessario per elaborare il JSON recuperato dall’AEM e viene utilizzato come componente di Angular per eseguire il rendering delle route.

   `ActivatedRoute`, fornito dal modulo Router Angular, contiene lo stato che indica quale contenuto JSON della pagina AEM deve essere caricato nell’istanza del componente Pagina Angular.

   `ModelManagerService`, ottiene i dati JSON in base alla route e mappa i dati sulle variabili di classe `path`, `items`, `itemsOrder`. Questi verranno quindi passati al [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Apri il file `page.component.html` a `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` include [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). Le variabili `path`, `items`, e `itemsOrder` vengono passati al `AEMPageComponent`. Il `AemPageComponent`, fornito tramite l’SDK JavaScript dell’editor SPA, esegue quindi l’iterazione di questi dati e crea un’istanza dinamica dei componenti Angular in base ai dati JSON come mostrato nella [Tutorial sui componenti mappa](./map-components.md).

   Il `PageComponent` è solo un proxy per `AEMPageComponent` ed è il `AEMPageComponent` che esegue la maggior parte del sollevamento pesante per mappare correttamente il modello JSON ai componenti di Angular.

## Inspect il routing dell’SPA nell’AEM

1. Aprire un terminale e arrestare **server di sviluppo webpack** se avviato. Passa alla directory principale del progetto e implementa il progetto in AEM utilizzando le abilità Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Nel progetto di Angular sono abilitate alcune regole di evidenziazione molto rigide. Se la build Maven non riesce, controlla l’errore e cerca **Sono stati rilevati errori di evidenziazione nei file elencati.**. Correggi eventuali problemi rilevati dal puntatore ed esegui nuovamente il comando Maven.

2. Passa alla home page dell’SPA nell’AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) e apri gli strumenti per sviluppatori del browser. Le schermate seguenti vengono acquisite dal browser Google Chrome.

   Aggiorna la pagina per visualizzare una richiesta XHR a `/content/wknd-spa-angular/us/en.model.json`, che è la radice dell&#39;SPA. Tieni presente che solo tre pagine figlie sono incluse in base alla configurazione della profondità della gerarchia rispetto al modello radice SPA creato in precedenza nell’esercitazione. Questo non include **Pagina 3**.

   ![Richiesta JSON iniziale - Radice SPA](assets/navigation-routing/initial-json-request.png)

3. Con gli strumenti per sviluppatori aperti, passa a **Pagina 3**:

   ![Pagina 3 Navigazione](assets/navigation-routing/page-three-navigation.png)

   Osserva che viene effettuata una nuova richiesta XHR per: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Pagina tre richiesta XHR](assets/navigation-routing/page-3-xhr-request.png)

   Il Model Manager dell&#39;AEM è consapevole che **Pagina 3** Il contenuto JSON non è disponibile e attiva automaticamente la richiesta XHR aggiuntiva.

4. Continuare a navigare nel SPA utilizzando i vari collegamenti di navigazione. Tieni presente che non vengono effettuate richieste XHR aggiuntive e che non si verifica alcun aggiornamento dell’intera pagina. Questo rende l’SPA veloce per l’utente finale e riduce le richieste non necessarie di nuovo all’AEM.

   ![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

5. Prova i collegamenti profondi navigando direttamente in: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Il pulsante Indietro del browser continua a funzionare.

## Congratulazioni. {#congratulations}

Congratulazioni, hai imparato come supportare più visualizzazioni nell’SPA mappando le pagine AEM con l’SDK dell’editor SPA. La navigazione dinamica è stata implementata utilizzando il routing Angular ed è stata aggiunta al `Header` componente.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) oppure controllare il codice localmente passando alla filiale `Angular/navigation-routing-solution`.

### Passaggi successivi {#next-steps}

[Creare un componente personalizzato](custom-component.md) - Scopri come creare un componente personalizzato da utilizzare con l’Editor SPA dell’AEM. Scopri come sviluppare finestre di dialogo di authoring e modelli Sling per estendere il modello JSON e popolare un componente personalizzato.
