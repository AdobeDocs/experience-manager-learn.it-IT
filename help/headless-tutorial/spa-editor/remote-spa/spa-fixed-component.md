---
title: Aggiungere componenti fissi modificabili a un SPA remoto
description: Scopri come aggiungere componenti fissi modificabili a un SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# Componenti fissi modificabili

I componenti React modificabili possono essere &quot;fissi&quot; o codificati nelle viste SPA. Questo consente agli sviluppatori di inserire SPA componenti compatibili con l’editor nelle viste di SPA e permette agli utenti di creare il contenuto dei componenti in AEM SPA Editor.

![Componenti fissi](./assets/spa-fixed-component/intro.png)

In questo capitolo, sostituiamo il titolo della visualizzazione Home, &quot;Current Adventures&quot;, che è un testo codificato in `Home.js` con un componente Titolo fisso ma modificabile. I componenti fissi garantiscono il posizionamento del titolo, ma consentono anche di creare il testo del titolo e di modificarlo al di fuori del ciclo di sviluppo.

## Aggiornare l’app WKND

Per aggiungere una __Fisso__ nella vista Home:

+ Crea un componente Titolo modificabile personalizzato e registralo nel tipo di risorsa del Titolo del progetto
+ Posiziona il componente Titolo modificabile nella vista Home SPA

### Creare un componente Titolo reazione modificabile

Nella vista Home SPA, sostituisci il testo hardcoded `<h2>Current Adventures</h2>` con un componente Titolo modificabile personalizzato. Prima di poter utilizzare il componente Titolo , è necessario:

1. Creare un componente personalizzato per la reazione di un titolo
1. Decorare il componente Titolo personalizzato utilizzando i metodi di `@adobe/aem-react-editable-components` per renderlo modificabile.
1. Registra il componente Titolo modificabile con `MapTo` può essere utilizzato in [componente contenitore in seguito](./spa-container-component.md).

Per effettuare questo collegamento:

1. Apri progetto di SPA remoto in `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` nell’IDE
1. Crea un componente React in `react-app/src/components/editable/core/Title.js`
1. Aggiungi il codice seguente a `Title.js`.

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   Tieni presente che questo componente React non è ancora modificabile utilizzando AEM editor di SPA. Questo componente di base sarà reso modificabile nel passaggio successivo.

   Leggi i commenti del codice per i dettagli di implementazione.

1. Crea un componente React in `react-app/src/components/editable/EditableTitle.js`
1. Aggiungi il codice seguente a `EditableTitle.js`.

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   Questo `EditableTitle` Il componente React racchiude il `Title` Reagire componente, avvolgerlo e decorarlo per essere modificabile in AEM Editor SPA.

### Utilizzare il componente React EditableTitle

Ora che il componente React EditableTitle è registrato e disponibile per l’uso nell’app React, sostituisci il testo del titolo codificato nella vista Home.

1. Modifica `react-app/src/components/Home.js`
1. In `Home()` in basso, importa `EditableTitle` e sostituisci il titolo codificato con il nuovo `AEMTitle` componente:

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

La `Home.js` dovrebbe essere simile a:

![Home.js](./assets/spa-fixed-component/home-js-update.png)

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

## Congratulazioni. 

Hai aggiunto un componente fisso e modificabile all’app WKND. Ora sai come:

+ È stato creato un componente fisso ma modificabile nella SPA
+ Crea il componente fisso in AEM
+ Visualizzare il contenuto creato in SPA remoto

## Passaggi successivi

I passaggi successivi sono i seguenti: [aggiungi un componente contenitore AEM responsiveGrid](./spa-container-component.md) al SPA che consente all’autore di aggiungere e modificare componenti al SPA.
