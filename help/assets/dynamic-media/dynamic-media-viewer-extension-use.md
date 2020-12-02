---
title: Utilizzo di visualizzatori per contenuti multimediali dinamici con  Adobe Analytics e lancio  Adobe
seo-title: Utilizzo di visualizzatori per contenuti multimediali dinamici con  Adobe Analytics e lancio  Adobe
description: Con l’estensione Dynamic Media Viewers per Adobe Launch, rilasciata con Dynamic Media Viewers 5.13, i clienti di Dynamic Media, Adobe Analytics e Adobe Launch possono utilizzare eventi e dati specifici per i visualizzatori Dynamic Media nella propria configurazione di Adobe Launch.
seo-description: 'Con l’estensione Dynamic Media Viewers per Adobe Launch, rilasciata con Dynamic Media Viewers 5.13, i clienti di Dynamic Media, Adobe Analytics e Adobe Launch possono utilizzare eventi e dati specifici per i visualizzatori Dynamic Media nella propria configurazione di Adobe Launch. '
sub-product: Dynamic-media, analisi
feature: asset-insights, media-player
topics: images, videos, renditions, authoring, integrations, publishing
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 23%

---


# Utilizzo di visualizzatori per contenuti multimediali dinamici con  Adobe Analytics e lancio  Adobe{#using-dynamic-media-viewers-adobe-analytics-launch}

Per i clienti con contenuti multimediali dinamici e  Adobe Analytics, ora potete monitorare l’utilizzo dei visualizzatori per contenuti multimediali dinamici sul vostro sito Web utilizzando l’estensione per visualizzatori per contenuti multimediali dinamici.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Per questa funzionalità, eseguite Adobe Experience Manager in modalità Dynamic Media Scene7. È inoltre necessario [integrare  Adobe Experience Platform Launch con l&#39;istanza AEM](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html).

Con l’introduzione dell’estensione Dynamic Media Viewer, Adobe Experience Manager offre ora il supporto di analisi avanzato per le risorse fornite con i visualizzatori per contenuti multimediali dinamici (5.13), fornendo un controllo più dettagliato sul tracciamento degli eventi quando un visualizzatore per contenuti multimediali dinamici viene utilizzato in una pagina Siti.

Se disponete già di  AEM Assets e Siti, potete integrare la proprietà Launch con l’istanza AEM autore. Una volta che l’integrazione di avvio è associata al sito Web, potete aggiungere alla pagina dei componenti per contenuti multimediali dinamici con il tracciamento degli eventi abilitato per i visualizzatori.

Per  clienti che utilizzano solo AEM Assets o i clienti di Dynamic Media Classic, l’utente può ottenere il codice da incorporare per un visualizzatore e aggiungerlo alla pagina. Le librerie di script di avvio possono essere aggiunte manualmente alla pagina per il tracciamento degli eventi del visualizzatore.

Nella tabella seguente sono elencati gli eventi del visualizzatore di contenuti multimediali dinamici e i relativi argomenti supportati:

<table>
   <tbody>
      <tr>
         <td>Nome evento visualizzatore</td>
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
         <td> CARICO </td>
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
         <td> MILESTONE </td>
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
         <td> STOP </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SWAP </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> CAMPIONE </td>
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

* [Integrazione di Adobe Experience Manager con  lancio Adobe](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [Esecuzione di Adobe Experience Manager in modalità Scene7 per contenuti multimediali dinamici](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integrazione dei visualizzatori Dynamic Media con Adobe Analytics e Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
