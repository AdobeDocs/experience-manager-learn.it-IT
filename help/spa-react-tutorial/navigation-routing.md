---
title: Aggiunta di navigazione e routing | Guida introduttiva all'editor SPA AEM e React
description: Scoprite come è possibile supportare più viste nell’area SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando React Router e aggiunta a un componente Intestazione esistente.
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

Scoprite come è possibile supportare più viste nell’area SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica viene implementata utilizzando React Router e aggiunta a un componente Intestazione esistente.

## Obiettivo

1. Comprendere le opzioni di routing del modello SPA disponibili quando si utilizza SPA Editor.
1. Scoprite come utilizzare [React Router](https://reacttraining.com/react-router/) per navigare tra le diverse viste dell&#39;SPA.
1. Implementa una navigazione dinamica guidata dalla gerarchia di pagine AEM.

## Cosa verrà creato

Questo capitolo aggiunge un menu di navigazione a un `Header` componente esistente. Il menu di navigazione sarà guidato dalla gerarchia di pagine AEM e utilizzerà il modello JSON fornito dal componente [core di](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)navigazione.

![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

## Prerequisiti

Esaminare le istruzioni e gli strumenti necessari per configurare un ambiente [di sviluppo](overview.md#local-dev-environment)locale.

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

   Se utilizzate [AEM 6.x](overview.md#compatibility) , aggiungete il `classic` profilo:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Installate il pacchetto finito per il sito [di riferimento](https://github.com/adobe/aem-guides-wknd/releases/latest)WKND tradizionale. Le immagini fornite dal sito [di riferimento](https://github.com/adobe/aem-guides-wknd/releases/latest) WKND verranno riutilizzate nell&#39;SPA WKND. Il pacchetto può essere installato tramite [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) o estrarre il codice localmente passando al ramo `React/navigation-routing-solution`.

##  aggiornamenti di intestazione Inspect {#inspect-header}

Nei capitoli precedenti, il `Header` componente era stato aggiunto come componente React puro incluso tramite `App.js`. In questo capitolo, il `Header` componente è stato rimosso e verrà aggiunto tramite l’Editor [](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)modelli. Questo consentirà agli utenti di configurare il menu di navigazione del `Header` modulo dall&#39;interno del AEM.

>[!NOTE]
>
> Diversi aggiornamenti CSS e JavaScript sono già stati apportati alla base di codice per avviare questo capitolo. Per concentrarsi sui concetti di base, non vengono discusse **tutte** le modifiche del codice. Potete visualizzare le modifiche complete [qui](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start).

1. Nell&#39;IDE di vostra scelta, aprite il progetto di avvio SPA per questo capitolo.
1. Sotto il `ui.frontend` modulo ispezionare il file `Header.js` : `ui.frontend/src/components/Header/Header.js`.

   Sono stati eseguiti diversi aggiornamenti, tra cui l’aggiunta di un componente `HeaderEditConfig` e di un `MapTo` componente per consentirne la mappatura a un componente AEM `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. Nel `ui.apps` modulo, esamina la definizione del componente del AEM `Header` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   Il `Header` componente AEM erediterà tutte le funzionalità del componente [core di](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) navigazione tramite la `sling:resourceSuperType` proprietà.

## Aggiungere l&#39;intestazione al modello {#add-header-template}

1. Aprite un browser e accedete a AEM, [http://localhost:4502/](Http://localhost:4502/). La base di codice iniziale dovrebbe essere già distribuita.
1. Andate al modello **di pagina** SPA: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Selezionate il contenitore **di layout** principale più esterno e fate clic sull&#39;icona **Criterio** . Fate attenzione a **non** selezionare il Contenitore **di** layout non bloccato per la creazione.

   ![Selezionate l&#39;icona del criterio contenitore del layout principale](assets/navigation-routing/root-layout-container-policy.png)

1. Create un nuovo criterio denominato Struttura **** SPA:

   ![Criteri di struttura SPA](assets/navigation-routing/spa-policy-update.png)

   In Componenti **consentiti** > **Generale** > selezionare il componente Contenitore **di** layout.

   In Componenti **** consentiti > REATTO SPA **WKND - STRUTTURA** > selezionare il componente **Intestazione** :

   ![Seleziona componente intestazione](assets/navigation-routing/select-header-component.png)

   In Componenti **** consentiti > REATTA SPA **WKND - Contenuto** > selezionare i componenti **Immagine** e **Testo** . È necessario selezionare 4 componenti totali.

   Click **Done** to save the changes.

1. Aggiornate la pagina e aggiungete il componente **Intestazione** sopra il contenitore **di** layout non bloccato:

   ![aggiunta del componente Intestazione al modello](./assets/navigation-routing/add-header-component.gif)

1. Selezionate il componente **Intestazione** e fate clic sull&#39;icona **Criterio** corrispondente per modificare il criterio.
1. Create un nuovo criterio con un Titolo **** criterio di intestazione **SPA** WKND.

   In **Proprietà**:

   * Impostare la directory principale **di** navigazione su `/content/wknd-spa-react/us/en`.
   * Impostate **Escludi livelli** radice su **1**.
   * Uncheck **Collect all child pages**.
   * Impostate la profondità **della struttura di** navigazione su **3**.

   ![Configurare i criteri di intestazione](assets/navigation-routing/header-policy.png)

   Questo raccoglierà i 2 livelli di navigazione in profondità sotto `/content/wknd-spa-react/us/en`.

1. Dopo aver salvato le modifiche, è necessario visualizzare il popolato `Header` come parte del modello:

   ![Componente intestazione compilata](assets/navigation-routing/populated-header.png)

## Crea pagine figlie

Create quindi altre pagine in AEM che fungano da diverse viste nell’area di protezione. Verranno inoltre analizzati la struttura gerarchica del modello JSON fornito da AEM.

1. Passate alla console **Siti** : [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Selezionate la home page **di Reazione SPA** WKND e fate clic su **Crea** > **Pagina**:

   ![Crea nuova pagina](assets/navigation-routing/create-new-page.png)

1. In **Modello** selezionate Pagina **** SPA. In **Proprietà** immettete **Pagina 1** come nome il **Titolo** e **pagina 1** .

   ![Immettere le proprietà della pagina iniziale](assets/navigation-routing/initial-page-properties.png)

   Fate clic su **Crea** e, nella finestra di dialogo a comparsa, fate clic su **Apri** per aprire la pagina nell’Editor AEM SPA.

1. Aggiungete un nuovo componente **Testo** al contenitore **di** layout principale. Modificate il componente e inserite il testo: **Pagina 1** con l’editor Rich Text e l’elemento **H1** (per modificare gli elementi di paragrafo dovrete passare alla modalità a schermo intero)

   ![Contenuto di esempio pagina 1](assets/navigation-routing/page-1-sample-content.png)

   Potete aggiungere contenuti aggiuntivi, come un’immagine.

1. Tornate alla console  AEM Sites e ripetete i passaggi precedenti, creando una seconda pagina denominata **Pagina 2** come pari a **Pagina 1**.
1. Infine, create una terza pagina, **Pagina 3** , ma come **figlia** di **Pagina 2**. Una volta completata la gerarchia del sito dovrebbe essere simile a quella riportata di seguito:

   ![Gerarchia del sito di esempio](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. In una nuova scheda, apri l&#39;API del modello JSON fornita da AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Questo contenuto JSON viene richiesto al primo caricamento dell&#39;SPA. La struttura esterna si presenta come segue:

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

   In `:children` questa sezione viene visualizzata una voce per ciascuna pagina creata. Il contenuto per tutte le pagine è in questa richiesta JSON iniziale. Una volta implementato il routing di navigazione, le viste successive dell&#39;SPA verranno caricate rapidamente, dal momento che il contenuto è già disponibile sul lato client.

   Non è saggio caricare **TUTTO** il contenuto di un&#39;app SPA nella richiesta JSON iniziale, in quanto ciò rallenterebbe il caricamento iniziale della pagina. Nella sezione che segue viene illustrato come vengono raccolte le profondità di pagine con l’ereditarietà.

1. Andate al modello **SPA Root** in: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Fare clic sul menu **Proprietà** pagina > Criterio **** pagina:

   ![Aprire i criteri di pagina per SPA Root](assets/navigation-routing/open-page-policy.png)

1. Il modello **SPA Root** dispone di una scheda Struttura **** gerarchica aggiuntiva per controllare il contenuto JSON raccolto. La profondità **** struttura determina la profondità nella gerarchia del sito per la raccolta delle pagine figlie sotto la **radice**. È inoltre possibile utilizzare il campo Pattern **** struttura per filtrare ulteriori pagine in base a un&#39;espressione regolare.

   Aggiorna la profondità della **struttura** a **2**:

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

   Il percorso **Pagina 3** è stato rimosso: `/content/wknd-spa-react/us/en/home/page-2/page-3` dal modello JSON iniziale.

   In seguito verrà illustrato come l’SDK dell’editor SPA AEM possa caricare in modo dinamico contenuti aggiuntivi.

## Implementare la navigazione

Quindi, implementate il menu di navigazione come parte del `Header`. Potremmo aggiungere il codice direttamente in `Header.js` ma una procedura migliore è evitare componenti di grandi dimensioni. Al contrario, implementeremo un componente `Navigation` SPA che potrebbe essere riutilizzato in un secondo momento.

1. Rivedete il JSON esposto dal `Header` componente AEM all’indirizzo [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

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

   La natura gerarchica delle pagine AEM è modellata nel JSON che può essere utilizzato per compilare un menu di navigazione. Ricorda che il `Header` componente eredita tutte le funzionalità del componente [core di](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) navigazione e che il contenuto esposto tramite JSON verrà mappato automaticamente sulle proprietà React.

1. Aprite una nuova finestra del terminale e andate alla `ui.frontend` cartella del progetto SPA. Avviare il **webpack-dev-server** con il comando `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Aprite una nuova scheda del browser e andate a [http://localhost:3000/](Http://localhost:3000/).

   Il **webpack-dev-server** deve essere configurato per il proxy del modello JSON da un&#39;istanza locale di AEM (`ui.frontend/.env.development`). Questo ci consentirà di eseguire direttamente il codice rispetto al contenuto creato in AEM nell&#39;esercizio precedente. Accertatevi di essere autenticati in AEM nella stessa sessione di navigazione.

   ![menu attivato](./assets/navigation-routing/nav-toggle-static.gif)

   Al `Header` momento la funzionalità di attivazione/disattivazione del menu è già implementata. Implementate quindi il menu di navigazione.

1. Tornate all&#39;IDE di vostra scelta e aprite la `Header.js` pagina `ui.frontend/src/components/Header/Header.js`.
1. Aggiornare il `homeLink()` metodo per rimuovere la stringa codificata e utilizzare i prop dinamici passati dal componente AEM:

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

   Il codice riportato sopra comporrà un URL basato sull’elemento di navigazione principale configurato dal componente. `homeLink()` viene utilizzato per compilare il logo nel `logo()` metodo e utilizzato per determinare se il pulsante Indietro deve essere visualizzato in `backButton()`.

   Save changes to `Header.js`.

1. Aggiungete una riga nella parte superiore di `Header.js` per importare il `Navigation` componente sotto le altre importazioni:

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. Aggiornare quindi il `get navigation()` metodo per creare un&#39;istanza del `Navigation` componente:

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Come già detto, invece di implementare la navigazione all&#39;interno del `Header` componente, implementeremo la maggior parte della logica nel `Navigation` componente.  I prop della struttura JSON `Header` includono la struttura JSON necessaria per costruire il menu, passiamo tutti i prop.
1. Open the file `Navigation.js` at `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implementare il `renderGroupNav(children)` metodo:

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

   Questo metodo prende un array di elementi di navigazione `children`e crea un elenco non ordinato. Quindi esegue un&#39;iterazione sull&#39;array e passa l&#39;elemento all&#39;elemento `renderNavItem`, che verrà implementato successivamente.

1. Implementa i `renderNavItem`seguenti elementi:

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

   Questo metodo esegue il rendering di una voce di elenco, con classi CSS basate su proprietà `level` e `active`. Il metodo quindi chiama `renderLink` per creare il tag di ancoraggio. Poiché il `Navigation` contenuto è gerarchico, viene utilizzata una strategia ricorsiva per chiamare gli elementi secondari `renderGroupNav` dell&#39;elemento corrente.

1. Implementare il `renderLink` metodo:

   Aggiungere un metodo di importazione per il componente [Link](https://reacttraining.com/react-router/web/api/Link) , parte del router React, nella parte superiore del file con le altre importazioni:

   ```js
   import {Link} from "react-router-dom";
   ```

   Completare l&#39;implementazione del `renderLink` metodo:

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   Invece di un normale tag di ancoraggio, `<a>`viene utilizzato il componente [Collegamento](https://reacttraining.com/react-router/web/api/Link) . In questo modo l’aggiornamento completo della pagina non viene attivato e si basa invece sul router React fornito dall’SDK JS AEM SPA Editor.

1. Salva le modifiche `Navigation.js` e torna al **webpack-dev-server**: [http://localhost:3000](Http://localhost:3000)

   ![Navigazione intestazione completata](assets/navigation-routing/completed-header.png)

   Aprite la navigazione facendo clic sul menu e visualizzate i collegamenti di navigazione compilati. È necessario essere in grado di passare a diverse viste dell&#39;SPA.

##  il ciclo SPA di Inspect

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

   Il `App` componente viene racchiuso nel `Router` componente dal [router](https://reacttraining.com/react-router/)React. Il `ModelManager`, fornito dall’SDK JS AEM SPA Editor, aggiunge i percorsi dinamici a AEM Pagine basate sull’API del modello JSON.

1. Apri un terminale, vai alla radice del progetto e distribuisci il progetto per AEM utilizzando le tue abilità Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Andate alla pagina iniziale SPA in AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) e aprire gli strumenti di sviluppo del browser. Le schermate riportate di seguito sono acquisite dal browser Google Chrome.

   Aggiorna la pagina e dovresti vedere una richiesta XHR a `/content/wknd-spa-react/us/en.model.json`, che è la directory principale dell&#39;SPA. Si noti che solo tre pagine figlie sono incluse in base alla configurazione della profondità gerarchica del modello SPA Root creata in precedenza nell&#39;esercitazione. Questa operazione non include la **pagina 3**.

   ![Richiesta JSON iniziale - SPA Root](assets/navigation-routing/initial-json-request.png)

1. Con gli strumenti di sviluppo aperti, utilizzate la `Header` navigazione per passare alla **pagina 3**:

   ![Navigazione pagina 3](assets/navigation-routing/page-three-navigation.png)

   Tenere presente che una nuova richiesta XHR è eseguita per: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Pagina tre richiesta XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager è consapevole del fatto che il contenuto JSON della **pagina 3** non è disponibile e attiva automaticamente la richiesta XHR aggiuntiva.

1. Continuate a navigare nell’area SPA utilizzando i vari collegamenti di navigazione del `Header` componente. Osservate che non vengono effettuate ulteriori richieste XHR e che non si verificano aggiornamenti di pagina completi. Questo rende l&#39;SPA veloce per l&#39;utente finale e riduce le richieste non necessarie al AEM.

   ![Navigazione implementata](assets/navigation-routing/final-navigation-implemented.gif)

1. Sperimenta con collegamenti profondi navigando direttamente a: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Tenere presente che il pulsante Indietro del browser continua a funzionare.

## Congratulazioni! {#congratulations}

Congratulazioni, hai imparato in che modo è possibile supportare più viste nell’SPA mappando AEM pagine con l’SDK dell’editor SPA. La navigazione dinamica è stata implementata utilizzando React Router e aggiunta al `Header` componente.

È sempre possibile visualizzare il codice finito su [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) o estrarre il codice localmente passando al ramo `React/navigation-routing-solution`.
