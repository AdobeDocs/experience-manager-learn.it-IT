---
title: Utilizzo dei visualizzatori Dynamic Media con Adobe Analytics e Adobe Launch
description: Con l’estensione Dynamic Media Viewers per Adobe Launch, rilasciata con Dynamic Media Viewers 5.13, i clienti di Dynamic Media, Adobe Analytics e Adobe Launch possono utilizzare eventi e dati specifici per i visualizzatori Dynamic Media nella propria configurazione di Adobe Launch.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 17%

---


# Utilizzo dei visualizzatori Dynamic Media con Adobe Analytics e Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Per i clienti con Dynamic Media e Adobe Analytics, ora puoi monitorare l’utilizzo dei visualizzatori Dynamic Media sul tuo sito web utilizzando l’estensione Dynamic Media Viewer.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Esegui Adobe Experience Manager in modalità Dynamic Media Scene7 per questa funzionalità. Devi anche [integrare Adobe Experience Platform Launch con la tua istanza AEM](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html).

Con l’introduzione dell’estensione Dynamic Media Viewer, Adobe Experience Manager offre ora il supporto di analisi avanzato per le risorse fornite con i visualizzatori Dynamic Media (5.13), garantendo un controllo più granulare sul tracciamento degli eventi quando un visualizzatore Dynamic Media viene utilizzato in una pagina Sites.

Se disponi già di AEM Assets e Sites, puoi integrare la proprietà Launch con l’istanza di authoring di AEM. Una volta associata l’integrazione di launch al sito web, puoi aggiungere alla pagina un componente multimediale dinamico con il tracciamento degli eventi per i visualizzatori abilitati.

Per i clienti AEM Assets-only o per i clienti Dynamic Media Classic, l’utente può ottenere il codice da incorporare per un visualizzatore e aggiungerlo alla pagina. Le librerie Script di Launch possono quindi essere aggiunte manualmente alla pagina per il tracciamento degli eventi del visualizzatore.

Nella tabella seguente sono elencati gli eventi del visualizzatore Dynamic Media e i relativi argomenti supportati:

<table>
   <tbody>
      <tr>
         <td>Nome evento del visualizzatore</td>
         <td>Riferimento argomento</td>
      </tr>
      <tr>
         <td> COMUNE </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> BANNER <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> ELEMENTO </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> CARICA </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.company% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> METADATI </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> PIETRA </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> PAGINA </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> PAUSA </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> PLAY </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> SPIN </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> FERMARE </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SWAP </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> CAPPELLO </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> ZOOM </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## Risorse aggiuntive{#additional-resources}

* [Integrazione di Adobe Experience Manager con Adobe Launch](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [Esecuzione di Adobe Experience Manager in modalità Dynamic Media Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integrazione dei visualizzatori Dynamic Media con Adobe Analytics e Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
