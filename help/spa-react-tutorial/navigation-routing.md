---
title: Aggiungi navigazione e indirizzamento | Guida introduttiva dell’Editor SPA di AEM e React
description: Scopri come è possibile supportare più visualizzazioni nell’applicazione a pagina singola mappando le pagine AEM con l’SDK per l’editor di applicazioni a pagina singola. La navigazione dinamica viene implementata utilizzando React Router e aggiunta a un componente Header esistente.
sub-product: sites
feature: Editor SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2117'
ht-degree: 2%

---


# Aggiungi navigazione e indirizzamento {#navigation-routing}

Scopri come è possibile supportare più visualizzazioni nell’applicazione a pagina singola mappando le pagine AEM con l’SDK per l’editor di applicazioni a pagina singola. La navigazione dinamica viene implementata utilizzando React Router e aggiunta a un componente Header esistente.

## Obiettivo

1. Comprendere le opzioni di indirizzamento del modello SPA disponibili quando si utilizza l’Editor SPA.
1. Scopri come utilizzare [React Router](https://reacttraining.com/react-router/) per navigare tra diverse viste dell’applicazione a pagina singola.
1. Implementa una navigazione dinamica guidata dalla gerarchia di pagine AEM.

## Cosa verrà creato

Questo capitolo aggiunge un menu di navigazione a un componente `Header` esistente. Il menu di navigazione sarà guidato dalla gerarchia di pagine AEM e utilizzerà il modello JSON fornito dal [componente di base di navigazione](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html).

![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

## Prerequisiti

Rivedi gli strumenti e le istruzioni necessari per configurare un [ambiente di sviluppo locale](overview.md#local-dev-environment).

### Ottieni il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Distribuisci la base di codice in un’istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se utilizzi [AEM 6.x](overview.md#compatibility) aggiungi il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Installa il pacchetto finito per il tradizionale [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite dal [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) verranno riutilizzate nell&#39;applicazione a pagina singola WKND. Il pacchetto può essere installato utilizzando [Gestione pacchetti di AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager installa wknd.all](./assets/map-components/package-manager-wknd-all.png)

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) o estrarre il codice localmente passando al ramo `React/navigation-routing-solution`.

## Ispezionare gli aggiornamenti delle intestazioni {#inspect-header}

Nei capitoli precedenti, il componente `Header` è stato aggiunto come componente React puro incluso tramite `App.js`. In questo capitolo, il componente `Header` è stato rimosso e verrà aggiunto tramite [Editor modelli](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Questo consentirà agli utenti di configurare il menu di navigazione di `Header` dall’interno di AEM.

>[!NOTE]
>
> Diversi aggiornamenti CSS e JavaScript sono già stati apportati alla base di codice per avviare questo capitolo. Per concentrarti sui concetti di base, non vengono discusse **tutte** delle modifiche al codice. Puoi visualizzare le modifiche complete [qui](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start).

1. Nell’IDE che preferisci, apri il progetto iniziale SPA per questo capitolo.
1. Sotto il modulo `ui.frontend` ispeziona il file `Header.js` in: `ui.frontend/src/components/Header/Header.js`.

   Sono stati effettuati diversi aggiornamenti, tra cui l’aggiunta di un `HeaderEditConfig` e di un `MapTo` per consentire la mappatura del componente su un componente AEM `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. Nel modulo `ui.apps` esamina la definizione del componente AEM `Header` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   Il componente AEM `Header` erediterà tutte le funzionalità del [componente core di navigazione](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) tramite la proprietà `sling:resourceSuperType` .

## Aggiungi l’intestazione al modello {#add-header-template}

1. Apri un browser e accedi ad AEM, [http://localhost:4502/](Http://localhost:4502/). La base di codice iniziale deve essere già distribuita.
1. Passa al **modello di pagina SPA**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Seleziona il contenitore di layout principale più esterno **e fai clic sulla relativa icona** Policy **.** Fai attenzione a **non** per selezionare il **Contenitore di layout** non bloccato per l’authoring.

   ![Seleziona l’icona del criterio del contenitore di layout principale](assets/navigation-routing/root-layout-container-policy.png)

1. Crea un nuovo criterio denominato **Struttura a pagina singola**:

   ![Criteri di struttura per applicazioni a pagina singola](assets/navigation-routing/spa-policy-update.png)

   In **Componenti consentiti** > **Generale** > seleziona il componente **Contenitore di layout** .

   Alla voce **Componenti consentiti** > **REATTO SPA WKND - STRUTTURA** > seleziona il componente **Intestazione** :

   ![Seleziona il componente intestazione](assets/navigation-routing/select-header-component.png)

   In **Componenti consentiti** > **REATTO SPA WKND - Contenuto** > seleziona i componenti **Immagine** e **Testo** . Dovresti aver selezionato 4 componenti totali.

   Fai clic su **Fine** per salvare le modifiche.

1. Aggiorna la pagina e aggiungi il componente **Intestazione** sopra il **Contenitore di layout** sbloccato:

   ![aggiungi componente Intestazione al modello](./assets/navigation-routing/add-header-component.gif)

1. Seleziona il componente **Intestazione** e fai clic sulla relativa icona **Criterio** per modificare il criterio.
1. Crea un nuovo criterio con un **Titolo criterio** di **intestazione SPA WKND**.

   Sotto **Proprietà**:

   * Impostare **Radice di navigazione** su `/content/wknd-spa-react/us/en`.
   * Imposta **Escludi livelli radice** su **1**.
   * Deseleziona **Raccogli tutte le pagine figlie**.
   * Impostare **Profondità struttura di navigazione** su **3**.

   ![Configurare i criteri di intestazione](assets/navigation-routing/header-policy.png)

   Questo raccoglierà i 2 livelli di navigazione in profondità sotto `/content/wknd-spa-react/us/en`.

1. Dopo aver salvato le modifiche, dovresti vedere il popolato `Header` come parte del modello:

   ![Componente intestazione popolata](assets/navigation-routing/populated-header.png)

## Crea pagine figlie

Quindi, crea altre pagine in AEM che fungeranno da viste diverse nell’applicazione a pagina singola. Esamineremo anche la struttura gerarchica del modello JSON fornito da AEM.

1. Passa alla console **Sites** : [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Seleziona la pagina principale **React SPA WKND** e fai clic su **Crea** > **Pagina**:

   ![Crea nuova pagina](assets/navigation-routing/create-new-page.png)

1. In **Modello** seleziona **Pagina applicazioni a pagina singola**. Sotto **Proprietà** immetti **Pagina 1** come nome **Titolo** e **pagina-1**.

   ![Immetti le proprietà della pagina iniziale](assets/navigation-routing/initial-page-properties.png)

   Fai clic su **Crea** e, nella finestra di dialogo a comparsa, fai clic su **Apri** per aprire la pagina nell’Editor SPA di AEM.

1. Aggiungi un nuovo componente **Testo** al **Contenitore di layout** principale. Modifica il componente e immetti il testo: **Pagina 1** utilizzando l’editor Rich Text e l’elemento **H1** (sarà necessario accedere alla modalità a tutto schermo per modificare gli elementi di paragrafo)

   ![Contenuto di esempio pagina 1](assets/navigation-routing/page-1-sample-content.png)

   Puoi aggiungere altri contenuti, come un’immagine.

1. Torna alla console AEM Sites e ripeti i passaggi precedenti, creando una seconda pagina denominata **Pagina 2** come pari a **Pagina 1**.
1. Infine, crea una terza pagina, **Pagina 3** ma come **figlio** di **Pagina 2**. Una volta completata la gerarchia del sito, avrà un aspetto simile al seguente:

   ![Gerarchia del sito di esempio](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. In una nuova scheda, apri l’API del modello JSON fornita da AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Questo contenuto JSON viene richiesto al primo caricamento dell’applicazione a pagina singola. La struttura esterna si presenta come segue:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {},
       "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
       }
   }
   ```

   Alla voce `:children` dovrebbe essere visualizzata una voce per ciascuna delle pagine create. Il contenuto per tutte le pagine si trova in questa richiesta JSON iniziale. Una volta implementato il routing di navigazione, le visualizzazioni successive dell’applicazione a pagina singola verranno caricate rapidamente, poiché il contenuto è già disponibile sul lato client.

   Non è saggio caricare **ALL** del contenuto di un’applicazione a pagina singola nella richiesta JSON iniziale, in quanto ciò rallenterebbe il caricamento della pagina iniziale. Ora, esaminiamo come viene raccolta la profondità irarica delle pagine.

1. Passa al modello **SPA Root** in: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Fai clic sul menu **Proprietà pagina** > **Criterio pagina**:

   ![Apri i criteri di pagina per la directory principale SPA](assets/navigation-routing/open-page-policy.png)

1. Il modello **Radice applicazione a pagina singola** dispone di una scheda **Struttura gerarchica** aggiuntiva per controllare il contenuto JSON raccolto. La **Profondità struttura** determina la profondità nella gerarchia del sito per la raccolta delle pagine figlie sotto la **radice**. È inoltre possibile utilizzare il campo **Pattern struttura** per filtrare ulteriori pagine in base a un’espressione regolare.

   Aggiorna **Profondità struttura** a **2**:

   ![Aggiorna profondità struttura](assets/navigation-routing/update-structure-depth.png)

   Fai clic su **Fine** per salvare le modifiche al criterio.

1. Riapri il modello JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {}
       }
   }
   ```

   Nota che il percorso **Pagina 3** è stato rimosso: `/content/wknd-spa-react/us/en/home/page-2/page-3` dal modello JSON iniziale.

   In seguito, osserveremo come l’SDK dell’Editor SPA di AEM può caricare in modo dinamico contenuti aggiuntivi.

## Implementare la navigazione

Quindi, implementa il menu di navigazione come parte del `Header`. Potremmo aggiungere il codice direttamente in `Header.js` ma una procedura migliore con è quella di evitare componenti di grandi dimensioni. Verrà invece implementato un componente `Navigation` SPA che potrebbe essere riutilizzato in un secondo momento.

1. Rivedi il JSON esposto dal componente AEM `Header` in [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   La natura gerarchica delle pagine AEM è modellata nel JSON che può essere utilizzato per compilare un menu di navigazione. Ricorda che il componente `Header` eredita tutte le funzionalità del [componente di base di navigazione](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) e che il contenuto esposto tramite il JSON verrà mappato automaticamente alle proprietà React.

1. Apri una nuova finestra terminale e passa alla cartella `ui.frontend` del progetto SPA. Avvia il **webpack-dev-server** con il comando `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Apri una nuova scheda del browser e passa a [http://localhost:3000/](Http://localhost:3000/).

   Il **webpack-dev-server** deve essere configurato per eseguire il proxy del modello JSON da un&#39;istanza locale di AEM (`ui.frontend/.env.development`). Questo ci consentirà di eseguire il codice direttamente rispetto al contenuto creato in AEM nell’esercizio precedente. Verifica di essere autenticato in AEM nella stessa sessione di navigazione.

   ![funzionamento del menu](./assets/navigation-routing/nav-toggle-static.gif)

   Al momento `Header` dispone della funzionalità di attivazione/disattivazione menu già implementata. Quindi, implementa il menu di navigazione.

1. Torna all’IDE che preferisci e apri `Header.js` in `ui.frontend/src/components/Header/Header.js`.
1. Aggiorna il metodo `homeLink()` per rimuovere la stringa hardcoded e utilizza le proprietà dinamiche passate dal componente AEM:

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   Il codice di cui sopra compilerà un url in base all’elemento di navigazione principale configurato dal componente. `homeLink()` viene utilizzato per popolare il logo nel  `logo()` metodo e per determinare se il pulsante Indietro deve essere visualizzato in  `backButton()`.

   Salva le modifiche apportate a `Header.js`.

1. Aggiungi una riga nella parte superiore di `Header.js` per importare il componente `Navigation` sotto le altre importazioni:

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. Aggiorna quindi il metodo `get navigation()` per creare un&#39;istanza del componente `Navigation`:

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Come accennato in precedenza, invece di implementare la navigazione all’interno di `Header` implementeremo la maggior parte della logica nel componente `Navigation` .  Le proprietà di `Header` includono la struttura JSON necessaria per creare il menu, passiamo tutte le proprietà.
1. Apri il file `Navigation.js` in `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implementa il metodo `renderGroupNav(children)` :

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   Questo metodo prende una matrice di elementi di navigazione, `children`, e crea un elenco non ordinato. Quindi esegue un’iterazione sull’array e passa l’elemento al `renderNavItem`, che verrà implementato successivamente.

1. Implementa il `renderNavItem`:

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   Questo metodo esegue il rendering di un elemento dell’elenco, con classi CSS basate su proprietà `level` e `active`. Il metodo chiama quindi `renderLink` per creare il tag di ancoraggio. Poiché il contenuto `Navigation` è gerarchico, viene utilizzata una strategia ricorsiva per chiamare `renderGroupNav` per gli elementi secondari dell’elemento corrente.

1. Implementa il metodo `renderLink` :

   Aggiungi un metodo di importazione per il componente [Link](https://reacttraining.com/react-router/web/api/Link), parte del router React, nella parte superiore del file con le altre importazioni:

   ```js
   import {Link} from "react-router-dom";
   ```

   Completare l&#39;implementazione del metodo `renderLink`:

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   Invece di un normale tag di ancoraggio, `<a>` viene utilizzato il componente [Collegamento](https://reacttraining.com/react-router/web/api/Link) . Questo assicura che non venga attivato un aggiornamento completo della pagina e sfrutta invece il router React fornito dall’SDK JS dell’Editor SPA di AEM.

1. Salva le modifiche in `Navigation.js` e torna al **webpack-dev-server**: [http://localhost:3000](Http://localhost:3000)

   ![Navigazione intestazione completata](assets/navigation-routing/completed-header.png)

   Apri la navigazione facendo clic sull’interruttore del menu e dovresti visualizzare i collegamenti di navigazione compilati. Dovresti essere in grado di passare a diverse visualizzazioni dell’applicazione a pagina singola.

## Ispezionare il routing SPA

Ora che la navigazione è stata implementata, controlla il routing in AEM.

1. Nell’IDE, apri il file `index.js` in `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   Tieni presente che il `App` viene racchiuso nel componente `Router` da [React Router](https://reacttraining.com/react-router/). Il `ModelManager`, fornito dall’SDK JS per AEM SPA Editor, aggiunge i percorsi dinamici alle pagine AEM in base all’API del modello JSON.

1. Apri un terminale, accedi alla directory principale del progetto e implementa il progetto in AEM utilizzando le tue competenze Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Passa alla home page dell’applicazione a pagina singola in AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) e apri gli strumenti per sviluppatori del browser. Le schermate seguenti vengono acquisite dal browser Google Chrome.

   Aggiorna la pagina e dovresti vedere una richiesta XHR a `/content/wknd-spa-react/us/en.model.json`, che è la directory principale dell’applicazione a pagina singola. Solo tre pagine figlie sono incluse in base alla configurazione della profondità gerarchica del modello SPA Root creato in precedenza nell’esercitazione. Questo non include **Pagina 3**.

   ![Richiesta JSON iniziale - Radice SPA](assets/navigation-routing/initial-json-request.png)

1. Con gli strumenti per sviluppatori aperti, utilizza la navigazione `Header` per passare a **Pagina 3**:

   ![Pagina 3 Naviga](assets/navigation-routing/page-three-navigation.png)

   Tieni presente che viene effettuata una nuova richiesta XHR a: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Pagina tre richiesta XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager è consapevole del fatto che il contenuto JSON **Pagina 3** non è disponibile e attiva automaticamente la richiesta XHR aggiuntiva.

1. Continua a navigare nell’applicazione a pagina singola utilizzando i vari collegamenti di navigazione del componente `Header` . Osserva che non vengono effettuate richieste XHR aggiuntive e che non si verifica alcun aggiornamento completo della pagina. Questo rende l’applicazione a pagina singola veloce per l’utente finale e riduce le richieste non necessarie ad AEM.

   ![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

1. Sperimenta i collegamenti profondi navigando direttamente in: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Tieni presente che il pulsante Indietro del browser continua a funzionare.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato come è possibile supportare più visualizzazioni nell’applicazione a pagina singola mappando le pagine AEM con l’SDK per l’editor di applicazioni a pagina singola. La navigazione dinamica è stata implementata utilizzando React Router e aggiunta al componente `Header` .

Puoi sempre visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) o estrarre il codice localmente passando al ramo `React/navigation-routing-solution`.
