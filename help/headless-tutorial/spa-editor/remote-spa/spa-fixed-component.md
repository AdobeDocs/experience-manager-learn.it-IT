---
title: Aggiungere componenti fissi modificabili a un SPA remoto
description: Scopri come aggiungere componenti fissi modificabili a un SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 1%

---

# Componenti fissi modificabili

I componenti React modificabili possono essere &quot;fissi&quot; o codificati nelle viste SPA. Questo consente agli sviluppatori di inserire SPA componenti compatibili con l’editor nelle viste di SPA e permette agli utenti di creare il contenuto dei componenti in AEM SPA Editor.

![Componenti fissi](./assets/spa-fixed-component/intro.png)

In questo capitolo, sostituiamo il titolo della visualizzazione Home, &quot;Current Adventures&quot;, che è un testo codificato in `Home.js` con un componente Titolo fisso ma modificabile. I componenti fissi garantiscono il posizionamento del titolo, ma consentono anche di creare il testo del titolo e di modificarlo al di fuori del ciclo di sviluppo.

## Aggiornare l’app WKND

Per aggiungere una __Fisso__ nella vista Home:

+ Importa il componente Titolo componente di base AEM React e lo registra nel tipo di risorsa del titolo del progetto
+ Posiziona il componente Titolo modificabile nella vista Home SPA

### Importa nel componente Titolo del componente di base AEM React

Nella vista Home SPA, sostituisci il testo hardcoded `<h2>Current Adventures</h2>` con il componente Titolo AEM React Core Components’. Prima di poter utilizzare il componente Titolo , è necessario:

1. Importa il componente Titolo da `@adobe/aem-core-components-react-base`
1. Registralo utilizzando `withMappable` in modo che gli sviluppatori possano inserirla nel SPA
1. Inoltre, registrati con `MapTo` può essere utilizzato in [componente contenitore in seguito](./spa-container-component.md).

Per effettuare questo collegamento:

1. Apri progetto di SPA remoto in `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` nell’IDE
1. Crea un componente React in `react-app/src/components/aem/AEMTitle.js`
1. Aggiungi il codice seguente a `AEMTitle.js`.

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

Leggi i commenti del codice per i dettagli di implementazione.

La `AEMTitle.js` dovrebbe essere simile a:

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### Utilizzare il componente React AEMTitle

Ora che il componente Titolo del componente core React AEM è registrato e disponibile per l’uso nell’app React, sostituisci il testo del titolo codificato nella vista Home.

1. Modifica `react-app/src/Home.js`
1. In `Home()` in basso, sostituisci il titolo hardcoded con il nuovo `AEMTitle` componente:

   ```
   <h2>Current Adventures</h2>
   ```

   con

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   Aggiorna `Home.js` con il seguente codice:

   ```
   ...
   import { AEMTitle } from './aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

La `Home.js` dovrebbe essere simile a:

![Home.js](./assets/spa-fixed-component/home-js.png)

## Creare il componente Titolo in AEM

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND__
1. Tocca __Pagina principale__ e seleziona __Modifica__ dalla barra delle azioni superiore
1. Seleziona __Modifica__ dal selettore della modalità di modifica in alto a destra dell’Editor pagina
1. Passa il puntatore del mouse sul testo del titolo predefinito sotto il logo WKND e sopra l’elenco delle avventure, fino a quando non viene visualizzata la struttura di modifica blu
1. Tocca per esporre la barra delle azioni del componente, quindi tocca __chiave__  per modificare

   ![Barra delle azioni del componente titolo](./assets/spa-fixed-component/title-action-bar.png)

1. Crea il componente Titolo :
   + Titolo: __Avventure WKND__
   + Tipo/Dimensione: __H2__

      ![Finestra di dialogo del componente titolo](./assets/spa-fixed-component/title-dialog.png)

1. Tocca __Fine__ per salvare
1. Anteprima delle modifiche in AEM Editor SPA
1. Aggiorna l’app WKND in esecuzione localmente su [http://localhost:3000](Http://localhost:3000) e vedere le modifiche al titolo create immediatamente applicate.

   ![Componente titolo in SPA](./assets/spa-fixed-component/title-final.png)

## Congratulazioni!

Hai aggiunto un componente fisso e modificabile all’app WKND. Ora sai come:

+ Importare da e riutilizzare un componente core React AEM nel SPA
+ Aggiungi un componente fisso ma modificabile al SPA
+ Crea il componente fisso in AEM
+ Visualizzare il contenuto creato in SPA remoto

## Passaggi successivi

I passaggi successivi sono i seguenti: [aggiungi un componente contenitore AEM responsiveGrid](./spa-container-component.md) al SPA che consente all’autore di aggiungere e modificare componenti al SPA.
