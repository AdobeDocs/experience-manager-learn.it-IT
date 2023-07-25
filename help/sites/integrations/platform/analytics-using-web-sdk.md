---
title: Integrare AEM Sites e Adobe Analytics con Platform Web SDK
description: Integra AEM Sites e Adobe Analytics utilizzando l’approccio moderno Platform Web SDK.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 9f54995f-4ce7-45f2-9021-6fdfe42ff89a
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '1637'
ht-degree: 2%

---

# Integrare AEM Sites e Adobe Analytics con Platform Web SDK

Scopri **approccio moderno** su come integrare Adobe Experience Manager (AEM) e Adobe Analytics utilizzando Platform Web SDK. Questo tutorial completo ti guida attraverso il processo di raccolta diretta [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) dati sui clic di pageview e CTA. Ottieni informazioni preziose visualizzando i dati raccolti in Adobe Analysis Workspace, dove puoi esplorare varie metriche e dimensioni. Inoltre, esplora il set di dati di Platform per verificare e analizzare i dati. Unisciti a noi in questo percorso per sfruttare la potenza di AEM e Adobe Analytics per un processo decisionale basato sui dati.

## Panoramica

Ottenere informazioni sul comportamento degli utenti è un obiettivo fondamentale per ogni team di marketing. Comprendendo il modo in cui gli utenti interagiscono con i loro contenuti, i team possono prendere decisioni informate, ottimizzare le strategie e ottenere risultati migliori. Per raggiungere questo obiettivo, il team di marketing WKND, un’entità fittizia, ha fissato obiettivi sull’implementazione di Adobe Analytics sul proprio sito web. L’obiettivo principale è quello di raccogliere dati su due metriche chiave: visualizzazioni di pagina e clic di invito all’azione (CTA) nella pagina home.

Tracciando le visualizzazioni di pagina, il team è in grado di analizzare quali pagine ricevono più attenzione da parte degli utenti. Inoltre, il tracciamento dei clic CTA della home page fornisce informazioni utili sull’efficacia degli elementi di invito all’azione del team. Questi dati possono rivelare quali CTA risuonano con gli utenti, quali necessitano di aggiustamento e potenzialmente scoprire nuove opportunità per migliorare il coinvolgimento degli utenti e favorire le conversioni.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Prerequisiti

Per l’integrazione di Adobe Analytics tramite Platform Web SDK sono necessari i seguenti elementi.

Sono stati completati i passaggi di configurazione della **[Integrare Experience Platform Web SDK](./web-sdk.md)** esercitazione.

In entrata **AEM come Cloud Service**:

+ [Accesso dell’amministratore AEM all’ambiente as a Cloud Service AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=it)
+ Accesso a Cloud Manager da parte di Deployment Manager
+ Clona e implementa [WKND - progetto Adobe Experience Manager di esempio](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) nell’ambiente as a Cloud Service dell’AEM.

In entrata **Adobe Analytics**:

+ Accesso per creare **Suite di rapporti**
+ Accesso per creare **Analysis Workspace**

In entrata **Experience Platform**:

+ Accesso alla produzione predefinita, **Prod** sandbox.
+ Accesso a **Schemi** in Gestione dati
+ Accesso a **Set di dati** in Gestione dati
+ Accesso a **Flussi di dati** in Raccolta dati
+ Accesso a **Tag** (precedentemente noto come Launch) in Raccolta dati

