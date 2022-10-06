---
title: Aggiungere componenti di contenitori modificabili a un SPA remoto
description: Scopri come aggiungere componenti contenitore modificabili a un SPA remoto che consente AEM autori di trascinarvi componenti.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 2%

---

# Componenti contenitore modificabili

[Componenti fissi](./spa-fixed-component.md) offre una certa flessibilità per l’authoring dei contenuti SPA, tuttavia questo approccio è rigido e richiede agli sviluppatori di definire con precisione la composizione dei contenuti modificabili. Per supportare la creazione di esperienze eccezionali da parte degli autori, SPA Editor supporta l’utilizzo di componenti contenitore nella SPA. I componenti contenitore consentono agli autori di trascinare e rilasciare i componenti consentiti nel contenitore e di crearli, proprio come nelle tradizionali funzioni di authoring di AEM Sites.

![Componenti contenitore modificabili](./assets/spa-container-component/intro.png)

In questo capitolo, aggiungiamo un contenitore modificabile alla vista home che consente agli autori di comporre e creare il layout di esperienze di contenuti avanzati utilizzando AEM React Core Components direttamente nel SPA.

## Aggiornare l’app WKND

Per aggiungere un componente contenitore alla vista Home:

+ Importa il componente ResponsiveGrid del componente AEM React Modificable
+ Importare e registrare AEM React Core Components (Testo e Immagine) per l’utilizzo nel componente contenitore

### Importare nel componente contenitore ResponsiveGrid

Per inserire un’area modificabile nella vista Home, è necessario:

1. Importa il componente ResponsiveGrid da `@adobe/aem-react-editable-components`
1. Registralo utilizzando `withMappable` in modo che gli sviluppatori possano inserirla nel SPA
1. Inoltre, registrati con `MapTo` può quindi essere riutilizzato in altri componenti Container, nidificando in modo efficace i contenitori.

Per effettuare questo collegamento:

1. Apri il progetto SPA nell’IDE
1. Crea un componente React in `src/components/aem/AEMResponsiveGrid.js`
1. Aggiungi il codice seguente a `AEMResponsiveGrid.js`

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

Il codice è simile `AEMTitle.js` che [importava il componente Titolo dei componenti core AEM reach](./spa-fixed-component.md).


La `AEMResponsiveGrid.js` dovrebbe essere simile a:

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### Utilizzare il componente SPA AEMResponsiveGrid

Ora che AEM componente ResponsiveGrid è registrato e disponibile per l’uso all’interno del SPA, possiamo inserirlo nella vista Home.

1. Apri e modifica `react-app/src/Home.js`
1. Importa `AEMResponsiveGrid` e posizionarlo sopra il `<AEMTitle ...>` componente.
1. Imposta i seguenti attributi nel `<AEMResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Questo istruisce il `AEMResponsiveGrid` per recuperare il relativo contenuto dalla risorsa AEM:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   La `itemPath` mappe `responsivegrid` nodo definito nel `Remote SPA Page` Modello AEM e viene creato automaticamente sulle nuove AEM Pagine create da `Remote SPA Page` Modello AEM.

   Aggiorna `Home.js` per aggiungere `<AEMResponsiveGrid...>` componente.

   ```
   ...
   import AEMResponsiveGrid from './aem/AEMResponsiveGrid';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <AEMResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
               <Adventures />
           </div>
       );
   }
   ```

La `Home.js` dovrebbe essere simile a:

![Home.js](./assets/spa-container-component/home-js.png)

## Creare componenti modificabili

Per ottenere l’effetto completo dei contenitori di esperienza di authoring flessibili forniti in SPA Editor. È già stato creato un componente Titolo modificabile, ma ne facciamo altri alcuni che consentono agli autori di utilizzare i componenti di base Testo e Immagine AEM WCM nel componente contenitore appena aggiunto.

### Componente testo

1. Apri il progetto SPA nell’IDE
1. Crea un componente React in `src/components/aem/AEMText.js`
1. Aggiungi il codice seguente a `AEMText.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

La `AEMText.js` dovrebbe essere simile a:

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### Componente immagine

