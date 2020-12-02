---
title: Aggiunta di navigazione e routing | Guida introduttiva all'editor SPA AEM e React
description: Scoprite come è possibile supportare più viste nel SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando React Router e aggiunta a un componente Intestazione esistente.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '2112'
ht-degree: 1%

---


# Aggiunta di navigazione e routing {#navigation-routing}

Scoprite come è possibile supportare più viste nel SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando React Router e aggiunta a un componente Intestazione esistente.

## Obiettivo

1. Comprendere le opzioni di routing SPA modello disponibili quando si utilizza l&#39;editor SPA.
1. Scoprite come utilizzare [React Router](https://reacttraining.com/react-router/) per spostarsi tra le diverse viste del SPA.
1. Implementa una navigazione dinamica guidata dalla gerarchia di pagine AEM.

## Cosa verrà creato

Questo capitolo aggiunge un menu di navigazione a un componente `Header` esistente. Il menu di navigazione sarà guidato dalla gerarchia di pagine AEM e utilizzerà il modello JSON fornito dal [componente core di navigazione](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html).

![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

## Prerequisiti

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente di sviluppo locale [](overview.md#local-dev-environment).

### Ottenere il codice

1. Scarica il punto di partenza per questa esercitazione tramite Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Distribuire la base di codice in un&#39;istanza AEM locale utilizzando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se si utilizza [AEM 6.x](overview.md#compatibility) aggiungere il profilo `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Installate il pacchetto finito per il tradizionale sito di riferimento [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Le immagini fornite dal sito di riferimento [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) verranno riutilizzate sul SPA WKND. Il pacchetto può essere installato utilizzando [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) o estrarre il codice localmente passando al ramo `React/navigation-routing-solution`.

##  aggiornamenti di intestazione Inspect {#inspect-header}

Nei capitoli precedenti, il componente `Header` veniva aggiunto come componente React puro incluso tramite `App.js`. In questo capitolo, il componente `Header` è stato rimosso e verrà aggiunto tramite l&#39; [Editor modelli](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Questo consentirà agli utenti di configurare il menu di navigazione di `Header` dall&#39;interno AEM.

>[!NOTE]
>
> Diversi aggiornamenti CSS e JavaScript sono già stati apportati alla base di codice per avviare questo capitolo. Per concentrarsi sui concetti di base, non vengono discusse tutte le **tutte** delle modifiche al codice. Potete visualizzare le modifiche complete [qui](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start).

1. Nell’IDE di vostra scelta, aprite il progetto iniziale SPA per questo capitolo.
1. Sotto il modulo `ui.frontend` ispezionare il file `Header.js` in: `ui.frontend/src/components/Header/Header.js`.

   Sono stati eseguiti diversi aggiornamenti, tra cui l&#39;aggiunta di un `HeaderEditConfig` e di un `MapTo` per consentire la mappatura del componente su un componente AEM `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. Nel modulo `ui.apps` ispeziona la definizione del componente AEM `Header`: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   Il componente AEM `Header` erediterà tutte le funzionalità del componente core di navigazione [a2/> tramite la proprietà `sling:resourceSuperType`.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)

## Aggiungere l&#39;intestazione al modello {#add-header-template}

1. Aprite un browser e accedete a AEM, [http://localhost:4502/](Http://localhost:4502/). La base di codice iniziale dovrebbe essere già distribuita.
1. Passare al **SPA modello di pagina**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Selezionare il contenitore di layout radice più esterno **e fare clic sull&#39;icona** Policy **.** Fare attenzione a **non** per selezionare **Contenitore di layout** non bloccato per l&#39;authoring.

   ![Selezionate l&#39;icona del criterio contenitore del layout principale](assets/navigation-routing/root-layout-container-policy.png)

1. Create un nuovo criterio denominato **Struttura SPA**:

   ![Criteri struttura SPA](assets/navigation-routing/spa-policy-update.png)

   In **Componenti consentiti** > **Generale** > selezionare il componente **Contenitore di layout**.

   In **Componenti consentiti** > **WKND SPA REACT - STRUCTURE** > selezionare il componente **Header**:

   ![Seleziona componente intestazione](assets/navigation-routing/select-header-component.png)

   In **Componenti consentiti** > **WKND SPA REACT - Content** > selezionare i componenti **Image** e **Text**. È necessario selezionare 4 componenti totali.

   Fare clic su **Fine** per salvare le modifiche.

1. Aggiornare la pagina e aggiungere il componente **Intestazione** sopra il contenitore di layout non bloccato **Contenitore di layout**:

   ![aggiunta del componente Intestazione al modello](./assets/navigation-routing/add-header-component.gif)

1. Selezionare il componente **Header** e fare clic sull&#39;icona **Policy** per modificare il criterio.
1. Create un nuovo criterio con un **titolo del criterio** di **WKND SPA Header**.

   In **Proprietà**:

   * Impostare la **radice di navigazione** su `/content/wknd-spa-react/us/en`.
   * Impostare **Exclude Root Levels** su **1**.
   * Deselezionare **Raccolta di tutte le pagine figlie**.
   * Impostare la **Profondità struttura di navigazione** su **3**.

   ![Configurare i criteri di intestazione](assets/navigation-routing/header-policy.png)

   Questo raccoglierà i 2 livelli di navigazione in profondità sotto `/content/wknd-spa-react/us/en`.

1. Dopo aver salvato le modifiche, è necessario visualizzare il `Header` popolato come parte del modello:

   ![Componente intestazione compilata](assets/navigation-routing/populated-header.png)

## Crea pagine figlie

Quindi, create ulteriori pagine in AEM che fungeranno da diverse viste del SPA. Verranno inoltre analizzati la struttura gerarchica del modello JSON fornito da AEM.

1. Passate alla console **Siti**: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Selezionare la pagina iniziale **WKND SPA React Home Page** e fare clic su **Create** > **Page**:

   ![Crea nuova pagina](assets/navigation-routing/create-new-page.png)

1. In **Template** selezionare **SPA Page**. In **Proprietà** immettere **Pagina 1** come nome per **Titolo** e **page-1**.

   ![Immettere le proprietà della pagina iniziale](assets/navigation-routing/initial-page-properties.png)

   Fare clic su **Crea** e, nella finestra di dialogo a comparsa, fare clic su **Apri** per aprire la pagina nell&#39;editor SPA AEM.

1. Aggiungere un nuovo componente **Testo** al contenitore di layout principale **Contenitore di layout**. Modificate il componente e inserite il testo: **Pagina 1** utilizzando l&#39;editor Rich Text e l&#39;elemento **H1** (sarà necessario passare alla modalità a schermo intero per modificare gli elementi di paragrafo)

   ![Contenuto di esempio pagina 1](assets/navigation-routing/page-1-sample-content.png)

   Potete aggiungere contenuti aggiuntivi, come un’immagine.

1. Tornate alla console  AEM Sites e ripetete i passaggi descritti sopra, creando una seconda pagina denominata **Page 2** come elemento di pari livello di **Page 1**.
1. Creare infine una terza pagina, **Pagina 3** ma come **figlio** di **Pagina 2**. Una volta completata la gerarchia del sito dovrebbe essere simile a quella riportata di seguito:

   ![Gerarchia del sito di esempio](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. In una nuova scheda, apri l&#39;API del modello JSON fornita da AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Questo contenuto JSON viene richiesto al primo caricamento del SPA. La struttura esterna si presenta come segue:

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

   In `:children` dovrebbe essere visualizzata una voce per ciascuna delle pagine create. Il contenuto per tutte le pagine è in questa richiesta JSON iniziale. Una volta implementato il ciclo di navigazione, le visualizzazioni successive del SPA verranno caricate rapidamente, poiché il contenuto è già disponibile sul lato client.

   Non è saggio caricare **ALL** del contenuto di un SPA nella richiesta JSON iniziale, in quanto ciò rallenterebbe il caricamento della pagina iniziale. Nella sezione che segue viene illustrato come vengono raccolte le profondità di pagine con l’ereditarietà.

1. Andate al modello **SPA Root** all&#39;indirizzo: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Fare clic sul menu **Proprietà pagina** > **Criteri pagina**:

   ![Aprire il criterio pagina per SPA radice](assets/navigation-routing/open-page-policy.png)

1. Il modello **SPA Root** dispone di una scheda **Struttura gerarchica** supplementare per controllare il contenuto JSON raccolto. La **Profondità struttura** determina la profondità nella gerarchia del sito per la raccolta delle pagine figlie sotto la **radice**. È inoltre possibile utilizzare il campo **Struttura Pattern** per filtrare ulteriori pagine in base a un&#39;espressione regolare.

   Aggiornare **Profondità struttura** a **2**:

   ![Aggiorna profondità struttura](assets/navigation-routing/update-structure-depth.png)

   Fate clic su **Fine** per salvare le modifiche al criterio.

1. Riaprite il modello JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

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

   Tenere presente che il percorso **Pagina 3** è stato rimosso: `/content/wknd-spa-react/us/en/home/page-2/page-3` dal modello JSON iniziale.

   In seguito verrà illustrato come l’SDK dell’editor SPA AEM possa caricare in modo dinamico contenuti aggiuntivi.

## Implementare la navigazione

Implementate quindi il menu di navigazione come parte del `Header`. Potremmo aggiungere il codice direttamente in `Header.js`, ma una procedura migliore è evitare componenti di grandi dimensioni. Al contrario, implementeremo un componente `Navigation` SPA che potrebbe essere riutilizzato in un secondo momento.

1. Rivedete il JSON esposto dal componente AEM `Header` all&#39;indirizzo [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

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

   La natura gerarchica delle pagine AEM è modellata nel JSON che può essere utilizzato per compilare un menu di navigazione. Ricordate che il componente `Header` eredita tutte le funzionalità del componente [core di navigazione](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) e che il contenuto esposto tramite JSON sarà automaticamente mappato su proprietà React.

1. Aprite una nuova finestra del terminale e andate alla cartella `ui.frontend` del progetto SPA. Avviare il **webpack-dev-server** con il comando `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Aprite una nuova scheda del browser e andate a [http://localhost:3000/](Http://localhost:3000/).

   Il **webpack-dev-server** deve essere configurato per il proxy del modello JSON da un&#39;istanza locale di AEM (`ui.frontend/.env.development`). Questo ci consentirà di eseguire direttamente il codice rispetto al contenuto creato in AEM nell&#39;esercizio precedente. Accertatevi di essere autenticati in AEM nella stessa sessione di navigazione.

   ![menu attivato](./assets/navigation-routing/nav-toggle-static.gif)

   Al momento la funzionalità `Header` dispone della funzionalità di attivazione/disattivazione del menu già implementata. Implementate quindi il menu di navigazione.

1. Tornate all&#39;IDE di vostra scelta e aprite il percorso `Header.js` in `ui.frontend/src/components/Header/Header.js`.
1. Aggiornate il metodo `homeLink()` per rimuovere la stringa codificata e utilizzare i prop dinamici passati dal componente AEM:

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

   Il codice riportato sopra comporrà un URL basato sull’elemento di navigazione principale configurato dal componente. `homeLink()` viene utilizzato per compilare il logo nel  `logo()` metodo e utilizzato per determinare se il pulsante Indietro deve essere visualizzato in  `backButton()`.

   Salvare le modifiche in `Header.js`.

1. Aggiungete una riga nella parte superiore di `Header.js` per importare il componente `Navigation` sotto le altre importazioni:

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. Aggiornare quindi il metodo `get navigation()` per creare un&#39;istanza del componente `Navigation`:

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Come già detto, invece di implementare la navigazione all&#39;interno del `Header`, implementeremo la maggior parte della logica nel componente `Navigation`.  Le proprietà di `Header` includono la struttura JSON necessaria per creare il menu, passiamo tutte le proprietà.
1. Aprire il file `Navigation.js` in `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implementare il metodo `renderGroupNav(children)`:

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

   Questo metodo prende un array di elementi di navigazione, `children`, e crea un elenco non ordinato. Quindi esegue un&#39;iterazione sull&#39;array e passa l&#39;elemento alla `renderNavItem`, che verrà implementata successivamente.

1. Implementa la `renderNavItem`:

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

   Questo metodo esegue il rendering di una voce di elenco, con classi CSS basate sulle proprietà `level` e `active`. Il metodo quindi chiama `renderLink` per creare il tag di ancoraggio. Poiché il contenuto `Navigation` è gerarchico, viene utilizzata una strategia ricorsiva per chiamare `renderGroupNav` per gli elementi secondari dell&#39;elemento corrente.

1. Implementare il metodo `renderLink`:

   Aggiungete un metodo di importazione per il componente [Link](https://reacttraining.com/react-router/web/api/Link), parte del router React, nella parte superiore del file con le altre importazioni:

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

   Invece di un normale tag di ancoraggio, viene utilizzato il componente [Link](https://reacttraining.com/react-router/web/api/Link). `<a>` In questo modo l’aggiornamento completo della pagina non viene attivato e si basa invece sul router React fornito dall’SDK JS dell’editor SPA AEM.

1. Salvare le modifiche in `Navigation.js` e tornare al **webpack-dev-server**: [http://localhost:3000](Http://localhost:3000)

   ![Navigazione intestazione completata](assets/navigation-routing/completed-header.png)

   Aprite la navigazione facendo clic sul menu e visualizzate i collegamenti di navigazione compilati. Dovrebbe essere possibile passare a diverse viste del SPA.

##  Inspect al ciclo di SPA

Ora che la navigazione è stata implementata, ispezionare il routing in AEM.

1. Nell&#39;IDE aprire il file `index.js` in `ui.frontend/src/index.js`.

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

   Tenere presente che il `App` è racchiuso nel componente `Router` da [React Router](https://reacttraining.com/react-router/). Il `ModelManager`, fornito dall&#39;SDK JS dell&#39;editor SPA AEM, aggiunge i percorsi dinamici a AEM pagine basate sull&#39;API del modello JSON.

1. Apri un terminale, vai alla radice del progetto e distribuisci il progetto per AEM utilizzando le tue abilità Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Andate alla pagina iniziale SPA in AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) e aprire gli strumenti di sviluppo del browser. Le schermate riportate di seguito sono acquisite dal browser Google Chrome.

   Aggiorna la pagina e dovresti visualizzare una richiesta XHR a `/content/wknd-spa-react/us/en.model.json`, che è la radice SPA. Si noti che solo tre pagine figlie sono incluse in base alla configurazione della profondità gerarchica per il modello SPA Root creato in precedenza nell&#39;esercitazione. Questo non include **Pagina 3**.

   ![Richiesta JSON iniziale - SPA radice](assets/navigation-routing/initial-json-request.png)

1. Con gli strumenti di sviluppo aperti, utilizzate la navigazione `Header` per passare a **Pagina 3**:

   ![Navigazione pagina 3](assets/navigation-routing/page-three-navigation.png)

   Tenere presente che una nuova richiesta XHR è eseguita per: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Pagina tre richiesta XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager è consapevole del fatto che il contenuto JSON **Page 3** non è disponibile e attiva automaticamente la richiesta XHR aggiuntiva.

1. Continuate a navigare nel SPA utilizzando i vari collegamenti di navigazione del componente `Header`. Osservate che non vengono effettuate ulteriori richieste XHR e che non si verificano aggiornamenti di pagina completi. Questo rende il SPA veloce per l&#39;utente finale e riduce le richieste non necessarie al AEM.

   ![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

1. Sperimenta con collegamenti profondi navigando direttamente a: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Tenere presente che il pulsante Indietro del browser continua a funzionare.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato in che modo è possibile supportare più viste nel SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica è stata implementata utilizzando React Router e aggiunta al componente `Header`.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) o estrarre il codice localmente passando al ramo `React/navigation-routing-solution`.
