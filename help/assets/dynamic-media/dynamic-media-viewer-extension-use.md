---
title: Utilizzo dei visualizzatori Dynamic Media con Adobe Analytics e i tag
description: L’estensione Dynamic Media Viewers per i tag, insieme al rilascio di Dynamic Media Viewers 5.13, consente ai clienti di Dynamic Media, Adobe Analytics e tag di utilizzare eventi e dati specifici per i visualizzatori Dynamic Media nella propria configurazione di tag.
sub-product: Dynamic Media
feature: Asset Insights
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 576
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 2%

---

# Utilizzo dei visualizzatori Dynamic Media con Adobe Analytics e i tag{#using-dynamic-media-viewers-adobe-analytics-tags}

Per i clienti con Dynamic Media e Adobe Analytics, ora puoi monitorare l’utilizzo dei visualizzatori Dynamic Media sul tuo sito web utilizzando l’estensione Dynamic Media Viewer.

>[!VIDEO](https://video.tv.adobe.com/v/39143?quality=12&learn=on&captions=ita)

>[!NOTE]
>
> Esegui Adobe Experience Manager in modalità Dynamic Media Scene7 per questa funzionalità. Devi anche [integrare i tag con la tua istanza di AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=it).

Con l’introduzione dell’estensione Dynamic Media Viewer, Adobe Experience Manager offre ora il supporto di analisi avanzate per le risorse fornite con i visualizzatori Dynamic Media (5.13), fornendo un controllo più granulare sul tracciamento degli eventi quando un visualizzatore Dynamic Media viene utilizzato su una pagina Sites.

Se disponi già di AEM Assets e Sites, puoi integrare la proprietà tags con l’istanza di authoring AEM. Una volta che l’integrazione di launch è associata al sito web, puoi aggiungere alla pagina componenti Dynamic Media con il tracciamento degli eventi per i visualizzatori abilitato.

Per i clienti solo AEM Assets o Dynamic Media Classic, l’utente può ottenere il codice di incorporamento di un visualizzatore e aggiungerlo alla pagina. Le librerie di script di tag possono quindi essere aggiunte manualmente alla pagina per il tracciamento degli eventi del visualizzatore.

Nella tabella seguente sono elencati gli eventi di Dynamic Media Viewer e i relativi argomenti supportati:

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

* [Integrazione di AEM con i tag in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=it)
* [Esecuzione di Adobe Experience Manager in modalità Dynamic Media Scene7](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=it)
* [Integrazione dei visualizzatori Dynamic Media con Adobe Analytics tramite tag](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html?lang=it)
