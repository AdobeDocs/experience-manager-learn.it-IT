---
title: Utilizzo del lettore video in AEM file multimediali dinamici
seo-title: Utilizzo del lettore video in AEM file multimediali dinamici
description: AEM lettore video Dynamic Media utilizzato per basarsi sul runtime di Flash per supportare lo streaming di video adattivi su client e browser desktop è diventato più aggressivo con lo streaming di contenuti basati su Flash. Con l'introduzione di HLS (il protocollo di distribuzione video HTTP Live Streaming di Apple), ora è possibile trasmettere in streaming i contenuti senza ricorrere al flash.
seo-description: AEM lettore video Dynamic Media utilizzato per basarsi sul runtime di Flash per supportare lo streaming di video adattivi su client e browser desktop è diventato più aggressivo con lo streaming di contenuti basati su Flash. Con l'introduzione di HLS (il protocollo di distribuzione video HTTP Live Streaming di Apple), ora è possibile trasmettere in streaming i contenuti senza ricorrere al flash.
uuid: aac6f471-4bed-4773-890f-0dd2ceee381d
discoiquuid: b01cc46b-ef64-4db9-b3b4-52d3f27bddf5
sub-product: dynamic-media
feature: media-player, video-profiles
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 5%

---


# Utilizzo del lettore video in AEM elemento multimediale dinamico{#using-the-video-player-in-aem-dynamic-media}

AEM lettore video Dynamic Media utilizzato per basarsi sul runtime di Flash per supportare lo streaming di video adattivi su client e browser desktop è diventato più aggressivo con lo streaming di contenuti basati su Flash. Con l&#39;introduzione di HLS (il protocollo di distribuzione video HTTP Live Streaming di Apple), ora è possibile trasmettere in streaming i contenuti senza ricorrere al flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Panoramica rapida del lettore video non Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

Il supporto del browser HLS è il seguente: per i browser non supportati, si torna alla distribuzione progressiva dei video

<table> 
 <thead> 
  <tr> 
   <th> <p>Dispositivo</p> </th>
   <th> <p>Browser</p> </th>
   <th > <p>Modalità di riproduzione video</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Internet Explorer 9 e 10</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr>
   <td> <p>Desktop</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr>
   <td> <p>Desktop</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Firefox 45 o versione successiva</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Effetto cromatura</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 6 o versione precedente)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 7 o successivo)</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Android (browser predefinito)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
 </tbody>
</table>