---
title: Integrare AEM Sites e Experienci Platform Web SDK
description: Scopri come integrare AEM Sites as a Cloud Service con Experienci Platform Web SDK. Questo passaggio fondamentale è essenziale per l’integrazione di prodotti Adobe Experience Cloud, come Adobe Analytics, Target o prodotti innovativi recenti come Real-time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service" before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '1354'
ht-degree: 4%

---

# Integrare AEM Sites e Experienci Platform Web SDK

Scopri come integrare AEM as a Cloud Service con Experienci Platform [SDK per web](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Questo passaggio fondamentale è essenziale per l’integrazione di prodotti Adobe Experience Cloud, come Adobe Analytics, Target o prodotti innovativi recenti come Real-time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.

Scopri anche come raccogliere e inviare [WKND - progetto Adobe Experience Manager di esempio](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) dati pageview in [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Dopo aver completato questa configurazione, hai implementato una solida base. Inoltre, sei pronto per implementare Experienci Platform utilizzando applicazioni come [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=it), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), e [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). L’implementazione avanzata contribuisce a migliorare il coinvolgimento dei clienti standardizzando il web e i dati dei clienti.

## Prerequisiti

Durante l’integrazione di Experienci Platform Web SDK sono necessari i seguenti elementi.

In entrata **AEM come Cloud Service**:

+ Accesso dell’amministratore AEM all’ambiente as a Cloud Service AEM
+ Accesso a Cloud Manager da parte di Deployment Manager
+ Clona e implementa [WKND - progetto Adobe Experience Manager di esempio](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) nell’ambiente as a Cloud Service dell’AEM.

In entrata **Experience Platform**:

+ Accesso alla produzione predefinita, **Prod** sandbox.
+ Accesso a **Schemi** in Gestione dati
+ Accesso a **Set di dati** in Gestione dati
+ Accesso a **Flussi di dati** in Raccolta dati
+ Accesso a **Tag** (precedentemente noto come Launch) in Raccolta dati

Se non disponi delle autorizzazioni necessarie, l’amministratore di sistema utilizza [Adobe Admin Console](https://adminconsole.adobe.com/) può concedere le autorizzazioni necessarie.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Crea schema XDM - Experience Platform

Lo schema Experience Data Model (XDM) consente di standardizzare i dati sull’esperienza del cliente. Per raccogliere **Visualizzazione pagina WKND** , creare uno schema XDM e utilizzare l’Adobe fornito di gruppi di campi `AEP Web SDK ExperienceEvent` per la raccolta di dati web.

Sono disponibili modelli generici e specifici per settori quali vendita al dettaglio, servizi finanziari, sanità e altro ancora, suite di modelli di dati di riferimento. [Panoramica dei modelli dati di settore](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) per ulteriori informazioni.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Scopri lo schema XDM e i concetti correlati, come gruppi di campi, tipi, classi e tipi di dati da [Panoramica del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

Il [Panoramica del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) è un’ottima risorsa per scoprire lo schema XDM e i concetti correlati, come gruppi di campi, tipi, classi e tipi di dati. Fornisce informazioni complete sul modello dati XDM e su come creare e gestire schemi XDM per standardizzare i dati in tutta l’azienda. Esploralo per comprendere più a fondo lo schema XDM e i vantaggi che può apportare ai processi di raccolta e gestione dei dati.

## Crea stream di dati - Experience Platform

Un flusso di dati indica alla Platform Edge Network dove inviare i dati raccolti. Può essere inviato ad Experience Platform, Analytics o Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Acquisisci familiarità con il concetto di flussi di dati e argomenti correlati, come la governance e la configurazione dei dati, visitando il [Panoramica sugli stream di dati](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) pagina.

## Crea proprietà tag - Experience Platform

Scopri come creare una proprietà tag (precedentemente nota come Launch) in Experienci Platform per aggiungere la libreria JavaScript dell’SDK web al sito web WKND. La proprietà tag appena definita dispone delle seguenti risorse:

+ Estensioni tag: [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) e [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Elementi dati: gli elementi dati del tipo di codice personalizzato che estraggono nome-pagina, sezione-sito e nome-host utilizzando l’Adobe del livello dati client del sito WKND. Inoltre, l’elemento dati del tipo di oggetto XDM conforme alla nuova generazione dello schema XDM WKND precedente [Crea schema XDM](#create-xdm-schema---experience-platform) passaggio.
+ Regola: invia dati a Platform Edge Network ogni volta che viene visitata una pagina web WKND utilizzando Adobe Client Data Layer attivato `cmp:show` evento.

Durante la creazione e la pubblicazione della libreria di tag utilizzando **Flusso di pubblicazione**, è possibile utilizzare **Aggiungi tutte le risorse modificate** pulsante. Per selezionare tutte le risorse come Elemento dati, Regola ed Estensioni tag invece di identificare e selezionare una singola risorsa. Inoltre, durante la fase di sviluppo, puoi pubblicare la libreria solo in _Sviluppo_ quindi verificarlo e promuoverlo nell&#39;ambiente _Fase_ o _Produzione_ ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>Puoi consultare il codice Data Element ed Rule-Event mostrato nel video, **espandi l’elemento pannello a soffietto sottostante**. Tuttavia, se NON utilizzi Adobe Client Data Layer, devi modificare il codice seguente, ma il concetto di definizione degli elementi dati e di utilizzo degli stessi nella definizione della regola è ancora applicabile.


+++ Codice elemento dati e regola-evento

+ Il `Page Name` Codice elemento dati.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ Il `Site Section` Codice elemento dati.

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

+ Il `Host Name` Codice elemento dati.

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ Il `all pages - on load` Codice regola-evento

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


Il [Panoramica sui tag](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) fornisce informazioni approfondite su concetti importanti come elementi dati, regole ed estensioni.

Per ulteriori informazioni sull’integrazione dei componenti core AEM con Adobe Client Data Layer, consulta [Utilizzo di Adobe Client Data Layer con i componenti core AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=it).

## Connettere la proprietà Tag a AEM

Scopri come collegare all’AEM la proprietà tag creata di recente tramite Adobe IMS e la configurazione di Adobe Launch nell’AEM. Quando viene stabilito un ambiente as a Cloud Service per l’AEM, vengono generate automaticamente diverse configurazioni dell’account tecnico Adobe IMS, incluso Adobe Launch. Tuttavia, per la versione AEM 6.5, è necessario configurarne manualmente una.

Dopo aver collegato la proprietà tag, il sito WKND è in grado di caricare la libreria JavaScript della proprietà tag nelle pagine web utilizzando la configurazione del servizio cloud Adobe Launch.

### Verifica del caricamento della proprietà tag in WKND

Utilizzo di Adobi Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) o [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) , verifica se la proprietà tag è in fase di caricamento sulle pagine WKND. Puoi verificare:

+ Dettagli della proprietà tag come estensione, versione, nome e altro ancora.
+ Versione della libreria dell’SDK web di Platform, ID dello stream di dati
+ Oggetto XDM come parte `events` attributo in Experienci Platform Web SDK

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Crea set di dati - Experience Platform

I dati di pageview raccolti tramite Web SDK vengono memorizzati nel data lake di Experienci Platform come set di dati. Il set di dati è un costrutto di archiviazione e gestione per una raccolta di dati come una tabella di database che segue uno schema. Scopri come creare un set di dati e configurare lo stream di dati creato in precedenza per inviare dati all’Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

Il [Panoramica sui set di dati](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) fornisce ulteriori informazioni su concetti, configurazioni e altre funzionalità di acquisizione.


## Dati WKND pageview in Experienci Platform

Dopo la configurazione dell’SDK per web con AEM, in particolare sul sito WKND, è ora di generare il traffico navigando tra le pagine del sito. Quindi verifica che i dati di pageview vengano acquisiti nel set di dati Experienci Platform. Nell’interfaccia utente del set di dati, vengono visualizzati vari dettagli quali record totali, dimensioni e batch acquisiti, insieme a un grafico a barre visivamente accattivante.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Riepilogo

Ottimo lavoro Hai completato la configurazione dell’AEM con Experienci Platform Web SDK per raccogliere e acquisire dati da un sito web. Con questa base, ora puoi esplorare ulteriori possibilità per migliorare e integrare prodotti come Analytics, Target, Customer Journey Analytics (CJA) e molti altri per creare esperienze ricche e personalizzate per i tuoi clienti. Continua ad apprendere ed esplorare per sfruttare appieno il potenziale di Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Se preferisci il **video end-to-end** che copre l’intero processo di integrazione invece dei singoli video delle fasi di configurazione, puoi fare clic su [qui](https://video.tv.adobe.com/v/3418905/) per accedervi.

## Risorse aggiuntive

+ [Utilizzo di Adobe Client Data Layer con i Componenti core](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=it)
+ [Integrazione dei tag di raccolta dati Experienci Platform e AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=it)
+ [Panoramica di Adobe Experience Platform Web SDK e Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutorial su Raccolta dati](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Panoramica Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
