---
title: Personalizzazione AEM headless e Target
description: Questa esercitazione esplora AEM modo in cui i frammenti di contenuto vengono esportati in Adobe Target e quindi utilizzati per personalizzare esperienze headless utilizzando l’SDK per web per Adobe.
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

# Personalizzare AEM esperienze headless con frammenti di contenuto

>[!IMPORTANT]
>
> L’esportazione dei frammenti di contenuto Adobe Experience Manager in Adobe Target è disponibile nella AEM as a Cloud Service [canale prerelease](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=it#new-features).



Questa esercitazione esplora AEM modo in cui i frammenti di contenuto vengono esportati in Adobe Target e quindi utilizzati per personalizzare esperienze headless utilizzando l’SDK per web per Adobe. La [Reagisce all’app WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) viene utilizzato per esplorare come aggiungere all’esperienza un’attività Target personalizzata utilizzando le offerte dei frammenti di contenuto, per promuovere un’avventura WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

L’esercitazione descrive i passaggi necessari per configurare AEM e Adobe Target:

1. [Creare una configurazione Adobe IMS per Adobe Target](#adobe-ims-configuration) in AEM Author
1. [Crea Cloud Service Adobe Target](#adobe-target-cloud-service) in AEM Author
1. [Applicare Adobe Target Cloud Service alle cartelle AEM Assets](#configure-asset-folders) in AEM Author
1. [Autorizzazione all&#39;Cloud Service Adobe Target](#permission) in Adobe Admin Console
1. [Esportare frammenti di contenuto](#export-content-fragments) da AEM Author a Target
1. [Creare un’attività utilizzando le offerte dei frammenti di contenuto](#activity) in Adobe Target
1. [Creare un Experience Platform Datastream](#datastream-id) in Experience Platform
1. [Integrare la personalizzazione in un’app AEM headless basata su React](#code) mediante l’SDK per web di Adobe.

## Configurazione di Adobe IMS{#adobe-ims-configuration}

Configurazione Adobe IMS che facilita l’autenticazione tra AEM e Adobe Target.

Revisione [la documentazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) per istruzioni dettagliate su come creare una configurazione Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Cloud Service Adobe Target{#adobe-target-cloud-service}

In AEM viene creato un Cloud Service Adobe Target per facilitare l’esportazione di frammenti di contenuto in Adobe Target.

Revisione [la documentazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) istruzioni dettagliate su come creare un Cloud Service Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Configurare le cartelle di risorse{#configure-asset-folders}

Il Cloud Service Adobe Target, configurato in una configurazione in base al contesto, deve essere applicato alla gerarchia di cartelle di AEM Assets che contiene i frammenti di contenuto da esportare in Adobe Target.

+++Espandi per istruzioni dettagliate

1. Accedi a __Servizio di authoring di AEM__ come amministratore DAM
1. Passa a __Risorse > File__, individua la cartella di risorse con la `/conf` applicato a
1. Seleziona la cartella di risorse e seleziona __Proprietà__ dalla barra delle azioni superiore
1. Seleziona la scheda __Servizi cloud__
1. Verifica che la configurazione cloud sia impostata sulla configurazione in base al contesto (`/conf`) che contiene la configurazione Adobe Target Cloud Services.
1. Seleziona __Adobe Target__ dal __Configurazioni Cloud Service__ a discesa.
1. Seleziona __Salva e chiudi__ in alto a destra

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Autorizzare l’integrazione di AEM Target{#permission}

All’integrazione di Adobe Target, che si manifesta come progetto developer.adobe.com, deve essere concesso il __Editor__ ruolo di prodotto in Adobe Admin Console, per esportare frammenti di contenuto in Adobe Target.

+++Espandi per istruzioni dettagliate

1. Accedi ad Experience Cloud come utente che può amministrare il prodotto Adobe Target in Adobe Admin Console
1. Apri [Adobe Admin Console](https://adminconsole.adobe.com)
1. Seleziona __Prodotti__ e quindi aprire __Adobe Target__
1. Sulla __Profili di prodotto__ scheda , seleziona __*Area di lavoro predefinita*__
1. Seleziona la __Credenziali API__ scheda
1. Individua l’app developer.adobe.com in questo elenco e impostane la relativa __Ruolo del prodotto__ a __Editor__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Esportare frammenti di contenuto in Target{#export-content-fragments}

Frammenti di contenuto presenti nella sezione [gerarchia di cartelle AEM Assets configurata](#apply-adobe-target-cloud-service-to-aem-assets-folders) può essere esportato in Adobe Target come offerte di frammenti di contenuto. Queste offerte di frammenti di contenuto, una forma speciale di offerte JSON in Target, possono essere utilizzate nelle attività di Target per distribuire esperienze personalizzate in app headless.

+++Espandi per istruzioni dettagliate

1. Accedi a __AEM Author__ come utente DAM
1. Passa a __Risorse > File__, e individua i frammenti di contenuto da esportare come JSON in Target nella cartella &quot;Adobe Target abilitato&quot;
1. Selezionare i frammenti di contenuto da esportare in Adobe Target
1. Seleziona __Esportazione in offerte Adobe Target__ dalla barra delle azioni superiore
   + Questa azione esporta in Adobe Target la rappresentazione JSON completamente idratata del frammento di contenuto come &quot;offerta di frammenti di contenuto&quot;
   + La rappresentazione JSON completamente idratata può essere rivista in AEM
      + Selezionare il frammento di contenuto
      + Espandi il pannello laterale
      + Seleziona __Anteprima__ nel pannello laterale sinistro
      + La rappresentazione JSON esportata in Adobe Target viene visualizzata nella vista principale
1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com) con un utente nel ruolo Editor per Adobe Target
1. Da [Experience Cloud](https://experience.adobe.com), seleziona __Target__ dal commutatore del prodotto in alto a destra per aprire Adobe Target.
1. Assicurati che l’area di lavoro predefinita sia selezionata nella __Switcher area di lavoro__ in alto a destra.
1. Seleziona la __Offerte__ scheda nella navigazione superiore
1. Seleziona la __Tipo__ a discesa e selezione __Frammenti di contenuto__
1. Verifica che il frammento di contenuto esportato da AEM venga visualizzato nell’elenco
   + Passa il puntatore del mouse sull’offerta e seleziona la __Visualizza__ pulsante
   + Consulta la sezione __Informazioni offerta__ e vedere __Collegamento profondo AEM__ che apre il frammento di contenuto direttamente nel servizio AEM Author

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Attività Target tramite le offerte dei frammenti di contenuto{#activity}

In Adobe Target è possibile creare un’attività che utilizza JSON per l’offerta di frammenti di contenuto, consentendo esperienze personalizzate in app headless con contenuti creati e gestiti in AEM.

In questo esempio, utilizziamo una semplice attività A/B, tuttavia è possibile utilizzare qualsiasi attività Target.

+++Espandi per istruzioni dettagliate

1. Seleziona la __Attività__ scheda nella navigazione superiore
1. Seleziona __+ Crea attività__, quindi seleziona il tipo di attività da creare.
   + Questo esempio crea un __Test A/B__ ma le offerte per i frammenti di contenuto possono alimentare qualsiasi tipo di attività
1. In __Crea attività__ procedura guidata
   + Seleziona __Web__
   + In __Scegli Compositore esperienza__, seleziona __Modulo__
   + In __Scegli area di lavoro__, seleziona __Area di lavoro predefinita__
   + In __Scegli proprietà__, seleziona la proprietà in cui è disponibile l’attività o seleziona __Nessuna limitazione di proprietà__ per consentirne l’utilizzo in tutte le proprietà.
   + Seleziona __Successivo__ per creare l’attività
1. Rinomina l’attività selezionando __rinomina__ in alto a sinistra
   + Assegna all’attività un nome significativo
1. Nell’esperienza iniziale, imposta __Posizione 1__ per l’attività di destinazione
   + In questo esempio, esegui il targeting di una posizione personalizzata denominata `wknd-adventure-promo`
1. Sotto __Contenuto__ seleziona il contenuto predefinito e seleziona __Modifica frammento di contenuto__
1. Seleziona il frammento di contenuto esportato da distribuire per questa esperienza e seleziona __Fine__
1. Rivedi JSON Offerta frammento di contenuto nell’area di testo Contenuto , lo stesso JSON disponibile nel servizio AEM Author tramite l’azione Anteprima del frammento di contenuto .
1. Nella barra a sinistra, aggiungi un’esperienza e seleziona un’offerta di frammenti di contenuto diversa da distribuire
1. Seleziona __Successivo__ e configura le regole di targeting come richiesto per l’attività
   + In questo esempio, lascia il test A/B come divisione manuale 50/50.
1. Seleziona __Successivo__ e completa le impostazioni dell’attività
1. Seleziona __Salva e chiudi__ e dargli un nome significativo
1. Dall’attività in Adobe Target, seleziona __Attiva__ dal menu a discesa Inattivo/Attiva/Archivia in alto a destra.

L’attività Adobe Target che esegue il targeting dell’ `wknd-adventure-promo` La posizione può ora essere integrata ed esposta in un’app AEM Headless.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## ID Experience Platform Datastream{#datastream-id}

Un [Adobe Experience Platform Datastream](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) È necessario che AEM le app headless interagiscano con Adobe Target utilizzando l’ [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++Espandi per istruzioni dettagliate

1. Passa a [Adobe Experience Cloud](https://experience.adobe.com/)
1. Apri __Experience Platform__
1. Seleziona __Raccolta dati > Datastreams__ e seleziona __Nuovo Datastream__
1. Nella procedura guidata Nuovo archivio dati immettere:
   + Nome: `AEM Target integration`
   + Descrizione: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Schema evento: `Leave blank`
1. Seleziona __Salva__
1. Seleziona __Aggiungi servizio__
1. In __Servizio__ select __Adobe Target__
   + Abilitato: __Sì__
   + Token di proprietà: __Lascia vuoto__
   + ID ambiente di destinazione: __Lascia vuoto__
      + L’ambiente di Target può essere impostato in Adobe Target in __Amministrazione > Host__.
   + Namespace ID di terze parti di destinazione: __Lascia vuoto__
1. Seleziona __Salva__
1. A destra, copia il __ID Datastream__ per l&#39;uso in [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) chiamata di configurazione.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Aggiungi la personalizzazione a un’app AEM Headless{#code}

Questa esercitazione esplora la personalizzazione di una semplice app React utilizzando le offerte di frammenti di contenuto in Adobe Target tramite [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Questo approccio può essere utilizzato per personalizzare qualsiasi esperienza web basata su JavaScript.

Le esperienze mobili Android™ e iOS possono essere personalizzate seguendo pattern simili utilizzando [SDK mobile di Adobe](https://developer.adobe.com/client-sdks/documentation/).

### Prerequisiti

+ Node.js 14
+ Git
+ [WKND condiviso 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) installati su AEM servizi Author e Publish di Cloud

### Configurazione

1. Scarica il codice sorgente per l’app React di esempio da [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri la base di codice in `~/Code/aem-guides-wknd-graphql/personalization-tutorial` nel tuo IDE preferito
1. Aggiorna l&#39;host del servizio AEM a cui desideri che l&#39;app si connetta `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. Esegui l’app e assicurati che si connetta al servizio AEM configurato. Dalla riga di comando, esegui:

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. Installa il [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) come pacchetto NPM.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   L’SDK per web può essere utilizzato nel codice per recuperare il JSON dell’offerta di frammenti di contenuto per posizione dell’attività.

   Quando configuri l&#39;SDK web, sono necessari due ID:

   + `edgeConfigId` che è [ID Datastream](#datastream-id)
   + `orgId` l&#39;ID organizzazione Adobe as a Cloud Service/di Target AEM che si trova in __Experience Cloud > Profilo > Informazioni account > ID organizzazione corrente__

   Quando si richiama l’SDK web, il percorso dell’attività Adobe Target (nel nostro esempio, `wknd-adventure-promo`) deve essere impostato come valore nel `decisionScopes` array.

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

1. Creare un componente React `AdventurePromo.js` per eseguire il rendering dell’avventura servita da JSON Adobe Target.

   Questo componente React prende il JSON completamente idratato che rappresenta un frammento di contenuto avventura e viene visualizzato in modo promozionale. I componenti React che visualizzano il servizio JSON gestito dalle offerte dei frammenti di contenuto di Adobe Target possono essere modificati e complessi in base ai frammenti di contenuto esportati in Adobe Target.

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

1. Aggiungi il componente AdobeTargetActivity al componente React dell&#39;app `Home.js` sopra l&#39;elenco delle avventure.

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

1. Se l&#39;app React non è in esecuzione, riavvia utilizzando `npm run start`.

   Apri l’app React in due browser diversi per consentire al test A/B di distribuire le diverse esperienze a ciascun browser. Se entrambi i browser mostrano la stessa offerta di avventura, prova a chiudere/riaprire uno dei browser fino a quando non viene visualizzata l’altra esperienza.

   L’immagine seguente mostra le due diverse offerte di frammenti di contenuto visualizzate per `wknd-adventure-promo` Attività, in base alla logica di Adobe Target.

   ![Offerte esperienza](./assets/target/offers-in-app.png)

## Congratulazioni. 

Ora che abbiamo configurato AEM as a Cloud Service per esportare i frammenti di contenuto in Adobe Target, abbiamo utilizzato le offerte di frammenti di contenuto in un’attività Adobe Target e abbiamo eseguito la visualizzazione di tale attività in un’app senza titolo AEM, personalizzando l’esperienza.
