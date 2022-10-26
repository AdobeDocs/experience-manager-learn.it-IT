---
title: Aggiungi navigazione e indirizzamento | Guida introduttiva all’editor di SPA AEM e all’Angular
description: Scopri come sono supportate più visualizzazioni nell’SPA utilizzando AEM pagine e l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando percorsi di Angular e aggiunta a un componente Header esistente.
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 197a0c1f-4d0a-4b99-ba89-cdff2e6ac4ec
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2712'
ht-degree: 1%

---

# Aggiungi navigazione e indirizzamento {#navigation-routing}

Scopri come sono supportate più visualizzazioni nell’SPA utilizzando AEM pagine e l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando percorsi di Angular e aggiunta a un componente Header esistente.

## Obiettivo

1. Comprendere le opzioni di indirizzamento del modello SPA disponibili quando si utilizza l&#39;editor SPA.
2. Scopri come utilizzare [routing di Angular](https://angular.io/guide/router) per spostarsi tra le diverse viste della SPA.
3. Implementa una navigazione dinamica guidata dalla gerarchia di pagine AEM.

## Cosa verrà creato

Questo capitolo aggiunge un menu di navigazione a un `Header` componente. Il menu di navigazione è guidato dalla gerarchia di pagine AEM e utilizza il modello JSON fornito dalla [Componente core di navigazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

## Prerequisiti

Rivedere gli strumenti e le istruzioni necessari per la configurazione di un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Distribuisci la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi le `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installa il pacchetto finito per il tradizionale [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite da [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) vengono riutilizzati nel SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o controlla il codice localmente passando al ramo `Angular/navigation-routing-solution`.

## Aggiornamenti di Inspect HeaderComponent {#inspect-header}

Nei capitoli precedenti, il `HeaderComponent` è stato aggiunto come componente di Angular puro incluso tramite `app.component.html`. In questo capitolo, il `HeaderComponent` il componente viene rimosso dall’app e aggiunto tramite l’ [Editor modelli](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=it). Questo consente agli utenti di configurare il menu di navigazione del `HeaderComponent` da AEM.

>[!NOTE]
>
> Diversi aggiornamenti CSS e JavaScript sono già stati apportati alla base di codice per avviare questo capitolo. Concentrarsi sui concetti principali, non **tutto** delle modifiche al codice sono discussi. Puoi visualizzare tutte le modifiche [qui](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. Nell’IDE che preferisci, apri il progetto iniziale SPA per questo capitolo.
2. Sotto `ui.frontend` il modulo esamina il file `header.component.ts` a: `ui.frontend/src/app/components/header/header.component.ts`.

   Sono stati effettuati diversi aggiornamenti, tra cui l&#39;aggiunta di un `HeaderEditConfig` e `MapTo` per consentire la mappatura del componente su un componente AEM `wknd-spa-angular/components/header`.

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

   Tieni presente che `@Input()` annotazione per `items`. `items` conterrà una matrice di oggetti di navigazione passati da AEM.

3. In `ui.apps` il modulo esamina la definizione del componente del AEM `Header` componente: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   AEM `Header` erediterà tutte le funzionalità del [Componente core di navigazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) tramite `sling:resourceSuperType` proprietà.

## Aggiungi il componente Header al modello SPA {#add-header-template}

1. Apri un browser e accedi a AEM, [http://localhost:4502/](Http://localhost:4502/). La base di codice iniziale deve essere già distribuita.
2. Passa a **[!UICONTROL Modello di pagina SPA]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Seleziona il più esterno **[!UICONTROL Contenitore di layout principale]** e fai clic sui relativi **[!UICONTROL Criterio]** icona. Fai attenzione **not** per selezionare **[!UICONTROL Contenitore di layout]** sbloccato per l&#39;authoring.

   ![Seleziona l’icona del criterio del contenitore di layout principale](assets/navigation-routing/root-layout-container-policy.png)

4. Copia il criterio corrente e crea un nuovo criterio denominato **[!UICONTROL Struttura SPA]**:

   ![Criteri per la struttura SPA](assets/map-components/spa-policy-update.png)

   Sotto **[!UICONTROL Componenti consentiti]** > **[!UICONTROL Generale]** > seleziona **[!UICONTROL Contenitore di layout]** componente.

   Sotto **[!UICONTROL Componenti consentiti]** > **[!UICONTROL Angular WKND SPA - STRUTTURA]** > seleziona **[!UICONTROL Intestazione]** componente:

   ![Seleziona il componente intestazione](assets/map-components/select-header-component.png)

   Sotto **[!UICONTROL Componenti consentiti]** > **[!UICONTROL Angular SPA WKND - Contenuto]** > seleziona **[!UICONTROL Immagine]** e **[!UICONTROL Testo]** componenti. Dovresti aver selezionato 4 componenti totali.

   Fai clic su **[!UICONTROL Fine]** per salvare le modifiche.

5. **Aggiorna la pagina.** Aggiungi il **[!UICONTROL Intestazione]** sopra il componente sbloccato **[!UICONTROL Contenitore di layout]**:

   ![aggiungi componente Intestazione al modello](./assets/navigation-routing/add-header-component.gif)

6. Seleziona la **[!UICONTROL Intestazione]** e fai clic sul relativo componente **Criterio** per modificare il criterio.

   ![Fai clic su Criterio intestazione](assets/navigation-routing/header-policy-icon.png)

7. Crea un nuovo criterio con un **[!UICONTROL Titolo criterio]** di **&quot;Intestazione SPA WKND&quot;**.

   Sotto la **[!UICONTROL Proprietà]**:

   * Imposta la **[!UICONTROL Radice di navigazione]** a `/content/wknd-spa-angular/us/en`.
   * Imposta la **[!UICONTROL Escludere i livelli della radice]** a **1**.
   * Deseleziona **[!UICONTROL Raccogliere tutte le pagine figlie]**.
   * Imposta la **[!UICONTROL Profondità struttura di navigazione]** a **3**.

   ![Configurare i criteri di intestazione](assets/navigation-routing/header-policy.png)

   Questo raccoglierà i 2 livelli di navigazione in fondo `/content/wknd-spa-angular/us/en`.

8. Dopo aver salvato le modifiche, dovresti vedere il popolato `Header` come parte del modello:

   ![Componente intestazione popolata](assets/navigation-routing/populated-header.png)

## Creare pagine figlie

Quindi, crea altre pagine in AEM che fungeranno da viste diverse nel SPA. Esamineremo anche la struttura gerarchica del modello JSON fornito da AEM.

1. Passa a **Sites** console: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Seleziona la **Home page Angular WKND SPA** e fai clic su **[!UICONTROL Crea]** > **[!UICONTROL Pagina]**:

   ![Crea nuova pagina](assets/navigation-routing/create-new-page.png)

2. Sotto **[!UICONTROL Modello]** select **[!UICONTROL Pagina SPA]**. Sotto **[!UICONTROL Proprietà]** enter **&quot;Pagina 1&quot;** per **[!UICONTROL Titolo]** e **&quot;page-1&quot;** come nome.

   ![Immetti le proprietà della pagina iniziale](assets/navigation-routing/initial-page-properties.png)

   Fai clic su **[!UICONTROL Crea]** e nella finestra di dialogo a comparsa, fai clic su **[!UICONTROL Apri]** per aprire la pagina nell’editor di SPA AEM.

3. Aggiungi un nuovo **[!UICONTROL Testo]** componente del **[!UICONTROL Contenitore di layout]**. Modifica il componente e immetti il testo: **&quot;Pagina 1&quot;** utilizzando l’editor Rich Text e **H1** (per modificare gli elementi paragrafo dovrai passare alla modalità a tutto schermo)

   ![Contenuto di esempio pagina 1](assets/navigation-routing/page-1-sample-content.png)

   Puoi aggiungere altri contenuti, come un’immagine.

4. Torna alla console AEM Sites e ripeti i passaggi precedenti, creando una seconda pagina denominata **&quot;Pagina 2&quot;** come fratelli di **Pagina 1**. Aggiungi contenuto a **Pagina 2** in modo che sia facilmente identificato.
5. Infine creare una terza pagina, **&quot;Pagina 3&quot;** ma **bambino** di **Pagina 2**. Una volta completata la gerarchia del sito, avrà un aspetto simile al seguente:

   ![Gerarchia del sito di esempio](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. In una nuova scheda, apri l’API del modello JSON fornita da AEM: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Questo contenuto JSON viene richiesto quando il SPA viene caricato per la prima volta. La struttura esterna si presenta come segue:

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

   Sotto `:children` dovrebbe essere visualizzata una voce per ciascuna delle pagine create. Il contenuto per tutte le pagine si trova in questa richiesta JSON iniziale. Una volta implementato il ciclo di navigazione, le visualizzazioni successive del SPA vengono caricate rapidamente, poiché il contenuto è già disponibile sul lato client.

   Non è saggio caricare **TUTTO** del contenuto di un SPA nella richiesta JSON iniziale, in quanto ciò rallenterebbe il caricamento della pagina iniziale. Ora, esaminiamo come viene raccolta la profondità irarica delle pagine.

7. Passa a **Radice SPA** modello in: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Fai clic sul pulsante **[!UICONTROL Menu delle proprietà della pagina]** > **[!UICONTROL Criterio pagina]**:

   ![Apri i criteri di pagina per SPA radice](assets/navigation-routing/open-page-policy.png)

8. La **Radice SPA** modello ha un **[!UICONTROL Struttura gerarchica]** per controllare il contenuto JSON raccolto. La **[!UICONTROL Profondità della struttura]** determina la profondità nella gerarchia del sito per la raccolta delle pagine figlie al di sotto di **root**. È inoltre possibile utilizzare **[!UICONTROL Pattern struttura]** per filtrare pagine aggiuntive in base a un’espressione regolare.

   Aggiorna **[!UICONTROL Profondità della struttura]** a **&quot;2&quot;**:

   ![Aggiorna profondità struttura](assets/navigation-routing/update-structure-depth.png)

   Fai clic su **[!UICONTROL Fine]** per salvare le modifiche al criterio.

9. Riaprire il modello JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

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

   In seguito, osserveremo come l’SDK dell’editor di SPA AEM può caricare in modo dinamico contenuti aggiuntivi.

## Implementare la navigazione

Quindi, implementa il menu di navigazione con un nuovo `NavigationComponent`. Potremmo aggiungere il codice direttamente in `header.component.html` ma è meglio evitare componenti di grandi dimensioni. Al contrario, implementa un `NavigationComponent` che potrebbe essere riutilizzato successivamente.

1. Esamina il JSON esposto dal AEM `Header` componente a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   La natura gerarchica delle pagine AEM è modellata nel JSON che può essere utilizzato per compilare un menu di navigazione. Ricorda che la `Header` il componente eredita tutte le funzionalità del [Componente core di navigazione](https://www.aemcomponents.dev/content/core-components-examples/library/core-structure/navigation.html) e il contenuto esposto tramite JSON viene mappato automaticamente all’Angular `@Input` annotazione.

2. Apri una nuova finestra terminale e passa alla `ui.frontend` cartella del progetto SPA. Crea un nuovo `NavigationComponent` utilizzando lo strumento Angular CLI:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Creare quindi una classe denominata `NavigationLink` utilizzo di Angular CLI nella nuova creazione `components/navigation` directory:

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

   Si tratta di una classe semplice che rappresenta un singolo collegamento di navigazione. Nel costruttore di classe ci aspettiamo `data` essere l&#39;oggetto JSON passato da AEM. Questa classe viene utilizzata in entrambe le `NavigationComponent` e `HeaderComponent` per popolare facilmente la struttura di navigazione.

   Non viene eseguita alcuna trasformazione dei dati, questa classe viene creata principalmente per digitare con forza il modello JSON. Tieni presente che `this.children` viene digitato come `NavigationLink[]` e che il costruttore crea in modo ricorsivo nuove `NavigationLink` per ciascuno degli elementi nel `children` array. Richiama il modello JSON per `Header` è gerarchico.

6. Aprire il file `navigation-link.spec.ts`. Questo è il file di prova per `NavigationLink` classe. Aggiornalo con i seguenti elementi:

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

   Tieni presente che `const data` segue lo stesso modello JSON esaminato in precedenza per un singolo collegamento. Questo è lungi dall&#39;essere un solido test di unità, tuttavia dovrebbe essere sufficiente testare il costruttore di `NavigationLink`.

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

   `NavigationComponent` si aspetta `object[]` denominato `items` questo è il modello JSON di AEM. Questa classe espone un singolo metodo `get navigationLinks()` che restituisce un array di `NavigationLink` oggetti.

8. Apri il file . `navigation.component.html` e aggiornalo con quanto segue:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Viene generato un `<ul>` e chiama `get navigationLinks()` metodo da `navigation.component.ts`. Un `<ng-container>` viene utilizzato per effettuare una chiamata a un modello denominato `recursiveListTmpl` e lo trasmette `navigationLinks` come variabile denominata `links`.

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

   Qui viene implementato il resto del rendering per il collegamento di navigazione. Tieni presente che la variabile `link` è di tipo `NavigationLink` e sono disponibili tutti i metodi/proprietà creati da tale classe. [`[routerLink]`](https://angular.io/api/router/RouterLink) viene utilizzato al posto del normale `href` attributo. Questo ci consente di collegare a percorsi specifici nell’app, senza un aggiornamento a pagina intera.

   La parte ricorsiva della navigazione viene implementata anche creando un’altra `<ul>` se `link` non è vuoto `children` array.

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

## Aggiorna il componente intestazione

Ora che `NavigationComponent` è stato implementato il `HeaderComponent` deve essere aggiornato per farvi riferimento.

1. Apri un terminale e passa alla `ui.frontend` all’interno del progetto SPA. Avvia la **server di sviluppo webpack**:

   ```shell
   $ npm start
   ```

2. Apri una scheda del browser e passa a [http://localhost:4200/](Http://localhost:4200/).

   La **server di sviluppo webpack** deve essere configurato per eseguire il proxy del modello JSON da un&#39;istanza locale di AEM (`ui.frontend/proxy.conf.json`). Questo ci consentirà di eseguire il codice direttamente in base al contenuto creato in AEM dalla precedente esercitazione.

   ![funzionamento del menu](./assets/navigation-routing/nav-toggle-static.gif)

   La `HeaderComponent` al momento la funzionalità di attivazione/disattivazione menu è già implementata. Quindi, aggiungi il componente di navigazione.

3. Torna all’IDE che preferisci e apri il file . `header.component.ts` a `ui.frontend/src/app/components/header/header.component.ts`.
4. Aggiorna `setHomePage()` per rimuovere l&#39;oggetto String codificato e utilizzare le proprietà dinamiche trasmesse dal componente AEM:

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

   Una nuova istanza di `NavigationLink` viene creato in base a `items[0]`, la radice del modello JSON di navigazione passato da AEM. `this.route.snapshot.data.path` restituisce il percorso del percorso Angular corrente. Questo valore viene utilizzato per determinare se la route corrente è **Home page**. `this.homePageUrl` viene utilizzato per popolare il collegamento di ancoraggio nel **logo**.

5. Apri `header.component.html` e sostituisci il segnaposto statico per la navigazione con un riferimento al nuovo `NavigationComponent`:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` l&#39;attributo passa `@Input() items` dal `HeaderComponent` al `NavigationComponent` dove verrà effettuata la navigazione.

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

7. Salva le modifiche apportate a tutti i file aperti e torna al **server di sviluppo webpack**: [http://localhost:4200/](Http://localhost:4200/)

   ![Navigazione intestazione completata](assets/navigation-routing/completed-header.png)

   Apri la navigazione facendo clic sull’interruttore del menu e dovresti visualizzare i collegamenti di navigazione compilati. Dovresti essere in grado di passare a diverse viste della SPA.

## Comprendere il ciclo di SPA

Ora che la navigazione è stata implementata, controlla il routing in AEM.

1. Nell’IDE apri il file . `app-routing.module.ts` a `ui.frontend/src/app`.

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

   La `routes: Routes = [];` array definisce i percorsi o i percorsi di navigazione per Angular le mappature dei componenti.

   `AemPageMatcher` è un router di Angular personalizzato [UrlMatcher](https://angular.io/api/router/UrlMatcher), che corrisponde a qualsiasi cosa che assomiglia a una pagina di AEM che fa parte di questa applicazione Angular.

   `PageComponent` è il componente Angular che rappresenta una pagina in AEM e utilizzato per eseguire il rendering dei percorsi corrispondenti. La `PageComponent` viene rivisto più avanti nell’esercitazione.

   `AemPageDataResolver`, fornito dall’SDK JS dell’editor di SPA AEM, è personalizzato [Risolutore router Angular](https://angular.io/api/router/Resolve) utilizzato per trasformare l’URL del percorso, che è il percorso in AEM inclusa l’estensione .html, nel percorso della risorsa in AEM, che è il percorso della pagina meno l’estensione.

   Ad esempio, il `AemPageDataResolver` trasforma l’URL di un percorso in `content/wknd-spa-angular/us/en/home.html` in un percorso di `/content/wknd-spa-angular/us/en/home`. Viene utilizzato per risolvere il contenuto della pagina in base al percorso nell’API del modello JSON.

   `AemPageRouteReuseStrategy`, fornito dall’SDK JS dell’editor di SPA AEM, è personalizzato [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) che impediscono il riutilizzo `PageComponent` attraverso i percorsi. In caso contrario, il contenuto della pagina &quot;A&quot; potrebbe essere visualizzato quando si passa alla pagina &quot;B&quot;.

2. Apri il file . `page.component.ts` a `ui.frontend/src/app/components/page/`.

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

   La `PageComponent` è necessario per elaborare il JSON recuperato da AEM e viene utilizzato come componente Angular per eseguire il rendering dei percorsi.

   `ActivatedRoute`, fornito dal modulo Router di Angular, contiene lo stato che indica AEM contenuto JSON della pagina da caricare in questa istanza del componente Pagina di Angular.

   `ModelManagerService`, ottiene i dati JSON in base al percorso e li mappa su variabili di classe `path`, `items`, `itemsOrder`. Questi verranno quindi passati al [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Apri il file . `page.component.html` a `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` include [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). Le variabili `path`, `items`e `itemsOrder` vengono trasmessi al `AEMPageComponent`. La `AemPageComponent`, fornito tramite gli SDK JavaScript dell’editor di SPA, ripeterà questi dati e creerà dinamicamente un’istanza di componenti Angular in base ai dati JSON visti nella [Esercitazione su Mappa componenti](./map-components.md).

   La `PageComponent` è solo un proxy per `AEMPageComponent` e `AEMPageComponent` che esegue la maggior parte del sollevamento pesante per mappare correttamente il modello JSON ai componenti di Angular.

## Inspect il routing SPA in AEM

1. Aprire un terminale e arrestare il **server di sviluppo webpack** se avviato. Passa alla directory principale del progetto e distribuisci il progetto da AEM utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Nel progetto di Angular sono state attivate alcune regole di stampa molto rigide. Se la build Maven non riesce, controlla l’errore e cerca **Errori di stampa rilevati nei file elencati.**. Correggi eventuali problemi rilevati dall&#39;icona ed esegui nuovamente il comando Maven.

2. Passa alla home page SPA in AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) e apri gli strumenti per sviluppatori del browser. Le schermate seguenti vengono acquisite dal browser Google Chrome.

   Aggiorna la pagina e dovresti visualizzare una richiesta XHR a `/content/wknd-spa-angular/us/en.model.json`, che è la radice SPA. Solo tre pagine figlie sono incluse in base alla configurazione della profondità gerarchica del modello SPA Root creato in precedenza nell’esercitazione. Questo non include **Pagina 3**.

   ![Richiesta JSON iniziale - SPA radice](assets/navigation-routing/initial-json-request.png)

3. Con gli strumenti per sviluppatori aperti, accedi a **Pagina 3**:

   ![Pagina 3 Naviga](assets/navigation-routing/page-three-navigation.png)

   Tieni presente che viene effettuata una nuova richiesta XHR a: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Pagina tre richiesta XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager riconosce che la **Pagina 3** Il contenuto JSON non è disponibile e attiva automaticamente la richiesta XHR aggiuntiva.

4. Continua a navigare nel SPA utilizzando i vari collegamenti di navigazione. Osserva che non vengono effettuate richieste XHR aggiuntive e che non si verifica alcun aggiornamento completo della pagina. In questo modo l’SPA diventa veloce per l’utente finale e si riducono le richieste non necessarie a AEM.

   ![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

5. Sperimenta i collegamenti profondi navigando direttamente in: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Tieni presente che il pulsante Indietro del browser continua a funzionare.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato come è possibile supportare più visualizzazioni nel SPA mappando le pagine AEM con l’SDK dell’editor SPA. La navigazione dinamica è stata implementata utilizzando il ciclo di Angular e aggiunta al `Header` componente.

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o controlla il codice localmente passando al ramo `Angular/navigation-routing-solution`.

### Passaggi successivi {#next-steps}

[Creare un componente personalizzato](custom-component.md) - Scopri come creare un componente personalizzato da utilizzare con l’editor SPA AEM. Scopri come sviluppare le finestre di dialogo degli autori e i modelli Sling per estendere il modello JSON a un componente personalizzato.
