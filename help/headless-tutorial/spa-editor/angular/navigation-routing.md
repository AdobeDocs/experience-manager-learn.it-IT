---
title: Aggiungi navigazione e indirizzamento | Guida introduttiva all’editor di SPA AEM e all’Angular
description: Scopri come sono supportate più visualizzazioni nell’SPA utilizzando AEM pagine e l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando percorsi di Angular e aggiunta a un componente Header esistente.
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2713'
ht-degree: 1%

---

# Aggiungi navigazione e indirizzamento {#navigation-routing}

Scopri come sono supportate più visualizzazioni nell’SPA utilizzando AEM pagine e l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando percorsi di Angular e aggiunta a un componente Header esistente.

## Obiettivo

1. Comprendere le opzioni di indirizzamento del modello SPA disponibili quando si utilizza l&#39;editor SPA.
2. Scopri come utilizzare [routing di Angular](https://angular.io/guide/router) per navigare tra diverse viste del SPA.
3. Implementa una navigazione dinamica guidata dalla gerarchia di pagine AEM.

## Cosa verrà creato

Questo capitolo aggiunge un menu di navigazione a un componente `Header` esistente. Il menu di navigazione è guidato dalla gerarchia di pagine AEM e utilizza il modello JSON fornito dal [componente di base di navigazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

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

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installa il pacchetto finito per il tradizionale [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite dal [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) verranno riutilizzate nel SPA WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o estrarre il codice localmente passando al ramo `Angular/navigation-routing-solution`.

## Aggiornamenti di Inspect HeaderComponent {#inspect-header}

Nei capitoli precedenti, il componente `HeaderComponent` è stato aggiunto come componente di Angular puro incluso tramite `app.component.html`. In questo capitolo, il componente `HeaderComponent` viene rimosso dall&#39;app e verrà aggiunto tramite l&#39; [Editor modelli](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Questo consente agli utenti di configurare il menu di navigazione di `HeaderComponent` da AEM.

>[!NOTE]
>
> Diversi aggiornamenti CSS e JavaScript sono già stati apportati alla base di codice per avviare questo capitolo. Per concentrarti sui concetti di base, non vengono discusse **tutte** delle modifiche al codice. Puoi visualizzare le modifiche complete [qui](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. Nell’IDE che preferisci, apri il progetto iniziale SPA per questo capitolo.
2. Sotto il modulo `ui.frontend` ispeziona il file `header.component.ts` in: `ui.frontend/src/app/components/header/header.component.ts`.

   Sono stati effettuati diversi aggiornamenti, tra cui l’aggiunta di un `HeaderEditConfig` e di un `MapTo` per consentire la mappatura del componente su un componente AEM `wknd-spa-angular/components/header`.

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

   Osserva l’annotazione `@Input()` per `items`. `items` conterrà una matrice di oggetti di navigazione passati da AEM.

3. Nel modulo `ui.apps` esamina la definizione del componente AEM `Header` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   Il componente AEM `Header` erediterà tutte le funzionalità del [componente di base di navigazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) tramite la proprietà `sling:resourceSuperType` .

## Aggiungi il componente Header al modello SPA {#add-header-template}

1. Apri un browser e accedi a AEM, [http://localhost:4502/](Http://localhost:4502/). La base di codice iniziale deve essere già distribuita.
2. Passa al **[!UICONTROL SPA modello di pagina]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Seleziona il contenitore di layout principale più esterno **[!UICONTROL e fai clic sulla relativa icona**[!UICONTROL  Policy ]**.]** Fai attenzione a **non** per selezionare il **[!UICONTROL Contenitore di layout]** non bloccato per l’authoring.

   ![Seleziona l’icona del criterio del contenitore di layout principale](assets/navigation-routing/root-layout-container-policy.png)

4. Copia il criterio corrente e crea un nuovo criterio denominato **[!UICONTROL Struttura SPA]**:

   ![Criteri per la struttura SPA](assets/map-components/spa-policy-update.png)

   In **[!UICONTROL Componenti consentiti]** > **[!UICONTROL Generale]** > seleziona il componente **[!UICONTROL Contenitore di layout]** .

   In **[!UICONTROL Componenti consentiti]** > **[!UICONTROL ANGULAR SPA WKND - STRUTTURA]** > seleziona il componente **[!UICONTROL Intestazione]**:

   ![Seleziona il componente intestazione](assets/map-components/select-header-component.png)

   In **[!UICONTROL Componenti consentiti]** > **[!UICONTROL ANGULAR SPA WKND - Contenuto]** > seleziona i componenti **[!UICONTROL Immagine]** e **[!UICONTROL Testo]** . Dovresti aver selezionato 4 componenti totali.

   Fai clic su **[!UICONTROL Fine]** per salvare le modifiche.

5. **Aggiorna la pagina.** Aggiungi il componente **[!UICONTROL Intestazione]** sopra il contenitore di layout **[!UICONTROL sbloccato]**:

   ![aggiungi componente Intestazione al modello](./assets/navigation-routing/add-header-component.gif)

6. Seleziona il componente **[!UICONTROL Intestazione]** e fai clic sulla relativa icona **Criterio** per modificare il criterio.

   ![Fai clic su Criterio intestazione](assets/navigation-routing/header-policy-icon.png)

7. Crea un nuovo criterio con un **[!UICONTROL Titolo criterio]** di **&quot;WKND SPA Header&quot;**.

   Sotto **[!UICONTROL Proprietà]**:

   * Impostare **[!UICONTROL Radice di navigazione]** su `/content/wknd-spa-angular/us/en`.
   * Imposta **[!UICONTROL Escludi livelli radice]** su **1**.
   * Deseleziona **[!UICONTROL Raccogli tutte le pagine figlie]**.
   * Impostare **[!UICONTROL Profondità struttura di navigazione]** su **3**.

   ![Configurare i criteri di intestazione](assets/navigation-routing/header-policy.png)

   Questo raccoglierà i 2 livelli di navigazione in profondità sotto `/content/wknd-spa-angular/us/en`.

8. Dopo aver salvato le modifiche, dovresti vedere il popolato `Header` come parte del modello:

   ![Componente intestazione popolata](assets/navigation-routing/populated-header.png)

## Creare pagine figlie

Quindi, crea altre pagine in AEM che fungeranno da viste diverse nel SPA. Esamineremo anche la struttura gerarchica del modello JSON fornito da AEM.

1. Passa alla console **Sites** : [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Seleziona la **Pagina iniziale Angular WKND SPA** e fai clic su **[!UICONTROL Crea]** > **[!UICONTROL Pagina]**:

   ![Crea nuova pagina](assets/navigation-routing/create-new-page.png)

2. In **[!UICONTROL Modello]** selezionare **[!UICONTROL SPA Pagina]**. Sotto **[!UICONTROL Proprietà]** immetti **&quot;Pagina 1&quot;** come nome **[!UICONTROL Titolo]** e **&quot;page-1&quot;**.

   ![Immetti le proprietà della pagina iniziale](assets/navigation-routing/initial-page-properties.png)

   Fai clic su **[!UICONTROL Crea]** e, nella finestra di dialogo a comparsa, fai clic su **[!UICONTROL Apri]** per aprire la pagina nell&#39;editor di SPA AEM.

3. Aggiungi un nuovo componente **[!UICONTROL Testo]** al **[!UICONTROL Contenitore di layout]** principale. Modifica il componente e immetti il testo: **&quot;Pagina 1&quot;** utilizzando l’editor Rich Text e l’elemento **H1** (sarà necessario accedere alla modalità a tutto schermo per modificare gli elementi di paragrafo)

   ![Contenuto di esempio pagina 1](assets/navigation-routing/page-1-sample-content.png)

   Puoi aggiungere altri contenuti, come un’immagine.

4. Torna alla console AEM Sites e ripeti i passaggi precedenti, creando una seconda pagina denominata **&quot;Pagina 2&quot;** come pari a **Pagina 1**. Aggiungi il contenuto a **Pagina 2** in modo che sia facilmente identificato.
5. Infine, crea una terza pagina, **&quot;Pagina 3&quot;** ma come **figlio** di **Pagina 2**. Una volta completata la gerarchia del sito, avrà un aspetto simile al seguente:

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

   Alla voce `:children` dovrebbe essere visualizzata una voce per ciascuna delle pagine create. Il contenuto per tutte le pagine si trova in questa richiesta JSON iniziale. Una volta implementato il ciclo di navigazione, le visualizzazioni successive del SPA verranno caricate rapidamente, poiché il contenuto è già disponibile sul lato client.

   Non è saggio caricare **ALL** del contenuto di un SPA nella richiesta JSON iniziale, in quanto ciò rallenterebbe il caricamento della pagina iniziale. Ora, esaminiamo come viene raccolta la profondità irarica delle pagine.

7. Passa al modello **SPA Root** in: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Fai clic sul menu **[!UICONTROL Proprietà pagina]** > **[!UICONTROL Criterio pagina]**:

   ![Apri i criteri di pagina per SPA radice](assets/navigation-routing/open-page-policy.png)

8. Il modello **SPA Root** dispone di una scheda **[!UICONTROL Struttura gerarchica]** aggiuntiva per controllare il contenuto JSON raccolto. La **[!UICONTROL Profondità struttura]** determina la profondità nella gerarchia del sito per la raccolta delle pagine figlie sotto la **radice**. È inoltre possibile utilizzare il campo **[!UICONTROL Pattern struttura]** per filtrare ulteriori pagine in base a un’espressione regolare.

   Aggiorna **[!UICONTROL Profondità struttura]** a **&quot;2&quot;**:

   ![Aggiorna profondità struttura](assets/navigation-routing/update-structure-depth.png)

   Fai clic su **[!UICONTROL Fine]** per salvare le modifiche al criterio.

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

   Nota che il percorso **Pagina 3** è stato rimosso: `/content/wknd-spa-angular/us/en/home/page-2/page-3` dal modello JSON iniziale.

   In seguito, osserveremo come l’SDK dell’editor di SPA AEM può caricare in modo dinamico contenuti aggiuntivi.

## Implementare la navigazione

Quindi, implementa il menu di navigazione con un nuovo `NavigationComponent`. Potremmo aggiungere il codice direttamente in `header.component.html`, ma una procedura migliore è quella di evitare componenti di grandi dimensioni. Al contrario, implementa un `NavigationComponent` che potrebbe essere riutilizzato in un secondo momento.

1. Rivedi il JSON esposto dal componente AEM `Header` in [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   La natura gerarchica delle pagine AEM è modellata nel JSON che può essere utilizzato per compilare un menu di navigazione. Ricorda che il componente `Header` eredita tutte le funzionalità del [componente core di navigazione](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html) e che il contenuto esposto tramite il JSON verrà mappato automaticamente all’annotazione Angular `@Input`.

2. Apri una nuova finestra terminale e passa alla cartella `ui.frontend` del progetto SPA. Crea un nuovo `NavigationComponent` utilizzando lo strumento Angular CLI:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Crea quindi una classe denominata `NavigationLink` utilizzando Angular CLI nella directory appena creata `components/navigation` :

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Torna all’IDE che preferisci e apri il file in `navigation-link.ts` all’indirizzo `/src/app/components/navigation/navigation-link.ts`.

   ![Apri il file navigation-link.ts](assets/navigation-routing/ide-navigation-link-file.png)

5. Popolare `navigation-link.ts` con quanto segue:

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

   Si tratta di una classe semplice che rappresenta un singolo collegamento di navigazione. Nel costruttore di classi ci aspettiamo che `data` sia l&#39;oggetto JSON passato da AEM. Questa classe verrà utilizzata sia all&#39;interno di `NavigationComponent` che `HeaderComponent` per popolare facilmente la struttura di navigazione.

   Non viene eseguita alcuna trasformazione dei dati, questa classe viene creata principalmente per digitare con forza il modello JSON. Tenere presente che `this.children` viene digitato come `NavigationLink[]` e che il costruttore crea in modo ricorsivo nuovi oggetti `NavigationLink` per ciascuno degli elementi della matrice `children`. Ricorda che il modello JSON per `Header` è gerarchico.

6. Aprire il file `navigation-link.spec.ts`. Questo è il file di prova per la classe `NavigationLink` . Aggiornalo con i seguenti elementi:

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

   Nota che `const data` segue lo stesso modello JSON esaminato in precedenza per un singolo collegamento. Questo è lungi dall&#39;essere un solido test dell&#39;unità, tuttavia dovrebbe essere sufficiente testare il costruttore di `NavigationLink`.

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

   `NavigationComponent` richiede un  `object[]` nome  `items` che è il modello JSON da AEM. Questa classe espone un singolo metodo `get navigationLinks()` che restituisce una matrice di oggetti `NavigationLink`.

8. Apri il file `navigation.component.html` e aggiornalo con quanto segue:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Questo genera un `<ul>` iniziale e chiama il metodo `get navigationLinks()` da `navigation.component.ts`. Un elemento `<ng-container>` viene utilizzato per effettuare una chiamata a un modello denominato `recursiveListTmpl` e lo trasmette come variabile `navigationLinks` denominata `links`.

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

   Qui viene implementato il resto del rendering per il collegamento di navigazione. La variabile `link` è di tipo `NavigationLink` e sono disponibili tutti i metodi/proprietà creati da tale classe. [`[routerLink]`](https://angular.io/api/router/RouterLink) viene utilizzato al posto dell&#39; `href` attributo normale. Questo ci consente di collegare a percorsi specifici nell’app, senza un aggiornamento a pagina intera.

   La parte ricorsiva della navigazione viene implementata anche creando un&#39;altra `<ul>` se la `link` corrente dispone di una matrice `children` non vuota.

9. Aggiorna `navigation.component.spec.ts` per aggiungere il supporto per `RouterTestingModule`:

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

   L’aggiunta di `RouterTestingModule` è necessaria perché il componente utilizza `[routerLink]`.

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

Ora che è stato implementato `NavigationComponent`, è necessario aggiornare `HeaderComponent` per farvi riferimento.

1. Apri un terminale e passa alla cartella `ui.frontend` all’interno del progetto SPA. Avviare il **webpack dev server**:

   ```shell
   $ npm start
   ```

2. Apri una scheda del browser e passa a [http://localhost:4200/](Http://localhost:4200/).

   Il **webpack dev server** deve essere configurato per eseguire il proxy del modello JSON da un&#39;istanza locale di AEM (`ui.frontend/proxy.conf.json`). Questo ci consentirà di eseguire il codice direttamente in base al contenuto creato in AEM dalla precedente esercitazione.

   ![funzionamento del menu](./assets/navigation-routing/nav-toggle-static.gif)

   Al momento `HeaderComponent` dispone della funzionalità di attivazione/disattivazione menu già implementata. Quindi, aggiungi il componente di navigazione.

3. Torna all’IDE che preferisci e apri il file `header.component.ts` in `ui.frontend/src/app/components/header/header.component.ts`.
4. Aggiorna il metodo `setHomePage()` per rimuovere la stringa hardcoded e utilizza le proprietà dinamiche passate dal componente AEM:

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

   Viene creata una nuova istanza di `NavigationLink` basata su `items[0]`, la radice del modello JSON di navigazione passato da AEM. `this.route.snapshot.data.path` restituisce il percorso del percorso Angular corrente. Questo valore viene utilizzato per determinare se la route corrente è la **Home Page**. `this.homePageUrl` viene utilizzato per popolare il collegamento di ancoraggio sul  **logo**.

5. Apri `header.component.html` e sostituisci il segnaposto statico per la navigazione con un riferimento al nuovo `NavigationComponent` creato:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` passa l’attributo  `@Input() items` dal  `HeaderComponent` al  `NavigationComponent` punto in cui verrà generata la navigazione.

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

   Poiché il `NavigationComponent` è ora utilizzato come parte del `HeaderComponent`, deve essere dichiarato come parte del banco di prova.

7. Salva le modifiche apportate a qualsiasi file aperto e torna al server di sviluppo del **webpack**: [http://localhost:4200/](Http://localhost:4200/)

   ![Navigazione intestazione completata](assets/navigation-routing/completed-header.png)

   Apri la navigazione facendo clic sull’interruttore del menu e dovresti visualizzare i collegamenti di navigazione compilati. Dovresti essere in grado di passare a diverse viste della SPA.

## Comprendere il ciclo di SPA

Ora che la navigazione è stata implementata, controlla il routing in AEM.

1. Nell’IDE, apri il file `app-routing.module.ts` in `ui.frontend/src/app`.

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

   La matrice `routes: Routes = [];` definisce i percorsi o i percorsi di navigazione per Angular le mappature dei componenti.

   `AemPageMatcher` è un router di Angular personalizzato  [UrlMatcher](https://angular.io/api/router/UrlMatcher), che corrisponde a qualsiasi cosa che &quot;assomiglia&quot; a una pagina in AEM che fa parte di questa applicazione di Angular.

   `PageComponent` è il componente Angular che rappresenta una pagina in AEM e verranno richiamati i percorsi corrispondenti. Il `PageComponent` verrà sottoposto a ulteriori ispezioni.

   `AemPageDataResolver`, fornito dall’SDK JS dell’editor di AEM SPA, è un  [Angular Router ](https://angular.io/api/router/Resolve) Resolverutilizzato per trasformare l’URL di route, che è il percorso in AEM inclusa l’estensione .html, nel percorso della risorsa in AEM, che è il percorso della pagina meno l’estensione .

   Ad esempio, l’ `AemPageDataResolver` trasforma l’URL di una route di `content/wknd-spa-angular/us/en/home.html` in un percorso di `/content/wknd-spa-angular/us/en/home`. Viene utilizzato per risolvere il contenuto della pagina in base al percorso nell’API del modello JSON.

   `AemPageRouteReuseStrategy`, fornito dall’SDK JS dell’editor di AEM SPA, è una  [](https://angular.io/api/router/RouteReuseStrategy) RouteReuseStrategyche impedisce il riutilizzo di  `PageComponent` tra percorsi diversi. In caso contrario, il contenuto della pagina &quot;A&quot; potrebbe comparire quando si passa alla pagina &quot;B&quot;.

2. Apri il file `page.component.ts` in `ui.frontend/src/app/components/page/`.

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

   `ActivatedRoute`, fornito dal modulo Router di Angular, contiene lo stato che indica AEM contenuto JSON della pagina da caricare in questa istanza del componente Pagina di Angular.

   `ModelManagerService`, ottiene i dati JSON in base al percorso e li mappa su variabili di classe  `path`,  `items`,  `itemsOrder`. Questi verranno quindi passati a [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Apri il file `page.component.html` in `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` include  [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). Le variabili `path`, `items` e `itemsOrder` vengono passate a `AEMPageComponent`. La sezione `AemPageComponent`, fornita tramite gli SDK JavaScript per l’editor di SPA, eseguirà quindi un’iterazione su tali dati e creerà dinamicamente un’istanza di componenti Angular in base ai dati JSON come mostrato nell’ [esercitazione sui componenti della mappa](./map-components.md).

   Il `PageComponent` è in realtà solo un proxy per il `AEMPageComponent` ed è il `AEMPageComponent` che esegue la maggior parte del sollevamento pesante per mappare correttamente il modello JSON ai componenti di Angular.

## Inspect il routing SPA in AEM

1. Aprire un terminale e arrestare il **webpack dev server** se avviato. Passa alla directory principale del progetto e distribuisci il progetto da AEM utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Nel progetto di Angular sono state attivate alcune regole di stampa molto rigide. Se la build Maven non riesce, controlla l&#39;errore e cerca gli errori **Lint trovati nei file elencati.**. Correggi eventuali problemi rilevati dall&#39;icona ed esegui nuovamente il comando Maven.

2. Passa alla home page SPA in AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) e apri gli strumenti per sviluppatori del browser. Le schermate seguenti vengono acquisite dal browser Google Chrome.

   Aggiorna la pagina e dovresti vedere una richiesta XHR a `/content/wknd-spa-angular/us/en.model.json`, che è la radice SPA. Solo tre pagine figlie sono incluse in base alla configurazione della profondità gerarchica del modello SPA Root creato in precedenza nell’esercitazione. Questo non include **Pagina 3**.

   ![Richiesta JSON iniziale - SPA radice](assets/navigation-routing/initial-json-request.png)

3. Con gli strumenti per sviluppatori aperti, passa a **Pagina 3**:

   ![Pagina 3 Naviga](assets/navigation-routing/page-three-navigation.png)

   Tieni presente che viene effettuata una nuova richiesta XHR a: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Pagina tre richiesta XHR](assets/navigation-routing/page-3-xhr-request.png)

   Il AEM Model Manager è consapevole del fatto che il contenuto JSON **Pagina 3** non è disponibile e attiva automaticamente la richiesta XHR aggiuntiva.

4. Continua a navigare nel SPA utilizzando i vari collegamenti di navigazione. Osserva che non vengono effettuate richieste XHR aggiuntive e che non si verifica alcun aggiornamento completo della pagina. In questo modo l’SPA diventa veloce per l’utente finale e si riducono le richieste non necessarie a AEM.

   ![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

5. Sperimenta i collegamenti profondi navigando direttamente in: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Tieni presente che il pulsante Indietro del browser continua a funzionare.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato come è possibile supportare più visualizzazioni nel SPA mappando le pagine AEM con l’SDK dell’editor SPA. La navigazione dinamica è stata implementata utilizzando il ciclo di Angular e aggiunta al componente `Header` .

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) o estrarre il codice localmente passando al ramo `Angular/navigation-routing-solution`.

### Passaggi successivi {#next-steps}

[Creare un componente personalizzato](custom-component.md)  - Scopri come creare un componente personalizzato da utilizzare con l’editor di SPA di AEM. Scopri come sviluppare le finestre di dialogo degli autori e i modelli Sling per estendere il modello JSON a un componente personalizzato.
