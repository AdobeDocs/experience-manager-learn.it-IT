---
title: Integrare AEM Sites e Experience Platform Web SDK
description: Scopri come integrare AEM Sites as a Cloud Service con Experience Platform Web SDK. Questo passaggio fondamentale è essenziale per l’integrazione di prodotti Adobe Experience Cloud, come Adobe Analytics, Target o prodotti innovativi recenti come Real-Time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.
version: Experience Manager as a Cloud Service
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
duration: 1303
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1248'
ht-degree: 1%

---

# Integrare AEM Sites e Experience Platform Web SDK

Scopri come integrare AEM as a Cloud Service con Experience Platform [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/web-sdk/home.html?lang=it). Questo passaggio fondamentale è essenziale per l’integrazione di prodotti Adobe Experience Cloud, come Adobe Analytics, Target o prodotti innovativi recenti come Real-Time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.

Scopri anche come raccogliere e inviare [WKND - dati di esempio del progetto Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) di pageview in [Experience Platform](https://experienceleague.adobe.com/it/docs/experience-platform/landing/home).

Dopo aver completato questa configurazione, hai implementato una solida base. Inoltre, puoi portare avanti l&#39;implementazione di Experience Platform utilizzando applicazioni come [Real-Time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=it), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/it/docs/customer-journey-analytics) e [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/it/docs/journey-optimizer). L’implementazione avanzata contribuisce a migliorare il coinvolgimento dei clienti standardizzando il web e i dati dei clienti.

## Prerequisiti

Per l’integrazione di Experience Platform Web SDK sono necessari i seguenti elementi.

In **AEM as Cloud Service**:

+ Accesso amministratore AEM all’ambiente AEM as a Cloud Service
+ Accesso di Responsabile dell’implementazione a Cloud Manager
+ Clona e distribuisci [WKND - progetto Adobe Experience Manager di esempio](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) nell&#39;ambiente AEM as a Cloud Service.

In **Experience Platform**:

+ Accesso alla sandbox di produzione predefinita **Prod**.
+ Accesso a **Schemi** in Gestione dati
+ Accesso a **Set di dati** in Gestione dati
+ Accesso a **Datastreams** in Raccolta dati
+ Accesso a **Tag** in Raccolta dati

Se non disponi delle autorizzazioni necessarie, l&#39;amministratore di sistema che utilizza [Adobe Admin Console](https://adminconsole.adobe.com/) può concedere le autorizzazioni necessarie.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Creare uno schema XDM - Experience Platform

Lo schema Experience Data Model (XDM) consente di standardizzare i dati sull’esperienza del cliente. Per raccogliere i dati **WKND pageview**, creare uno schema XDM e utilizzare i gruppi di campi `AEP Web SDK ExperienceEvent` forniti da Adobe per la raccolta di dati Web.

Sono disponibili modelli generici e specifici per settori, ad esempio vendita al dettaglio, servizi finanziari, sanità e altro ancora, suite di modelli di dati di riferimento. Per ulteriori informazioni, consulta [Panoramica sui modelli di dati di settore](https://experienceleague.adobe.com/it/docs/experience-platform/xdm/schema/industries/overview).


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Scopri lo schema XDM e i concetti correlati, come gruppi di campi, tipi, classi e tipi di dati, dalla [panoramica del sistema XDM](https://experienceleague.adobe.com/it/docs/experience-platform/xdm/home).

[Panoramica del sistema XDM](https://experienceleague.adobe.com/it/docs/experience-platform/xdm/home) è un&#39;ottima risorsa per conoscere lo schema XDM e i concetti correlati, come gruppi di campi, tipi, classi e tipi di dati. Fornisce informazioni complete sul modello dati XDM e su come creare e gestire schemi XDM per standardizzare i dati in tutta l’azienda. Esploralo per comprendere più a fondo lo schema XDM e i vantaggi che può apportare ai processi di raccolta e gestione dei dati.

## Creare uno stream di dati - Experience Platform

Un flusso di dati indica a Platform Edge Network dove inviare i dati raccolti. Può essere inviato ad esempio ad Experience Platform, Analytics o Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Acquisisci familiarità con il concetto di flussi di dati e gli argomenti correlati, come la governance e la configurazione dei dati, visitando la pagina [Panoramica sui flussi di dati](https://experienceleague.adobe.com/docs/experience-platform/datastreams/overview.html?lang=it).

## Crea proprietà tag - Experience Platform

Scopri come creare una proprietà tag in Experience Platform per aggiungere la libreria JavaScript di Web SDK al sito web WKND. La proprietà tag appena definita dispone delle risorse seguenti:

+ Estensioni tag: [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) e [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Elementi dati: gli elementi dati del tipo di codice personalizzato che estraggono nome-pagina, sezione-sito e nome-host utilizzando Adobe Client Data Layer del sito WKND. Inoltre, l&#39;elemento dati del tipo di oggetto XDM conforme alla compilazione del nuovo schema XDM WKND creata in precedenza [passaggio Crea schema XDM](#create-xdm-schema---experience-platform).
+ Regola: invia dati a Platform Edge Network ogni volta che viene visitata una pagina web WKND utilizzando l’evento Adobe Client Data Layer attivato `cmp:show`.

Durante la creazione e la pubblicazione della libreria di tag utilizzando **Flusso di pubblicazione**, puoi utilizzare il pulsante **Aggiungi tutte le risorse modificate**. Per selezionare tutte le risorse come Elemento dati, Regola ed Estensioni tag invece di identificare e selezionare una singola risorsa. Inoltre, durante la fase di sviluppo, puoi pubblicare la libreria solo nell&#39;ambiente _Sviluppo_, quindi verificarla e promuoverla nell&#39;ambiente _Stage_ o _Produzione_.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>L&#39;elemento dati e il codice di evento regola mostrati nel video sono disponibili come riferimento, **espandi il seguente elemento pannello a soffietto**. Tuttavia, se NON utilizzi Adobe Client Data Layer, devi modificare il codice seguente, ma il concetto di definizione degli elementi dati e di utilizzo degli stessi nella definizione della regola è ancora applicabile.


+++ Codice elemento dati e regola-evento

+ Codice elemento dati `Page Name`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ Codice elemento dati `Site Section`.

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

+ Codice elemento dati `Host Name`.

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ Codice evento regola `all pages - on load`

  ```javascript
  var pageShownEventHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      // trigger tags Rule and pass event
      console.debug("cmp:show event: " + evt.eventInfo.path);
      var event = {
          // include the path of the component that triggered the event
          path: evt.eventInfo.path,
          // get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      // Trigger the tags Rule, passing in the new 'event' object
      // the 'event' obj can now be referenced by the reserved name 'event' by other tags data elements
      // i.e 'event.component['someKey']'
      trigger(event);
      }
  }
  
  // set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  
  // push the event listener for cmp:show into the data layer
  window.adobeDataLayer.push(function (dl) {
      //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
      dl.addEventListener("cmp:show", pageShownEventHandler);
  });
  ```

+++


La [Panoramica sui tag](https://experienceleague.adobe.com/it/docs/experience-platform/tags/home) fornisce informazioni approfondite su concetti importanti come elementi dati, regole ed estensioni.

Per ulteriori informazioni sull&#39;integrazione dei componenti core di AEM con Adobe Client Data Layer, consulta la [guida Utilizzo di Adobe Client Data Layer con i componenti core di AEM](https://experienceleague.adobe.com/it/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview).

## Connettere la proprietà Tag ad AEM

Scopri come collegare ad AEM la proprietà tag creata di recente tramite Adobe IMS e i tag in Configurazione Adobe Experience Platform in AEM. Quando viene stabilito un ambiente AEM as a Cloud Service, vengono generate automaticamente diverse configurazioni dell’account tecnico Adobe IMS, inclusi i tag. Per istruzioni dettagliate, consulta [Connettere AEM Sites con la proprietà tag utilizzando IMS](https://experienceleague.adobe.com/it/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/connect-aem-tag-property-using-ims).

Tuttavia, per la versione 6.5 di AEM, è necessario configurarne una manualmente.



Dopo aver collegato la proprietà tag, il sito WKND è in grado di caricare la libreria JavaScript della proprietà tag nelle pagine web utilizzando i tag nella configurazione del servizio cloud Adobe Experience Platform.

### Verifica del caricamento della proprietà tag in WKND

Utilizzando l&#39;estensione Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob), verifica se la proprietà tag è in fase di caricamento sulle pagine WKND. Puoi verificare:

+ Dettagli della proprietà tag come estensione, versione, nome e altro ancora.
+ Versione della libreria di Platform Web SDK, ID Datastream
+ Oggetto XDM come parte dell&#39;attributo `events` in Experience Platform Web SDK

>[!VIDEO](https://video.tv.adobe.com/v/3454507?quality=12&learn=on&captions=ita)

## Crea set di dati - Experience Platform

I dati di pageview raccolti tramite Web SDK vengono memorizzati nel data lake di Experience Platform come set di dati. Il set di dati è un costrutto di archiviazione e gestione per una raccolta di dati come una tabella di database che segue uno schema. Scopri come creare un set di dati e configurare lo stream di dati creato in precedenza per inviare dati ad Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

La [Panoramica sui set di dati](https://experienceleague.adobe.com/it/docs/experience-platform/catalog/datasets/overview) fornisce ulteriori informazioni su concetti, configurazioni e altre funzionalità di acquisizione.


## Dati WKND pageview in Experience Platform

Dopo la configurazione del Web SDK con AEM, in particolare nel sito WKND, è ora di generare il traffico navigando tra le pagine del sito. Quindi verifica che i dati di pageview vengano acquisiti nel set di dati di Experience Platform. Nell’interfaccia utente del set di dati, vengono visualizzati vari dettagli quali record totali, dimensioni e batch acquisiti, insieme a un grafico a barre visivamente accattivante.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Riepilogo

Ottimo lavoro! Hai completato la configurazione di AEM con Experience Platform Web SDK per raccogliere e acquisire dati da un sito Web. Con questa base, ora puoi esplorare ulteriori possibilità per migliorare e integrare prodotti come Analytics, Target, Customer Journey Analytics (CJA) e molti altri per creare esperienze ricche e personalizzate per i tuoi clienti. Continua ad apprendere ed esplorare per sfruttare appieno il potenziale di Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Se preferisci il **video end-to-end** che copre l&#39;intero processo di integrazione invece dei singoli video delle fasi di configurazione, puoi fare clic [qui](https://video.tv.adobe.com/v/3418905/) per accedervi.

## Risorse aggiuntive

+ [Utilizzo di Adobe Client Data Layer con i Componenti core](https://experienceleague.adobe.com/it/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview)
+ [Integrazione dei tag di raccolta dati di Experience Platform e AEM](https://experienceleague.adobe.com/it/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview)
+ [Panoramica di Adobe Experience Platform Web SDK e Edge Network](https://experienceleague.adobe.com/it/docs/platform-learn/data-collection/web-sdk/overview)
+ [Esercitazioni sulla raccolta dati](https://experienceleague.adobe.com/it/docs/platform-learn/data-collection/overview)
+ [Panoramica di Adobe Experience Platform Debugger](https://experienceleague.adobe.com/it/docs/platform-learn/data-collection/debugger/overview)
