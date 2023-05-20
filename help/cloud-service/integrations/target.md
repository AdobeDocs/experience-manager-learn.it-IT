---
title: Personalizzazione AEM headless e Target
description: Questa esercitazione esplora il modo in cui i frammenti di contenuto AEM vengono esportati in Adobe Target e quindi utilizzati per personalizzare esperienze headless tramite l’SDK web per Adobe.
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
source-git-commit: b3cc9c4fbd36cdf5be46e4546a174fea0c8da05c
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 2%

---

# Personalizzare esperienze AEM headless con frammenti di contenuto

>[!IMPORTANT]
>
> L’esportazione dei frammenti di contenuto da Adobe Experience Manager ad Adobe Target è disponibile in nell’as a Cloud Service AEM [canale prerelease](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=it#new-features).



Questa esercitazione esplora il modo in cui i frammenti di contenuto AEM vengono esportati in Adobe Target e quindi utilizzati per personalizzare esperienze headless tramite l’SDK web per Adobe. Il [App WKND di React](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) viene utilizzato per esplorare il modo in cui un’attività Target personalizzata che utilizza le offerte con frammenti di contenuto può essere aggiunta all’esperienza, per promuovere un’avventura WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

Il tutorial illustra i passaggi necessari per configurare AEM e Adobe Target:

1. [Creare una configurazione Adobe IMS per Adobe Target](#adobe-ims-configuration) in AEM Author
1. [Creazione di un Cloud Service Adobe Target](#adobe-target-cloud-service) in AEM Author
1. [Applicare il Cloud Service Adobe Target alle cartelle AEM Assets](#configure-asset-folders) in AEM Author
1. [Autorizzare il Cloud Service Adobe Target](#permission) in Adobe Admin Console
1. [Esporta frammenti di contenuto](#export-content-fragments) da AEM Author a Target
1. [Creare un’attività utilizzando le offerte dei frammenti di contenuto](#activity) in Adobe Target
1. [Creazione di uno stream di dati di Experience Platform](#datastream-id) nell’Experience Platform
1. [Integrare la personalizzazione in un’app AEM headless basata su React](#code) utilizzando l’SDK per web di Adobe.

## Configurazione Adobe IMS{#adobe-ims-configuration}

Configurazione Adobe IMS che facilita l’autenticazione tra AEM e Adobe Target.

Revisione [la documentazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) istruzioni dettagliate su come creare una configurazione Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Cloud Service Adobe Target{#adobe-target-cloud-service}

Nell’AEM viene creato un Cloud Service Adobe Target per facilitare l’esportazione dei frammenti di contenuto in Adobe Target.

Revisione [la documentazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) istruzioni dettagliate sulla creazione di un Cloud Service Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Configurare le cartelle di risorse{#configure-asset-folders}

Il Cloud Service Adobe Target, configurato in una configurazione in base al contesto, deve essere applicato alla gerarchia di cartelle di AEM Assets che contiene i frammenti di contenuto da esportare in Adobe Target.

+++Espandi per istruzioni dettagliate

1. Accedi a __Servizio AEM Author__ come amministratore DAM
1. Accedi a __Risorse > File__, individua la cartella di risorse con `/conf` applicato a
1. Seleziona la cartella delle risorse e fai clic su __Proprietà__ dalla barra delle azioni superiore
1. Seleziona la scheda __Servizi cloud__
1. Assicurati che la configurazione cloud sia impostata sulla configurazione in base al contesto (`/conf`) che contiene la configurazione di Cloud Services Adobe Target.
1. Seleziona __Adobe Target__ dal __Configurazioni Cloud Service__ a discesa.
1. Seleziona __Salva e chiudi__ in alto a destra

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Autorizzare l’integrazione di AEM Target{#permission}

L’integrazione Adobe Target, che si manifesta come progetto developer.adobe.com, deve ottenere il __Editor__ ruolo del prodotto in Adobe Admin Console, per esportare i frammenti di contenuto in Adobe Target.

+++Espandi per istruzioni dettagliate

1. Accedi ad Experience Cloud come utente in grado di amministrare il prodotto Adobe Target in Adobe Admin Console
1. Apri [Adobe Admin Console](https://adminconsole.adobe.com)
1. Seleziona __Prodotti__ e quindi aprire __Adobe Target__
1. Il giorno __Profili di prodotto__ , seleziona __*DefaultWorkspace*__
1. Seleziona la __Credenziali API__ scheda
1. Individua l’app developer.adobe.com in questo elenco e impostane le __Ruolo prodotto__ a __Editor__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Esportare frammenti di contenuto in Target{#export-content-fragments}

Frammenti di contenuto esistenti in [gerarchia di cartelle di AEM Assets configurata](#apply-adobe-target-cloud-service-to-aem-assets-folders) può essere esportato in Adobe Target come Offerte con frammenti di contenuto. Queste offerte di frammenti di contenuto, una forma speciale di offerte JSON in Target, possono essere utilizzate nelle attività di Target per fornire esperienze personalizzate in app headless.

+++Espandi per istruzioni dettagliate

1. Accedi a __Autore AEM__ come utente DAM
1. Accedi a __Risorse > File__, e individuare i frammenti di contenuto da esportare come JSON in Target nella cartella &quot;Adobe Target enabled&quot;
1. Selezionare i frammenti di contenuto da esportare in Adobe Target
1. Seleziona __Esporta in offerte Adobe Target__ dalla barra delle azioni superiore
   + Questa azione esporta la rappresentazione JSON completa di tutti i dati del frammento di contenuto in Adobe Target come &quot;Offerta frammento di contenuto&quot;
   + La rappresentazione JSON completamente idratata può essere rivista in AEM
      + Seleziona il frammento di contenuto
      + Espandi il pannello laterale
      + Seleziona __Anteprima__ nel pannello laterale sinistro
      + La rappresentazione JSON esportata in Adobe Target viene visualizzata nella vista principale
1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com) con un utente nel ruolo Editor per Adobe Target
1. Dalla sezione [Experience Cloud](https://experience.adobe.com), seleziona __Target__ dallo switcher del prodotto in alto a destra per aprire Adobe Target.
1. Assicurati che l’area di lavoro predefinita sia selezionata in __Commutatore area di lavoro__ in alto a destra.
1. Seleziona la __Offerte__ scheda nella navigazione superiore
1. Seleziona la __Tipo__ e selezionando __Frammenti di contenuto__
1. Verifica che il frammento di contenuto esportato dall’AEM venga visualizzato nell’elenco
   + Passa il puntatore del mouse sull’offerta e seleziona __Visualizza__ pulsante
   + Rivedi __Informazioni offerta__ e visualizzare __Collegamento profondo AEM__ che apre il frammento di contenuto direttamente nel servizio AEM Author

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Attività Target con offerte per frammenti di contenuto{#activity}

In Adobe Target, è possibile creare un’attività che utilizza come contenuto il codice JSON per l’offerta di frammenti di contenuto, consentendo esperienze personalizzate in app headless con contenuti creati e gestiti nell’AEM.

In questo esempio utilizziamo una semplice attività A/B, ma è possibile utilizzare qualsiasi attività Target.

+++Espandi per istruzioni dettagliate

1. Seleziona la __Attività__ scheda nella navigazione superiore
1. Seleziona __+ Crea attività__, quindi seleziona il tipo di attività da creare.
   + In questo esempio viene creato un semplice __Test A/B__ Tuttavia, le offerte di frammenti di contenuto possono alimentare qualsiasi tipo di attività
1. In __Crea attività__ procedura guidata
   + Seleziona __Web__
   + In entrata __Scegli Compositore esperienza__, seleziona __Modulo__
   + In entrata __Scegli area di lavoro__, seleziona __Area di lavoro predefinita__
   + In entrata __Scegli proprietà__, seleziona la Proprietà in cui è disponibile l’attività, oppure seleziona __Nessuna restrizione di proprietà__ per consentirne l’utilizzo in tutte le Proprietà.
   + Seleziona __Successivo__ per creare l’attività
1. Rinominare l’attività selezionando __rinomina__ in alto a sinistra
   + Assegna all’attività un nome significativo
1. Nell’esperienza iniziale, imposta __Posizione 1__ per l&#39;attività di destinazione
   + In questo esempio, esegui il targeting di una posizione personalizzata denominata `wknd-adventure-promo`
1. Sotto __Contenuto__ seleziona il contenuto predefinito, quindi fai clic su __Modifica frammento di contenuto__
1. Seleziona il frammento di contenuto esportato da distribuire per questa esperienza e fai clic su __Fine__
1. Rivedi il JSON per l’offerta di frammenti di contenuto nell’area di testo Contenuto; si tratta dello stesso JSON disponibile nel servizio di authoring di AEM tramite l’azione Anteprima del frammento di contenuto.
1. Nella barra a sinistra, aggiungi un’esperienza e seleziona un’offerta di frammento di contenuto diversa da distribuire
1. Seleziona __Successivo__ e configura le regole di targeting in base alle esigenze dell’attività
   + In questo esempio, lascia il test A/B come suddivisione manuale 50/50.
1. Seleziona __Successivo__, e completare le impostazioni dell&#39;attività
1. Seleziona __Salva e chiudi__ e gli dia un nome significativo
1. Dall’attività in Adobe Target, seleziona __Attiva__ dal menu a discesa Inattivo/Attiva/Archivia in alto a destra.

L’attività Adobe Target che esegue il targeting del `wknd-adventure-promo` La posizione può ora essere integrata ed esposta in un’app headless AEM.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## ID dello stream di dati di Experience Platform{#datastream-id}

Un [Datastream Adobe Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) È necessario un ID per consentire alle app AEM headless di interagire con Adobe Target utilizzando [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++Espandi per istruzioni dettagliate

1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com/)
1. Apri __Experience Platform__
1. Seleziona __Raccolta dati > Flussi di dati__ e seleziona __Nuovo flusso di dati__
1. Nella procedura guidata Nuovo flusso di dati, immetti:
   + Nome: `AEM Target integration`
   + Descrizione: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Schema evento: `Leave blank`
1. Seleziona __Salva__
1. Seleziona __Aggiungi servizio__
1. In entrata __Servizio__ seleziona __Adobe Target__
   + Attivato: __Sì__
   + Token proprietà: __Lascia vuoto__
   + ID ambiente di destinazione: __Lascia vuoto__
      + L’ambiente di Target può essere impostato in Adobe Target all’indirizzo __Administration > Hosts__.
   + Spazio dei nomi ID di terze parti di destinazione: __Lascia vuoto__
1. Seleziona __Salva__
1. Sul lato destro, copia il __ID flusso di dati__ da utilizzare in [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) chiamata di configurazione.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Aggiungere personalizzazione a un’app AEM headless{#code}

Questo tutorial esplora come personalizzare una semplice app React utilizzando le offerte dei frammenti di contenuto in Adobe Target tramite [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Questo approccio può essere utilizzato per personalizzare qualsiasi esperienza web basata su JavaScript.

Le esperienze mobili Android™ e iOS possono essere personalizzate seguendo modelli simili utilizzando [SDK mobile di Adobe](https://developer.adobe.com/client-sdks/documentation/).

### Prerequisiti

+ Node.js 14
+ Git
+ [WKND condiviso 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) installato sui servizi di authoring e pubblicazione di AEM as a Cloud

### Configurazione

1. Scarica il codice sorgente per l’app React di esempio da [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri base di codice in `~/Code/aem-guides-wknd-graphql/personalization-tutorial` nell’IDE preferito
1. Aggiorna l’host del servizio AEM a cui desideri che l’app si connetta `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. Esegui l’app e accertati che si connetta al servizio AEM configurato. Dalla riga di comando, esegui:

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. Installare [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) come pacchetto NPM.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   L’SDK per web può essere utilizzato nel codice per recuperare il JSON offerta frammento di contenuto per posizione di attività.

   Durante la configurazione dell’SDK web, sono necessari due ID:

   + `edgeConfigId` che è il [ID flusso di dati](#datastream-id)
   + `orgId` l&#39;ID organizzazione dell&#39;Adobe as a Cloud Service/target dell&#39;AEM reperibile all&#39;indirizzo __Experience Cloud > Profilo > Informazioni account > ID organizzazione corrente__

   Quando si richiama l’SDK per web, la posizione dell’attività Adobe Target (nel nostro esempio, `wknd-adventure-promo`) deve essere impostato come valore nella sezione `decisionScopes` array.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implementazione

1. Creare un componente React `AdobeTargetActivity.js` per far emergere le attività di Adobe Target.

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   Il componente React di AdobeTargetActivity viene richiamato utilizzando come segue:

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. Creare un componente React `AdventurePromo.js` per eseguire il rendering dell’avventura JSON Adobe Target serve.

   Questo componente React utilizza il JSON completo di idratazione che rappresenta un frammento di contenuto di avventura e viene visualizzato in modo promozionale. I componenti React che visualizzano il codice JSON servito da Offerte con frammenti di contenuto di Adobe Target possono essere vari e complessi come richiesto in base ai frammenti di contenuto esportati in Adobe Target.

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   Questo componente React viene richiamato come segue:

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. Aggiungere il componente AdobeTargetActivity a quello dell’app React `Home.js` sopra l&#39;elenco delle avventure.

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. Se l’app React non è in esecuzione, riavvia utilizzando `npm run start`.

   Apri l’app React in due browser diversi, quindi consenti al test A/B di distribuire le diverse esperienze a ciascun browser. Se entrambi i browser mostrano la stessa offerta di avventura, prova a chiudere/riaprire uno dei browser fino a visualizzare l’altra esperienza.

   L’immagine seguente mostra le due diverse offerte di frammenti di contenuto visualizzate per `wknd-adventure-promo` Attività, in base alla logica di Adobe Target.

   ![Offerte di esperienza](./assets/target/offers-in-app.png)

## Congratulazioni. 

Ora che abbiamo configurato l’AEM as a Cloud Service per esportare i frammenti di contenuto in Adobe Target, abbiamo utilizzato le offerte di frammenti di contenuto in un’attività Adobe Target e abbiamo fatto emergere tale attività in un’app AEM headless, personalizzando l’esperienza.
