---
title: Utilizzo dei visualizzatori Dynamic Medie con Adobe Analytics e Adobe Launch
description: Con l’estensione Dynamic Media Viewers per Adobe Launch, rilasciata con Dynamic Media Viewers 5.13, i clienti di Dynamic Media, Adobe Analytics e Adobe Launch possono utilizzare eventi e dati specifici per i visualizzatori Dynamic Media nella propria configurazione di Adobe Launch.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 623
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 18%

---

# Utilizzo dei visualizzatori Dynamic Medie con Adobe Analytics e Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Per i clienti con Dynamic Medie e Adobe Analytics, ora puoi monitorare l’utilizzo dei visualizzatori Dynamic Medie sul tuo sito web utilizzando l’estensione Dynamic Medie Viewer.

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> Esegui Adobe Experience Manager in modalità Dynamic Medie Scene7 per questa funzionalità. È inoltre necessario [integrare Adobe Experience Platform Launch con l’istanza AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=it).

Con l’introduzione dell’estensione Dynamic Medie Viewer, Adobe Experience Manager offre ora il supporto di analisi avanzate per le risorse fornite con i visualizzatori Dynamic Medie (5.13), fornendo un controllo più granulare sul tracciamento degli eventi quando un visualizzatore Dynamic Medie viene utilizzato su una pagina Sites.

Se disponi già di AEM Assets e Sites, puoi integrare la proprietà Launch con l’istanza Autore AEM. Una volta che l’integrazione di launch è associata al sito web, puoi aggiungere alla pagina componenti Dynamic Media con il tracciamento degli eventi per i visualizzatori abilitato.

Per i clienti solo AEM Assets o Dynamic Media Classic, l’utente può ottenere il codice di incorporamento di un visualizzatore e aggiungerlo alla pagina. Le librerie di script di Launch possono quindi essere aggiunte manualmente alla pagina per il tracciamento degli eventi del visualizzatore.

Nella tabella seguente sono elencati gli eventi di Dynamic Medie Viewer e i relativi argomenti supportati:

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
         <td> ROTAZIONE </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> INTERROMPI </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SCAMBIA </td>
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

* [Integrazione di Adobe Experience Manager con Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=it)
* [Esecuzione di Adobe Experience Manager in modalità Dynamic Medie Scene7](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=en)
* [Integrazione dei visualizzatori Dynamic Media con Adobe Analytics e Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
