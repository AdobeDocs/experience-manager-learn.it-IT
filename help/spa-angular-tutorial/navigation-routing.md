---
title: Aggiunta di navigazione e routing | Guida introduttiva all'editor SPA AEM e Angular
description: Scoprite come sono supportate più viste nel SPA utilizzando AEM Pagine e l’SDK SPA Editor. La navigazione dinamica viene implementata utilizzando route angolari e aggiunta a un componente Intestazione esistente.
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

Scoprite come sono supportate più viste nel SPA utilizzando AEM Pagine e l’SDK SPA Editor. La navigazione dinamica viene implementata utilizzando route angolari e aggiunta a un componente Intestazione esistente.

## Obiettivo

1. Comprendere le opzioni di routing SPA modello disponibili quando si utilizza l&#39;editor SPA.
2. Scopri come utilizzare [Ciclo angolare](https://angular.io/guide/router) per spostarsi tra le diverse viste della SPA.
3. Implementa una navigazione dinamica guidata dalla gerarchia di pagine AEM.

## Cosa verrà creato

Questo capitolo aggiunge un menu di navigazione a un componente `Header` esistente. Il menu di navigazione è gestito dalla gerarchia di pagine AEM e utilizza il modello JSON fornito dal componente di base di navigazione [Navigazione](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html).

![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

## Prerequisiti

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

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

   Se si utilizza [AEM 6.x](overview.md#compatibility) aggiungere il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installate il pacchetto finito per il tradizionale sito di riferimento [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite dal sito di riferimento [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) verranno riutilizzate sul SPA WKND. Il pacchetto può essere installato utilizzando [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o estrarre il codice localmente passando al ramo `Angular/navigation-routing-solution`.

##  Inspect HeaderComponent aggiornamenti {#inspect-header}

Nei capitoli precedenti, il componente `HeaderComponent` veniva aggiunto come componente Angolare puro incluso tramite `app.component.html`. In questo capitolo, il componente `HeaderComponent` viene rimosso dall&#39;app e aggiunto tramite l&#39; [Editor modelli](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Questo consente agli utenti di configurare il menu di navigazione di `HeaderComponent` dall&#39;interno AEM.

>[!NOTE]
>
> Diversi aggiornamenti CSS e JavaScript sono già stati apportati alla base di codice per avviare questo capitolo. Per concentrarsi sui concetti di base, non vengono discusse tutte le **tutte** delle modifiche al codice. Potete visualizzare le modifiche complete [qui](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. Nell’IDE di vostra scelta, aprite il progetto iniziale SPA per questo capitolo.
2. Sotto il modulo `ui.frontend` ispezionare il file `header.component.ts` in: `ui.frontend/src/app/components/header/header.component.ts`.

   Sono stati eseguiti diversi aggiornamenti, tra cui l&#39;aggiunta di un `HeaderEditConfig` e di un `MapTo` per consentire la mappatura del componente su un componente AEM `wknd-spa-angular/components/header`.

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

   Notare l&#39;annotazione `@Input()` per `items`. `items` conterrà un array di oggetti di navigazione passati da AEM.

3. Nel modulo `ui.apps` ispeziona la definizione del componente AEM `Header`: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   Il componente AEM `Header` erediterà tutte le funzionalità del componente core di navigazione [a2/> tramite la proprietà `sling:resourceSuperType`.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)

## Aggiungere il componente HeaderComponent al modello SPA {#add-header-template}

1. Aprite un browser e accedete a AEM, [http://localhost:4502/](Http://localhost:4502/). La base di codice iniziale dovrebbe essere già distribuita.
2. Passare al **[!UICONTROL SPA modello di pagina]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Selezionare il contenitore di layout radice più esterno **[!UICONTROL e fare clic sull&#39;icona**[!UICONTROL  Policy ]**.]** Fare attenzione a **non** per selezionare **[!UICONTROL Contenitore di layout]** non bloccato per l&#39;authoring.

   ![Selezionate l&#39;icona del criterio contenitore del layout principale](assets/navigation-routing/root-layout-container-policy.png)

4. Copiate il criterio corrente e create un nuovo criterio denominato **[!UICONTROL Struttura SPA]**:

   ![Criteri struttura SPA](assets/map-components/spa-policy-update.png)

   In **[!UICONTROL Componenti consentiti]** > **[!UICONTROL Generale]** > selezionare il componente **[!UICONTROL Contenitore di layout]**.

   In **[!UICONTROL Componenti consentiti]** > **[!UICONTROL WKND SPA ANGOLARE - STRUTTURA]** > selezionare il componente **[!UICONTROL Intestazione]**:

   ![Seleziona componente intestazione](assets/map-components/select-header-component.png)

   In **[!UICONTROL Componenti consentiti]** > **[!UICONTROL WKND SPA ANGOLARE - Content]** > selezionare i componenti **[!UICONTROL Image]** e **[!UICONTROL Text]**. È necessario selezionare 4 componenti totali.

   Fare clic su **[!UICONTROL Fine]** per salvare le modifiche.

5. **Aggiorna la pagina.** Aggiungete il componente **[!UICONTROL Header]** sopra il contenitore di layout **[!UICONTROL sbloccato]**:

   ![aggiunta del componente Intestazione al modello](./assets/navigation-routing/add-header-component.gif)

6. Selezionare il componente **[!UICONTROL Header]** e fare clic sull&#39;icona **Policy** per modificare il criterio.

   ![Fare clic su Criterio intestazione](assets/navigation-routing/header-policy-icon.png)

7. Create un nuovo criterio con un **[!UICONTROL titolo del criterio]** di **&quot;WKND SPA Header&quot;**.

   In **[!UICONTROL Proprietà]**:

   * Impostare la **[!UICONTROL radice di navigazione]** su `/content/wknd-spa-angular/us/en`.
   * Impostare **[!UICONTROL Exclude Root Levels]** su **1**.
   * Deselezionare **[!UICONTROL Raccolta di tutte le pagine figlie]**.
   * Impostare la **[!UICONTROL Profondità struttura di navigazione]** su **3**.

   ![Configurare i criteri di intestazione](assets/navigation-routing/header-policy.png)

   Questo raccoglierà i 2 livelli di navigazione in profondità sotto `/content/wknd-spa-angular/us/en`.

8. Dopo aver salvato le modifiche, è necessario visualizzare il `Header` popolato come parte del modello:

   ![Componente intestazione compilata](assets/navigation-routing/populated-header.png)

## Creare pagine figlie

Quindi, create ulteriori pagine in AEM che fungeranno da diverse viste del SPA. Verranno inoltre analizzati la struttura gerarchica del modello JSON fornito da AEM.

1. Passate alla console **Siti**: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Selezionare la pagina iniziale angolare **WKND SPA** e fare clic su **[!UICONTROL Crea]** > **[!UICONTROL Pagina]**:

   ![Crea nuova pagina](assets/navigation-routing/create-new-page.png)

2. In **[!UICONTROL Template]** selezionare **[!UICONTROL SPA Page]**. In **[!UICONTROL Proprietà]** immettere **&quot;Page 1&quot;** come nome per **[!UICONTROL Titolo]** e **&quot;page-1&quot;**.

   ![Immettere le proprietà della pagina iniziale](assets/navigation-routing/initial-page-properties.png)

   Fare clic su **[!UICONTROL Crea]** e, nella finestra di dialogo a comparsa, fare clic su **[!UICONTROL Apri]** per aprire la pagina nell&#39;editor SPA AEM.

3. Aggiungere un nuovo componente **[!UICONTROL Testo]** al contenitore di layout principale **[!UICONTROL Contenitore di layout]**. Modificate il componente e inserite il testo: **&quot;Page 1&quot;** utilizzando l&#39;editor Rich Text e l&#39;elemento **H1** (sarà necessario passare alla modalità a schermo intero per modificare gli elementi paragrafo)

   ![Contenuto di esempio pagina 1](assets/navigation-routing/page-1-sample-content.png)

   Potete aggiungere contenuti aggiuntivi, come un’immagine.

4. Tornate alla console AEM Sites  e ripetete i passaggi descritti sopra, creando una seconda pagina denominata **&quot;Page 2&quot;** come elemento di pari livello di **Pagina 1**. Aggiungete il contenuto a **Pagina 2** in modo che venga facilmente identificato.
5. Creare infine una terza pagina, **&quot;Page 3&quot;** ma come **child** di **Page 2**. Una volta completata la gerarchia del sito dovrebbe essere simile a quella riportata di seguito:

   ![Gerarchia del sito di esempio](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. In una nuova scheda, apri l&#39;API del modello JSON fornita da AEM: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Questo contenuto JSON viene richiesto al primo caricamento del SPA. La struttura esterna si presenta come segue:

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

   In `:children` dovrebbe essere visualizzata una voce per ciascuna delle pagine create. Il contenuto per tutte le pagine è in questa richiesta JSON iniziale. Una volta implementato il ciclo di navigazione, le visualizzazioni successive del SPA verranno caricate rapidamente, poiché il contenuto è già disponibile sul lato client.

   Non è saggio caricare **ALL** del contenuto di un SPA nella richiesta JSON iniziale, in quanto ciò rallenterebbe il caricamento della pagina iniziale. Nella sezione che segue viene illustrato come vengono raccolte le profondità di pagine con l’ereditarietà.

7. Andate al modello **SPA Root** all&#39;indirizzo: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Fare clic sul menu **[!UICONTROL Proprietà pagina]** > **[!UICONTROL Criteri pagina]**:

   ![Aprire il criterio pagina per SPA radice](assets/navigation-routing/open-page-policy.png)

8. Il modello **SPA Root** dispone di una scheda **[!UICONTROL Struttura gerarchica]** supplementare per controllare il contenuto JSON raccolto. La **[!UICONTROL Profondità struttura]** determina la profondità nella gerarchia del sito per la raccolta delle pagine figlie sotto la **radice**. È inoltre possibile utilizzare il campo **[!UICONTROL Struttura Pattern]** per filtrare ulteriori pagine in base a un&#39;espressione regolare.

   Aggiornare **[!UICONTROL Profondità struttura]** a **&quot;2&quot;**:

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

   Tenere presente che il percorso **Pagina 3** è stato rimosso: `/content/wknd-spa-angular/us/en/home/page-2/page-3` dal modello JSON iniziale.

   In seguito verrà illustrato come l’SDK dell’editor SPA AEM possa caricare in modo dinamico contenuti aggiuntivi.

## Implementare la navigazione

Implementate quindi il menu di navigazione con un nuovo `NavigationComponent`. Potremmo aggiungere il codice direttamente in `header.component.html`, ma è consigliabile evitare componenti di grandi dimensioni. Implementare invece un `NavigationComponent` che potrebbe essere riutilizzato in un secondo momento.

1. Rivedete il JSON esposto dal componente AEM `Header` all&#39;indirizzo [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   La natura gerarchica delle pagine AEM è modellata nel JSON che può essere utilizzato per compilare un menu di navigazione. Ricordate che il componente `Header` eredita tutte le funzionalità del componente [core di navigazione](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html) e che il contenuto esposto tramite JSON verrà automaticamente mappato sull&#39;annotazione Angular `@Input`.

2. Aprite una nuova finestra del terminale e andate alla cartella `ui.frontend` del progetto SPA. Create un nuovo `NavigationComponent` utilizzando lo strumento CLI angolare:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Create quindi una classe denominata `NavigationLink` utilizzando l&#39;interfaccia CLI angolare nella directory `components/navigation` appena creata:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Tornate all&#39;IDE di vostra scelta e aprite il file in `navigation-link.ts` all&#39;indirizzo `/src/app/components/navigation/navigation-link.ts`.

   ![Apri file navigation-link.ts](assets/navigation-routing/ide-navigation-link-file.png)

5. Compilare `navigation-link.ts` con i seguenti elementi:

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

   Si tratta di una classe semplice che rappresenta un singolo collegamento di navigazione. Nel costruttore di classe si prevede che `data` sia l&#39;oggetto JSON passato da AEM. Questa classe verrà utilizzata sia all&#39;interno di `NavigationComponent` che `HeaderComponent` per comporre facilmente la struttura di navigazione.

   Non viene eseguita alcuna trasformazione di dati, questa classe viene creata principalmente per digitare fortemente il modello JSON. Tenere presente che `this.children` viene digitato come `NavigationLink[]` e che il costruttore crea in modo ricorsivo nuovi oggetti `NavigationLink` per ciascuno degli elementi dell&#39;array `children`. Ricordate che il modello JSON per `Header` è gerarchico.

6. Aprire il file `navigation-link.spec.ts`. Questo è il file di prova per la classe `NavigationLink`. Aggiornalo con i seguenti elementi:

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

   Tenere presente che `const data` segue lo stesso modello JSON analizzato in precedenza per un singolo collegamento. Non si tratta di un solido test di unità, ma dovrebbe essere sufficiente testare il costruttore di `NavigationLink`.

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

   `NavigationComponent` prevede un  `object[]` nome  `items` che sia il modello JSON da AEM. Questa classe espone un singolo metodo `get navigationLinks()` che restituisce un array di oggetti `NavigationLink`.

8. Aprite il file `navigation.component.html` e aggiornatelo con quanto segue:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Questo genera un `<ul>` iniziale e chiama il metodo `get navigationLinks()` da `navigation.component.ts`. Un elemento `<ng-container>` viene utilizzato per effettuare una chiamata a un modello denominato `recursiveListTmpl` e lo trasmette come variabile denominata `navigationLinks`.`links`

   Aggiungere il simbolo `recursiveListTmpl` successivo:

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

   Qui viene implementato il resto del rendering per il collegamento di navigazione. Tenere presente che la variabile `link` è di tipo `NavigationLink` e che tutti i metodi/proprietà creati da tale classe sono disponibili. [`[routerLink]`](https://angular.io/api/router/RouterLink) viene utilizzato al posto dell&#39; `href` attributo normale. Questo ci consente di collegarci a percorsi specifici nell&#39;app, senza un aggiornamento a pagina intera.

   La parte ricorsiva della navigazione viene implementata anche creando un&#39;altra `<ul>` se la `link` corrente ha una matrice `children` non vuota.

9. Aggiornate `navigation.component.spec.ts` per aggiungere il supporto per `RouterTestingModule`:

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

   L&#39;aggiunta di `RouterTestingModule` è necessaria perché il componente utilizza `[routerLink]`.

10. Aggiornate `navigation.component.scss` per aggiungere alcuni stili di base alla `NavigationComponent`:

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

Ora che è stata implementata la `NavigationComponent`, è necessario aggiornare la `HeaderComponent` per farvi riferimento.

1. Aprite un terminale e andate alla cartella `ui.frontend` all&#39;interno del progetto SPA. Avviare il **server di sviluppo webpack**:

   ```shell
   $ npm start
   ```

2. Aprite una scheda del browser e andate a [http://localhost:4200/](Http://localhost:4200/).

   È necessario configurare il server di sviluppo dei webpack **webpack** per il proxy del modello JSON da un&#39;istanza locale di AEM (`ui.frontend/proxy.conf.json`). Questo ci consentirà di eseguire la codifica direttamente rispetto al contenuto creato in AEM da una precedente esercitazione.

   ![menu attivato](./assets/navigation-routing/nav-toggle-static.gif)

   Al momento la funzionalità `HeaderComponent` dispone della funzionalità di attivazione/disattivazione del menu già implementata. Quindi, aggiungete il componente di navigazione.

3. Tornate all&#39;IDE di vostra scelta e aprite il file `header.component.ts` in `ui.frontend/src/app/components/header/header.component.ts`.
4. Aggiornate il metodo `setHomePage()` per rimuovere la stringa codificata e utilizzare i prop dinamici passati dal componente AEM:

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

   Viene creata una nuova istanza di `NavigationLink` basata su `items[0]`, la radice del modello JSON di navigazione passata da AEM. `this.route.snapshot.data.path` restituisce il percorso della route Angular corrente. Questo valore viene utilizzato per determinare se la route corrente è la **Home Page**. `this.homePageUrl` viene utilizzato per compilare il collegamento di ancoraggio sul  **logo**.

5. Aprite `header.component.html` e sostituite il segnaposto statico per la navigazione con un riferimento alla `NavigationComponent` appena creata:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` l&#39;attributo passa  `@Input() items` dal pannello  `HeaderComponent` al  `NavigationComponent` punto in cui verrà creata la navigazione.

6. Aprire `header.component.spec.ts` e aggiungere una dichiarazione per `NavigationComponent`:

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

   Poiché il `NavigationComponent` è ora utilizzato come parte del `HeaderComponent`, deve essere dichiarato come parte del letto di prova.

7. Salvare le modifiche apportate ai file aperti e tornare al server di sviluppo **webpack**: [http://localhost:4200/](Http://localhost:4200/)

   ![Navigazione intestazione completata](assets/navigation-routing/completed-header.png)

   Aprite la navigazione facendo clic sul menu e visualizzate i collegamenti di navigazione compilati. Dovrebbe essere possibile passare a diverse viste del SPA.

## Comprendere il ciclo di SPA

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

   L&#39;array `routes: Routes = [];` definisce i percorsi o i percorsi di navigazione per le mappature dei componenti Angular.

   `AemPageMatcher` è un router Angular personalizzato  [UrlMatcher](https://angular.io/api/router/UrlMatcher), che corrisponde a qualsiasi cosa &quot;sembra&quot; una pagina in AEM che fa parte di questa applicazione Angular.

   `PageComponent` è il componente angolare che rappresenta una pagina in AEM e verranno richiamate le route corrispondenti. La `PageComponent` verrà esaminata ulteriormente.

   `AemPageDataResolver`, fornito dall’SDK JS dell’editor SPA AEM, è un router  [angolare personalizzato ](https://angular.io/api/router/Resolve) Resolverutilizzato per trasformare l’URL di route, che è il percorso AEM include l’estensione .html, nel percorso della risorsa in AEM, che è il percorso della pagina meno l’estensione.

   Ad esempio, il percorso `AemPageDataResolver` trasforma l&#39;URL di una route di `content/wknd-spa-angular/us/en/home.html` in un percorso di `/content/wknd-spa-angular/us/en/home`. Viene utilizzato per risolvere il contenuto della pagina in base al percorso nell&#39;API del modello JSON.

   `AemPageRouteReuseStrategy`, fornito dall’SDK JS dell’editor SPA AEM, è una  [](https://angular.io/api/router/RouteReuseStrategy) RouteReuseStrategyche impedisce il riutilizzo dell’interfaccia  `PageComponent` tra più route. In caso contrario, il contenuto della pagina &quot;A&quot; potrebbe venire visualizzato quando si passa alla pagina &quot;B&quot;.

2. Aprire il file `page.component.ts` in `ui.frontend/src/app/components/page/`.

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

   Il `PageComponent` è necessario per elaborare il JSON recuperato da AEM e viene utilizzato come componente Angular per eseguire il rendering dei percorsi.

   `ActivatedRoute`, fornito dal modulo Router angolare, contiene lo stato che indica quale contenuto JSON di AEM pagina deve essere caricato in questa istanza del componente Pagina angolare.

   `ModelManagerService`, ottiene i dati JSON in base al percorso e li mappa sulle variabili di classe  `path`,  `items`,  `itemsOrder`. Questi verranno quindi passati a [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Aprire il file `page.component.html` in `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` include  [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). Le variabili `path`, `items` e `itemsOrder` vengono passate alla variabile `AEMPageComponent`. La `AemPageComponent`, fornita tramite l&#39;SDK JavaScript di Editor SPA, eseguirà quindi un&#39;iterazione su tali dati e creerà dinamicamente un&#39;istanza di componenti Angular basati sui dati JSON come mostrato nella [esercitazione Mappa componenti](./map-components.md).

   Il `PageComponent` è in realtà solo un proxy per il `AEMPageComponent` ed è il `AEMPageComponent` che esegue la maggior parte del sollevamento pesante per mappare correttamente il modello JSON ai componenti Angular.

##  Inspect il routing SPA in AEM

1. Aprire un terminale e arrestare il server di sviluppo del **webpack** se avviato. Andate alla directory principale del progetto e distribuite il progetto per AEM utilizzando le vostre abilità di Paradiso:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Il progetto Angular ha alcune regole di lint molto rigide abilitate. Se la build Maven non riesce, controllate l&#39;errore e cercate gli errori **Lint trovati nei file elencati.**. Correggete eventuali problemi riscontrati dalla linter ed eseguite nuovamente il comando Maven.

2. Andate alla pagina iniziale SPA in AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) e aprire gli strumenti di sviluppo del browser. Le schermate riportate di seguito sono acquisite dal browser Google Chrome.

   Aggiorna la pagina e dovresti visualizzare una richiesta XHR a `/content/wknd-spa-angular/us/en.model.json`, che è la radice SPA. Si noti che solo tre pagine figlie sono incluse in base alla configurazione della profondità gerarchica per il modello SPA Root creato in precedenza nell&#39;esercitazione. Questo non include **Pagina 3**.

   ![Richiesta JSON iniziale - SPA radice](assets/navigation-routing/initial-json-request.png)

3. Con gli strumenti di sviluppo aperti, andate a **Pagina 3**:

   ![Navigazione pagina 3](assets/navigation-routing/page-three-navigation.png)

   Tenere presente che una nuova richiesta XHR è eseguita per: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Pagina tre richiesta XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager è consapevole del fatto che il contenuto JSON **Page 3** non è disponibile e attiva automaticamente la richiesta XHR aggiuntiva.

4. Continuate a navigare nel SPA utilizzando i vari collegamenti di navigazione. Osservate che non vengono effettuate ulteriori richieste XHR e che non si verificano aggiornamenti di pagina completi. Questo rende il SPA veloce per l&#39;utente finale e riduce le richieste non necessarie al AEM.

   ![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

5. Sperimenta con collegamenti profondi navigando direttamente a: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Tenere presente che il pulsante Indietro del browser continua a funzionare.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato in che modo è possibile supportare più viste nel SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica è stata implementata utilizzando l&#39;indirizzamento angolare e aggiunta al componente `Header`.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o estrarre il codice localmente passando al ramo `Angular/navigation-routing-solution`.

### Passaggi successivi {#next-steps}

[Creare un componente](custom-component.md)  personalizzato - Scoprite come creare un componente personalizzato da utilizzare con l&#39;editor SPA AEM. Scoprite come sviluppare finestre di dialogo degli autori e modelli Sling per estendere il modello JSON e compilare un componente personalizzato.
