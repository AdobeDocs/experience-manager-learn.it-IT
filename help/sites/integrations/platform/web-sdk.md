---
title: Integrare Experience Platform Web SDK
description: Scopri come integrare AEM as a Cloud Service con Experience Platform Web SDK. Questo passaggio fondamentale è fondamentale per l’integrazione di prodotti Adobe Experience Cloud, come Adobe Analytics, Target o prodotti innovativi recenti come Real-time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
source-git-commit: 3f129fb4fc53e55d118802d3a0e566a9a9bcb9a2
workflow-type: tm+mt
source-wordcount: '1153'
ht-degree: 3%

---


# Integrare Experience Platform Web SDK

Scopri come integrare AEM as a Cloud Service con Experience Platform [SDK per web](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Questo passaggio fondamentale è fondamentale per l’integrazione di prodotti Adobe Experience Cloud, come Adobe Analytics, Target o prodotti innovativi recenti come Real-time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.

Inoltre, imparerai a raccogliere e inviare [WKND - esempio di progetto Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) visualizzazione di dati nella pagina [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Dopo aver completato questa configurazione, puoi procedere con l’implementazione di Experience Platform e applicazioni correlate, come [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=it), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html) e [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). Promuovere un migliore coinvolgimento dei clienti standardizzando i dati web e dei clienti.

## Prerequisiti

Quando si integra Experience Platform Web SDK, sono necessari i seguenti requisiti.

In **AEM come Cloud Service**:

+ Accesso AEM amministratore all’ambiente as a Cloud Service AEM
+ Accesso a Cloud Manager da Deployment Manager
+ Clona e distribuisci [WKND - esempio di progetto Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) al tuo ambiente as a Cloud Service AEM.

In **Experience Platform**:

+ Accesso alla produzione predefinita, **Prod** sandbox.
+ Accesso a **Schemi** in Gestione dati
+ Accesso a **Set di dati** in Gestione dati
+ Accesso a **Datastreams** in Raccolta dati
+ Accesso a **Tag** (precedentemente noto come Launch) in Raccolta dati

Se non si dispone delle autorizzazioni necessarie, l&#39;amministratore di sistema utilizza [Adobe Admin Console](https://adminconsole.adobe.com/) può concedere le autorizzazioni necessarie.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Crea schema XDM - Experience Platform

Lo schema Experience Data Model (XDM) consente di standardizzare i dati sulla customer experience. Per raccogliere i dati **Visualizzazione a pagina WKND** creare uno schema XDM e utilizzare i gruppi di campi forniti dall’Adobe `AEP Web SDK ExperienceEvent` per la raccolta dati web.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Scopri lo schema XDM e concetti correlati quali gruppi di campi, tipi, classi e tipi di dati da [Panoramica del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

La [Panoramica del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) è una grande risorsa per informazioni sullo schema XDM e su concetti correlati quali gruppi di campi, tipi, classi e tipi di dati. Fornisce una comprensione completa del modello dati XDM e di come creare e gestire schemi XDM per standardizzare i dati in tutta l’azienda. Esploralo per comprendere meglio lo schema XDM e come può beneficiare dei processi di raccolta e gestione dei dati.

## Crea DataStream - Experience Platform

Un DataStream indica a Platform Edge Network dove inviare i dati raccolti. Ad esempio, può essere inviato ad Experience Platform, Analytics o Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Acquisisci familiarità con il concetto di Datastreams e argomenti correlati, come la governance dei dati e la configurazione, visitando il [Panoramica dei Datastreams](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) pagina.

## Crea proprietà tag - Experience Platform

Scopri come creare una proprietà tag (precedentemente nota come Launch) in Experience Platform per aggiungere la libreria JavaScript SDK per web al sito web WKND. La nuova proprietà tag definita dispone delle risorse seguenti:

+ Estensioni tag: [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) e [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Elementi dati: Gli elementi dati di tipo di codice personalizzato che estraggono nome-pagina, sezione-sito e nome-host utilizzando Livello dati client Adobe del sito WKND. Inoltre, l’elemento dati del tipo di oggetto XDM che è conforme alla build dello schema XDM WKND appena creata, precedente [Crea schema XDM](#create-xdm-schema---experience-platform) passo.
+ Regola: Invia dati a Platform Edge Network ogni volta che viene visitata una pagina web WKND utilizzando Adobe Client Data Layer attivato `cmp:show` evento.


>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


+++ Elemento dati e codice evento regola

+ La `Page Name` Codice elemento dati.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ La `Site Section` Codice elemento dati.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('repo:path')) {
   let pagePath = event.component['repo:path'];
   
   let siteSection = '';
   
   //Check of html String in URL.
   if (pagePath.indexOf('.html') > -1) { 
    siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
   
    //replace slash with colon
    siteSection = siteSection.replaceAll('/', ':');
   
    //remove `:content`
    siteSection = siteSection.replaceAll(':content:','');
   }
   
       return siteSection 
   }
   ```

+ La `Host Name` Codice elemento dati.

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ La `all pages - on load` Codice evento regola

   ```javascript
   var pageShownEventHandler = function(evt) {
   // defensive coding to avoid a null pointer exception
   if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
       //trigger Launch Rule and pass event
       console.debug("cmp:show event: " + evt.eventInfo.path);
       var event = {
           //include the path of the component that triggered the event
           path: evt.eventInfo.path,
           //get the state of the component that triggered the event
           component: window.adobeDataLayer.getState(evt.eventInfo.path)
       };
   
       //Trigger the Launch Rule, passing in the new 'event' object
       // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
       // i.e 'event.component['someKey']'
       trigger(event);
       }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
       //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
       dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

+++


La [Panoramica sui tag](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) fornisce informazioni approfondite su concetti importanti come Elementi dati, Regole ed Estensioni.

Per ulteriori informazioni sull’integrazione AEM componenti core con Adobe Client Data Layer, consulta [Utilizzo di Adobe Client Data Layer con la guida AEM componenti core](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=it).

## Connetti proprietà tag a AEM

Scopri come collegare la proprietà tag creata di recente a AEM tramite Adobe IMS e Adobe Launch Configuration in AEM. Quando viene stabilito un ambiente as a Cloud Service AEM, vengono generate automaticamente diverse configurazioni dell’account tecnico Adobe IMS, incluso Adobe Launch. Tuttavia, per AEM versione 6.5, è necessario configurarne una manualmente.

Dopo aver collegato la proprietà tag, il sito WKND è in grado di caricare la libreria JavaScript della proprietà tag sulle pagine web utilizzando la configurazione del servizio cloud Adobe Launch.

### Verifica il caricamento della proprietà tag su WKND

Utilizzo di Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) o [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) verifica se la proprietà tag viene caricata sulle pagine WKND. Puoi verificare,

+ Dettagli delle proprietà del tag come estensione, versione, nome e altro ancora.
+ Versione libreria SDK per web Platform, ID Datastream
+ Oggetto XDM come parte `events` attributo in Experience Platform Web SDK

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Crea set di dati - Experience Platform

I dati di visualizzazione pagina raccolti tramite SDK per web vengono memorizzati in un data lake di Experience Platform come set di dati. Il set di dati è un costrutto di archiviazione e gestione per una raccolta di dati come una tabella di database che segue uno schema. Scopri come creare un set di dati e configurare il Datastream creato in precedenza per inviare dati all’Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

La [Panoramica dei set di dati](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) fornisce ulteriori informazioni su concetti, configurazioni e altre funzionalità di acquisizione.


## Dati di visualizzazione di pagina WKND in Experience Platform

Dopo la configurazione dell&#39;SDK web con AEM, in particolare sul sito WKND, è ora di generare traffico navigando tra le pagine del sito. Quindi verifica che i dati della visualizzazione di pagina vengano acquisiti nel set di dati di Experience Platform. Nell’interfaccia utente del set di dati vengono visualizzati vari dettagli, quali record totali, dimensioni e batch acquisiti, insieme a un grafico a barre visivamente accattivante.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Riepilogo

Ottimo lavoro Hai completato la configurazione di AEM con Adobe Experience Platform (Experience Platform) Web SDK per raccogliere e acquisire dati da un sito web. Con questa base, puoi ora esplorare ulteriori possibilità per migliorare e integrare prodotti come Analytics, Target, Customer Journey Analytics (CJA) e molti altri per creare esperienze personalizzate per i tuoi clienti. Continua a imparare ed esplorare per sfruttare appieno il potenziale di Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)

## Risorse aggiuntive

+ [Utilizzo di Adobe Client Data Layer con i Componenti core](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=it)
+ [Integrazione dei tag e dei AEM di raccolta dati di Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Panoramica di Adobe Experience Platform Web SDK e Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutorial su Raccolta dati](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Panoramica di Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

