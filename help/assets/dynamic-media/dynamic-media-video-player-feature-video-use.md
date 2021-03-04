---
title: Utilizzo del lettore video in AEM Dynamic Media
description: Il lettore video AEM Dynamic Media utilizzato per basarsi sul runtime Flash per supportare lo streaming video adattivo su client e browser desktop è diventato più aggressivo nello streaming di contenuti basato su flash. Con l’introduzione di HLS (protocollo di distribuzione video HTTP Live Streaming di Apple), ora è possibile inviare in streaming i contenuti senza ricorrere al flash.
sub-product: dynamic-media
feature: Profili video
version: 6.3, 6.4, 6.5
topic: Gestione dei contenuti
role: Professionista
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 8%

---


# Utilizzo del lettore video in AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

Il lettore video AEM Dynamic Media utilizzato per basarsi sul runtime Flash per supportare lo streaming video adattivo su client e browser desktop è diventato più aggressivo nello streaming di contenuti basato su flash. Con l’introduzione di HLS (protocollo di distribuzione video HTTP Live Streaming di Apple), ora è possibile inviare in streaming i contenuti senza ricorrere al flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Ricerca rapida sul lettore video non Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

Il supporto del browser HLS è il seguente: per i browser non supportati si basa sulla distribuzione progressiva dei video

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
   <td> <p>Chrome</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 6 o precedente)</p> </td>
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