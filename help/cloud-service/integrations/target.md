---
title: Integrare AEM Headless e Target
description: Scopri come integrare AEM Headless e Adobe Target per personalizzare le esperienze headless tramite Experience Platform Web SDK.
version: Experience Manager as a Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
duration: 1549
source-git-commit: adc2f352544b4718522073642c6bf971b3600616
workflow-type: tm+mt
source-wordcount: '1618'
ht-degree: 0%

---

# Integrare AEM Headless e Target

Scopri come integrare AEM Headless con Adobe Target esportando frammenti di contenuto AEM in Adobe Target e utilizzarli per personalizzare esperienze headless utilizzando Adobe Experience Platform Web SDK alloy.js. L&#39;[app WKND React](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) viene utilizzata per esplorare come aggiungere all&#39;esperienza un&#39;attività Target personalizzata che utilizza offerte con frammenti di contenuto, per promuovere un&#39;avventura WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

Il tutorial illustra i passaggi necessari per configurare AEM e Adobe Target:

1. [Creare una configurazione Adobe IMS per Adobe Target](#adobe-ims-configuration) in AEM Author
2. [Creare Adobe Target Cloud Service](#adobe-target-cloud-service) in AEM Author
3. [Applicare Adobe Target Cloud Service alle cartelle di AEM Assets](#configure-asset-folders) in AEM Author
4. [Autorizzazione per Adobe Target Cloud Service](#permission) in Adobe Admin Console
5. [Esporta frammenti di contenuto](#export-content-fragments) da AEM Author a Target
6. [Crea un&#39;attività utilizzando le offerte dei frammenti di contenuto](#activity) in Adobe Target
7. [Creazione di uno stream di dati di Experience Platform](#datastream-id) in Experience Platform
8. [Integra la personalizzazione in un&#39;app AEM headless basata su React](#code) tramite Adobe Web SDK.

## Configurazione Adobe IMS{#adobe-ims-configuration}

Configurazione Adobe IMS che facilita l’autenticazione tra AEM e Adobe Target.

Consulta [la documentazione](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/integrations/target#adobe-target-cloud-service) per istruzioni dettagliate su come creare una configurazione Adobe IMS.

## Adobe Target Cloud Service{#adobe-target-cloud-service}

In AEM viene creato un Cloud Service di Adobe Target per facilitare l’esportazione dei frammenti di contenuto in Adobe Target.

Consulta [la documentazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) per istruzioni dettagliate su come creare un Cloud Service di Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Configurare le cartelle di risorse{#configure-asset-folders}

Adobe Target Cloud Service, configurato in una configurazione in base al contesto, deve essere applicato alla gerarchia di cartelle di AEM Assets che contiene i frammenti di contenuto da esportare in Adobe Target.

+++Espandi per istruzioni dettagliate

1. Accedi al servizio __AEM Author__ come amministratore DAM
1. Passa a __Assets > File__, individua la cartella risorse a cui è applicato `/conf`
1. Seleziona la cartella delle risorse e fai clic su __Proprietà__ nella barra delle azioni in alto
1. Seleziona la scheda __Servizi cloud__
1. Verificare che la configurazione cloud sia impostata sulla configurazione in base al contesto (`/conf`) che contiene la configurazione di Adobe Target Cloud Services.
1. Seleziona __Adobe Target__ dal menu a discesa __Configurazioni Cloud Service__.
1. Seleziona __Salva e chiudi__ in alto a destra

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Autorizzare l’integrazione con AEM Target{#permission}

Per esportare i frammenti di contenuto in Adobe Target, è necessario assegnare all&#39;integrazione Adobe Target, che si presenta come progetto developer.adobe.com, il ruolo di prodotto __Editor__ in Adobe Admin Console.

+++Espandi per istruzioni dettagliate

1. Accedi ad Experience Cloud come utente in grado di amministrare il prodotto Adobe Target in Adobe Admin Console
1. Apri [Adobe Admin Console](https://adminconsole.adobe.com)
1. Seleziona __Prodotti__, quindi apri __Adobe Target__
1. Nella scheda __Profili di prodotto__, seleziona __*DefaultWorkspace*__
1. Seleziona la scheda __Credenziali API__
1. Individua l&#39;app developer.adobe.com in questo elenco e imposta __Product Role__ su __Editor__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Esportare frammenti di contenuto in Target{#export-content-fragments}

I frammenti di contenuto presenti nella gerarchia di cartelle di AEM Assets [configurata](#apply-adobe-target-cloud-service-to-aem-assets-folders) possono essere esportati in Adobe Target come Offerte di frammenti di contenuto. Queste offerte di frammenti di contenuto, una forma speciale di offerte JSON in Target, possono essere utilizzate nelle attività di Target per fornire esperienze personalizzate in app headless.

+++Espandi per istruzioni dettagliate

1. Accedi a __AEM Author__ come utente DAM
1. Passa a __Assets > File__ e individua i frammenti di contenuto da esportare come JSON in Target nella cartella &quot;Adobe Target enabled&quot;
1. Selezionare i frammenti di contenuto da esportare in Adobe Target
1. Seleziona __Esporta in offerte Adobe Target__ dalla barra delle azioni superiore
   + Questa azione esporta la rappresentazione JSON completa di tutti i dati del frammento di contenuto in Adobe Target come &quot;Offerta frammento di contenuto&quot;
   + La rappresentazione JSON completamente idratata può essere rivista in AEM
      + Seleziona il frammento di contenuto
      + Espandi il pannello laterale
      + Seleziona l&#39;icona __Anteprima__ nel pannello laterale sinistro
      + La rappresentazione JSON esportata in Adobe Target viene visualizzata nella vista principale
1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com) con un utente con il ruolo di editor per Adobe Target
1. Da [Experience Cloud](https://experience.adobe.com), seleziona __Target__ dal selettore prodotti in alto a destra per aprire Adobe Target.
1. Assicurati che il Workspace predefinito sia selezionato nel __commutatore Workspace__ in alto a destra.
1. Seleziona la scheda __Offerte__ nella navigazione superiore
1. Seleziona il menu a discesa __Tipo__ e seleziona __Frammenti di contenuto__
1. Verifica che il frammento di contenuto esportato da AEM venga visualizzato nell’elenco
   + Passa il puntatore del mouse sull&#39;offerta e seleziona il pulsante __Visualizza__
   + Rivedi le __Informazioni sull&#39;offerta__ e osserva il __collegamento profondo AEM__ che apre il frammento di contenuto direttamente nel servizio AEM Author

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Attività Target con offerte per frammenti di contenuto{#activity}

In Adobe Target, è possibile creare un’attività che utilizza come contenuto il codice JSON per l’offerta di frammenti di contenuto, per creare esperienze personalizzate in app headless con contenuti creati e gestiti in AEM.

In questo esempio utilizziamo una semplice attività A/B, ma è possibile utilizzare qualsiasi attività Target.

+++Espandi per istruzioni dettagliate

1. Seleziona la scheda __Attività__ nella navigazione superiore
1. Selezionare __+ Crea attività__, quindi selezionare il tipo di attività da creare.
   + Questo esempio crea un semplice __test A/B__, ma le offerte di frammenti di contenuto possono alimentare qualsiasi tipo di attività
1. Nella procedura guidata __Crea attività__
   + Seleziona __Web__
   + In __Scegli Compositore esperienza__, seleziona __Modulo__
   + In __Scegli Workspace__, seleziona __Workspace predefinito__
   + In __Scegli proprietà__, seleziona la proprietà in cui l&#39;attività è disponibile oppure seleziona __Nessuna restrizione proprietà__ per consentirne l&#39;utilizzo in tutte le proprietà.
   + Seleziona __Successivo__ per creare l&#39;attività
1. Rinomina l&#39;attività selezionando __rinomina__ in alto a sinistra
   + Assegna all’attività un nome significativo
1. Nell&#39;esperienza iniziale, imposta __Posizione 1__ per l&#39;attività di destinazione
   + In questo esempio, eseguire il targeting di una posizione personalizzata denominata `wknd-adventure-promo`
1. In __Contenuto__ selezionare il contenuto predefinito e selezionare __Cambia frammento di contenuto__
1. Seleziona il frammento di contenuto esportato da distribuire per questa esperienza e seleziona __Fine__
1. Rivedi il JSON per l’offerta di frammenti di contenuto nell’area di testo del contenuto; si tratta dello stesso JSON disponibile nel servizio di authoring di AEM tramite l’azione Anteprima del frammento di contenuto.
1. Nella barra a sinistra, aggiungi un’esperienza e seleziona un’offerta di frammento di contenuto diversa da distribuire
1. Seleziona __Avanti__ e configura le regole di targeting in base alle esigenze per l&#39;attività
   + In questo esempio, lascia il test A/B come suddivisione manuale 50/50.
1. Seleziona __Avanti__ e completa le impostazioni dell&#39;attività
1. Seleziona __Salva e chiudi__ e assegnagli un nome significativo
1. Dall&#39;attività in Adobe Target, seleziona __Attiva__ dal menu a discesa Inattivo/Attiva/Archivia in alto a destra.

L&#39;attività Adobe Target che esegue il targeting della posizione `wknd-adventure-promo` può ora essere integrata ed esposta in un&#39;app AEM Headless.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## ID dello stream di dati di Experience Platform{#datastream-id}

È necessario un ID [Adobe Experience Platform Datastream](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) per consentire alle app AEM Headless di interagire con Adobe Target utilizzando [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++Espandi per istruzioni dettagliate

1. Passa a [Adobe Experience Cloud](https://experience.adobe.com/)
1. Apri __Experience Platform__
1. Seleziona __Raccolta dati > Flussi di dati__ e seleziona __Nuovo flusso di dati__
1. Nella procedura guidata Nuovo flusso di dati, immetti:
   + Nome: `AEM Target integration`
   + Descrizione: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Schema evento: `Leave blank`
1. Seleziona __Salva__
1. Seleziona __Aggiungi servizio__
1. In __Servizio__ seleziona __Adobe Target__
   + Abilitato: __Sì__
   + Token proprietà: __Lascia vuoto__
   + ID ambiente di destinazione: __Lascia vuoto__
      + L&#39;ambiente di destinazione può essere impostato in Adobe Target in __Amministrazione > Host__.
   + Spazio dei nomi ID di terze parti di destinazione: __Lascia vuoto__
1. Seleziona __Salva__
1. Sul lato destro, copiare l&#39;ID __Datastream__ per utilizzarlo nella chiamata di configurazione [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Aggiungere personalizzazione a un’app AEM Headless{#code}

Questo tutorial illustra come personalizzare una semplice app React utilizzando le offerte relative ai frammenti di contenuto in Adobe Target tramite [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Questo approccio può essere utilizzato per personalizzare qualsiasi esperienza web basata su JavaScript.

Le esperienze mobili Android™ e iOS possono essere personalizzate seguendo modelli simili utilizzando [Adobe Mobile SDK](https://developer.adobe.com/client-sdks/documentation/).

### Prerequisiti

+ Node.js 14
+ Git
+ Installazione di [WKND Shared 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) in AEM as a Cloud Author e Publish Services

### Configurazione

1. Scarica il codice sorgente per l&#39;app React di esempio da [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Apri la base di codice in `~/Code/aem-guides-wknd-graphql/personalization-tutorial` nell&#39;IDE preferito
1. Aggiorna l&#39;host del servizio AEM a cui desideri che l&#39;app si connetta `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

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

1. Installa [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) come pacchetto NPM.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Il SDK web può essere utilizzato nel codice per recuperare il JSON offerta frammento di contenuto per posizione di attività.

   Durante la configurazione del Web SDK sono necessari due ID:

   + `edgeConfigId` che è l&#39;[ID Datastream](#datastream-id)
   + `orgId` l&#39;ID organizzazione AEM as a Cloud Service/Target Adobe reperibile in __Experience Cloud > Profilo > Informazioni account > ID organizzazione corrente__

   Quando si richiama il Web SDK, la posizione dell&#39;attività di Adobe Target (nel nostro esempio, `wknd-adventure-promo`) deve essere impostata come valore nell&#39;array `decisionScopes`.

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

1. Creare un componente React `AdventurePromo.js` per eseguire il rendering dell&#39;avventura JSON Adobe Target serve.

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

1. Aggiungi il componente AdobeTargetActivity a `Home.js` dell&#39;app React sopra l&#39;elenco delle avventure.

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

1. Se l&#39;app React non è in esecuzione, riavviare utilizzando `npm run start`.

   Apri l’app React in due browser diversi, quindi consenti al test A/B di distribuire le diverse esperienze a ciascun browser. Se entrambi i browser mostrano la stessa offerta di avventura, prova a chiudere/riaprire uno dei browser fino a visualizzare l’altra esperienza.

   L&#39;immagine seguente mostra le due diverse offerte di frammenti di contenuto visualizzate per l&#39;attività `wknd-adventure-promo`, in base alla logica di Adobe Target.

   ![Offerte esperienza](./assets/target/offers-in-app.png)

## Congratulazioni.

Ora che abbiamo configurato AEM as a Cloud Service per esportare frammenti di contenuto in Adobe Target, abbiamo utilizzato le Offerte di frammenti di contenuto in un’attività Adobe Target e abbiamo visualizzato tale attività in un’app headless AEM, personalizzando l’esperienza.