1. Apri il progetto SPA nell’IDE
1. Crea un componente React in `src/components/aem/AEMImage.js`
1. Aggiungi il codice seguente a `AEMImage.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. Creare un file SCSS `src/components/aem/AEMImage.scss` che fornisce stili personalizzati per `AEMImage.scss`. Questi stili sono destinati alle classi CSS di notazione BEM del componente di base AEM React.
1. Aggiungi il seguente SCSS a `AEMImage.scss`

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importa `AEMImage.scss` in `AEMImage.js`

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

La `AEMImage.js` e `AEMImage.scss` dovrebbe essere simile a:

![AEMImage.js e AEMImage.scss](./assets/spa-container-component/aem-image-js-scss.png)

### Importare i componenti modificabili

La nuova creazione `AEMText` e `AEMImage` SPA componenti sono citati nella SPA e vengono create dinamicamente in base al JSON restituito da AEM. Per garantire che questi componenti siano disponibili per l’SPA, crea istruzioni di importazione per tali componenti in `Home.js`

1. Apri il progetto SPA nell’IDE
1. Aprire il file `src/Home.js`
1. Aggiungi istruzioni di importazione per `AEMText` e `AEMImage`

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


Il risultato dovrebbe essere simile al seguente:

![Home.js](./assets/spa-container-component/home-js-imports.png)

Se queste importazioni sono _not_ inoltre, `AEMText` e `AEMImage` Il codice non viene richiamato da SPA, pertanto i componenti non vengono registrati rispetto ai tipi di risorse specificati.

## Configurazione del contenitore in AEM

I componenti contenitore AEM utilizzano i criteri per dettare i componenti consentiti. Si tratta di una configurazione critica quando si utilizza SPA Editor, in quanto solo AEM componenti core WCM che hanno mappato SPA controparti dei componenti possono essere sottoposti a rendering dal SPA. Assicurati che siano consentiti solo i componenti per i quali abbiamo fornito SPA implementazioni:

+ `AEMTitle` mappato su `wknd-app/components/title`
+ `AEMText` mappato su `wknd-app/components/text`
+ `AEMImage` mappato su `wknd-app/components/image`

Per configurare il contenitore reponsivegrid del modello di pagina SPA remota:

1. Accedi ad AEM Author
1. Passa a __Strumenti > Generale > Modelli > App WKND__
1. Modifica __Pagina Report SPA__

   ![Criteri della griglia reattiva](./assets/spa-container-component/templates-remote-spa-page.png)

1. Seleziona __Struttura__ nel commutatore di modalità in alto a destra
1. Tocca per selezionare la __Contenitore di layout__
1. Tocca __Criterio__ icona nella barra a comparsa

   ![Criteri della griglia reattiva](./assets/spa-container-component/templates-policies-action.png)

1. Sulla destra, sotto il __Componenti consentiti__ scheda, espandi __APP WKND - CONTENUTO__
1. Assicurati che siano selezionati solo i seguenti elementi:
   + Immagine
   + Testo
   + Titolo

   ![Pagina SPA remota](./assets/spa-container-component/templates-allowed-components.png)

1. Toccate __Chiudi__

## Creazione del contenitore in AEM

Dopo l’aggiornamento del SPA per incorporare il `<AEMResponsiveGrid...>`, wrapper per tre componenti core React (`AEMTitle`, `AEMText`e `AEMImage`) e AEM viene aggiornato con un criterio Modello corrispondente, possiamo iniziare a creare contenuto nel componente contenitore .

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND__
1. Tocca __Pagina principale__ e seleziona __Modifica__ dalla barra delle azioni superiore
   + Viene visualizzato un componente Testo &quot;Ciao a tutti&quot;, che viene aggiunto automaticamente durante la generazione del progetto dall’archetipo AEM progetto
1. Seleziona __Modifica__ dal selettore modalità in alto a destra dell’Editor pagina
1. Individua il __Contenitore di layout__ area modificabile sotto il titolo
1. Apri __Barra laterale dell’Editor pagina__, quindi seleziona la __Vista Componenti__
1. Trascina i seguenti componenti nel __Contenitore di layout__
   + Immagine
   + Titolo
1. Trascina i componenti per riordinarli nell’ordine seguente:
   1. Titolo
   1. Immagine
   1. Testo
1. __Autore__ la __Titolo__ component
   1. Tocca il componente Titolo e tocca il __chiave__ icona a __modifica__ il componente Titolo
   1. Aggiungi il testo seguente:
      + Titolo: __L&#39;estate sta arrivando, sfruttiamo al massimo!__
      + Tipo: __H1__
   1. Toccate __Chiudi__
1. __Autore__ la __Immagine__ component
   1. Trascina un’immagine dalla barra laterale (dopo il passaggio alla visualizzazione Risorse) sul componente Immagine
   1. Tocca il componente Immagine e tocca il __chiave__ icona da modificare
   1. Controlla la __L&#39;immagine è decorativa__ spunta
   1. Toccate __Chiudi__
1. __Autore__ la __Testo__ component
   1. Per modificare il componente Testo , toccate il componente Testo e toccate il pulsante __chiave__ icona
   1. Aggiungi il testo seguente:
      + _In questo momento, è possibile ottenere il 15% su tutte le avventure di 1 settimana, e il 20% di sconto su tutte le avventure che sono 2 settimane o più! Al momento del pagamento, aggiungi il codice della campagna SOMMERISVENING per ottenere i tuoi sconti!_
   1. Toccate __Chiudi__

1. I componenti vengono ora creati, ma vengono sovrapposti verticalmente.

   ![Componenti creati](./assets/spa-container-component/authored-components.png)

   Utilizza AEM modalità Layout per modificare le dimensioni e il layout dei componenti.

1. Passa a __Modalità Layout__ utilizzo del selettore modalità in alto a destra
1. __Ridimensiona__ i componenti Immagine e Testo, in modo che siano affiancati
   + __Immagine__ dovrebbe essere __8 colonne__
   + __Testo__ dovrebbe essere __3 colonne__

   ![Componenti di layout](./assets/spa-container-component/layout-components.png)

1. __Anteprima__ le modifiche in AEM Editor pagina
1. Aggiorna l’app WKND in esecuzione localmente su [http://localhost:3000](Http://localhost:3000) per visualizzare le modifiche create!

   ![Componente contenitore in SPA](./assets/spa-container-component/localhost-final.png)


## Congratulazioni!

Hai aggiunto un componente contenitore che consente agli autori di aggiungere componenti modificabili all’app WKND. Ora sai come:

+ Utilizza il componente AEM React Modificable Component’s ResponsiveGrid nel SPA
+ Registra AEM React Core Components (Testo e Immagine) per l’utilizzo nell’SPA tramite il componente contenitore
+ Configurare il modello Pagina SPA remota per consentire i componenti core abilitati per SPA
+ Aggiungere componenti modificabili al componente contenitore
+ Componenti di authoring e layout nell’Editor SPA

## Passaggi successivi

Il passaggio successivo utilizza la stessa tecnica per [aggiungi un componente modificabile a un percorso Dettagli avventura](./spa-dynamic-routes.md) nel SPA.