Se non disponi delle autorizzazioni necessarie, l’amministratore di sistema utilizza [Adobe Admin Console](https://adminconsole.adobe.com/) può concedere le autorizzazioni necessarie.

Prima di approfondire il processo di integrazione di AEM e Analytics tramite Platform Web SDK, _ricapitolare i componenti essenziali e gli elementi chiave_ che sono stati stabiliti nel [Integrare Experience Platform Web SDK](./web-sdk.md) esercitazione. Fornisce una solida base per l’integrazione.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Dopo la ricapitolazione della connessione tra schema XDM, stream di dati, set di dati, proprietà tag e, AEM e proprietà tag, entriamo nel percorso di integrazione.

## Definire il documento SDR (Solution Design Reference) di Analytics

Come parte del processo di implementazione, si consiglia di creare un documento Solution Design Reference (SDR). Questo documento svolge un ruolo cruciale come blueprint per definire i requisiti di business e progettare strategie efficaci di raccolta dati.

Il documento sul documento SDR fornisce una panoramica completa del piano di attuazione, garantendo che tutte le parti interessate siano allineate e comprendano gli obiettivi e l’ambito del progetto.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Per maggiori informazioni sui concetti e sui vari elementi da includere nel documento SDR, visita [Creare e gestire un documento Solution Design Reference (SDR)](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html). Puoi anche scaricare un modello Excel di esempio, ma è disponibile anche la versione specifica WKND [qui](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Configurazione di Analytics: suite di rapporti, Analysis Workspace

Il primo passaggio consiste nel configurare Adobe Analytics, in particolare una suite di rapporti con variabili di conversione (o eVar) ed eventi di successo. Le variabili di conversione vengono utilizzate per misurare la causa e l’effetto. Gli eventi di successo vengono utilizzati per tenere traccia delle azioni.

In questa esercitazione,  `eVar5, eVar6, and eVar7` track  _Nome pagina WKND, ID CTA WKND e nome CTA WKND_ rispettivamente e `event7` viene utilizzato per tracciare  _Evento clic CTA WKND_.

Per analizzare, raccogliere informazioni e condividerle con altri dai dati raccolti, viene creato un progetto in Analysis Workspace.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Per ulteriori informazioni sulla configurazione e i concetti di Analytics, si consiglia vivamente di utilizzare le risorse seguenti:

+ [Suite per report](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html)
+ [Variabili di conversione](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [Eventi di successo](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## Aggiornare lo stream di dati - Aggiungere il servizio Analytics

Un flusso di dati indica alla Platform Edge Network dove inviare i dati raccolti. In [esercitazione precedente](./web-sdk.md), viene configurato un Datastream per inviare i dati all’Experience Platform. Questo flusso di dati viene aggiornato per inviare i dati alla suite di rapporti di Analytics configurata in [sopra](#setup-analytics---report-suite-analysis-workspace) passaggio.

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## Crea schema XDM

Lo schema Experience Data Model (XDM) consente di standardizzare i dati raccolti. In [esercitazione precedente](./web-sdk.md), uno schema XDM con `AEP Web SDK ExperienceEvent` viene creato un gruppo di campi. Inoltre, utilizzando questo schema XDM viene creato un set di dati per memorizzare i dati raccolti nell’Experience Platform.

Tuttavia, tale schema XDM non dispone di gruppi di campi specifici per Adobe Analytics per inviare i dati evento eVar. Viene creato un nuovo schema XDM invece di aggiornare lo schema esistente per evitare di memorizzare i dati evento eVar nella piattaforma.

Il nuovo schema XDM creato ha `AEP Web SDK ExperienceEvent` e `Adobe Analytics ExperienceEvent Full Extension` gruppi di campi.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Aggiorna proprietà tag

In [esercitazione precedente](./web-sdk.md), viene creata una proprietà tag con elementi dati e una regola per raccogliere, mappare e inviare i dati di visualizzazione della pagina. Deve essere migliorato per:

+ Mappatura del nome della pagina su `eVar5`
+ Attivazione di **pageview** Chiamata di Analytics (o beacon di invio)
+ Raccolta di dati CTA tramite Adobe Client Data Layer
+ Mappatura dell’ID e del nome CTA su `eVar6` e `eVar7` rispettivamente. Inoltre, il conteggio dei clic CTA per `event7`
+ Attivazione di **clic collegamento** Chiamata di Analytics (o beacon di invio)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>Puoi consultare il codice Data Element ed Rule-Event mostrato nel video, **espandi l’elemento pannello a soffietto sottostante**. Tuttavia, se NON utilizzi Adobe Client Data Layer, devi modificare il codice seguente, ma il concetto di definizione degli elementi dati e di utilizzo degli stessi nella definizione della regola è ancora applicabile.

+++ Codice elemento dati e regola-evento

+ Il `Component ID` Codice elemento dati.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ Il `Component Name` Codice elemento dati.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ Il `all pages - on load` **Rule-Condition** codice

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ Il `home page - cta click` **Rule-Event** codice

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
      // i.e `event.component['someKey']`
      trigger(event);
  }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  //push the event listener for cmp:click into the data layer
  window.adobeDataLayer.push(function (dl) {
  //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
  dl.addEventListener("cmp:click", componentClickedHandler);
  });    
  ```

+ Il `home page - cta click` **Rule-Condition** codice

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

Per ulteriori informazioni sull’integrazione dei componenti core AEM con Adobe Client Data Layer, consulta [Utilizzo di Adobe Client Data Layer con i componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=it).


>[!INFO]
>
>Per una comprensione completa del **Mappa variabili** nel documento Solution Design Reference (SDR), accedere alla versione WKND completa per il download [qui](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## Verifica proprietà tag aggiornata in WKND

Per garantire che la proprietà tag aggiornata sia generata, pubblicata e funzioni correttamente nelle pagine del sito WKND. Utilizza il browser web Google Chrome [Estensione Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob):

+ Per verificare che la proprietà tag sia la versione più recente, controlla la data di build.

+ Per verificare i dati dell’evento XDM per PageView e HomePage CTA Click, utilizza l’opzione di menu Experience Platform Web SDK all’interno dell’estensione.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Simulare il traffico web - Automazione Selenium

Per generare una quantità significativa di traffico a scopo di test, viene sviluppato uno script di automazione Selenium. Questo script personalizzato simula le interazioni dell’utente con il sito web WKND, ad esempio la visualizzazione della pagina, e con il clic sui CTA.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Verifica del set di dati: WKND pageview, dati CTA

Il set di dati è un costrutto di archiviazione e gestione per una raccolta di dati come una tabella di database che segue uno schema. Il set di dati creato in [esercitazione precedente](./web-sdk.md) viene riutilizzato per verificare che i dati di pageview e CTA click vengano acquisiti nel set di dati Experience Platform. Nell’interfaccia utente del set di dati, vengono visualizzati vari dettagli quali record totali, dimensioni e batch acquisiti, insieme a un grafico a barre visivamente accattivante.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics: visualizzazione pagina WKND, visualizzazione dati CTA

Analysis Workspace è uno strumento potente all’interno di Adobe Analytics che consente di esplorare e visualizzare i dati in modo flessibile e interattivo. Fornisce un’interfaccia di trascinamento per creare rapporti personalizzati, eseguire segmentazioni avanzate e applicare varie visualizzazioni di dati.

Riapriamo il progetto Analysis Workspace creato in [Configurazione analisi](#setup-analytics---report-suite-analysis-workspace) passaggio. In **Pagine principali** , esamina varie metriche quali visite, visitatori univoci, voci, frequenza di rimbalzo e altro ancora. Per valutare le prestazioni delle pagine WKND e dei CTA della home page, trascina le dimensioni (Nome pagina WKND, Nome CTA WKND) e le metriche specifiche per WKND (Evento clic WKND CTA). Queste informazioni sono utili per gli esperti di marketing per capire quali CTA sono più efficaci e prendere decisioni basate sui dati, in linea con i loro obiettivi aziendali.

Per visualizzare i percorsi di utenti, utilizza la visualizzazione Flusso, che inizia con **Nome pagina WKND** ed espanderli in vari percorsi.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Riepilogo

Ottimo lavoro Hai completato la configurazione di AEM e Adobe Analytics utilizzando Platform Web SDK per raccogliere e analizzare i dati sul clic di visualizzazione pagina e CTA.

L’implementazione di Adobe Analytics è fondamentale per consentire ai team di marketing di acquisire informazioni sul comportamento degli utenti, prendere decisioni informate, ottimizzare i contenuti e prendere decisioni basate sui dati.

Implementando i passaggi consigliati e utilizzando le risorse fornite, come il documento Solution Design Reference (SDR) e la comprensione dei concetti chiave di Analytics, gli esperti di marketing possono raccogliere e analizzare i dati in modo efficace.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Se preferisci il **video end-to-end** che copre l’intero processo di integrazione invece dei singoli video delle fasi di configurazione, puoi fare clic su [qui](https://video.tv.adobe.com/v/3419889/) per accedervi.


## Risorse aggiuntive

+ [Integrare Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [Utilizzo di Adobe Client Data Layer con i Componenti core](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=it)
+ [Integrazione dei tag di raccolta dati Experience Platform e AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Panoramica di Adobe Experience Platform Web SDK e Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutorial su Raccolta dati](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Panoramica di Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
