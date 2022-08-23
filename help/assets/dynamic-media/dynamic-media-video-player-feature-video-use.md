---
title: Utilizzo del lettore video in AEM Dynamic Media
description: AEM lettore video Dynamic Media utilizzato per fare affidamento sul runtime di Flash per supportare lo streaming video adattivo su client e browser desktop è diventato più aggressivo nello streaming di contenuti basati su flash. Con l’introduzione di HLS (protocollo di distribuzione video HTTP Live Streaming Apple), ora è possibile inviare in streaming i contenuti senza ricorrere al flash.
sub-product: dynamic-media
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 6%

---


# Utilizzo del lettore video in AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM lettore video Dynamic Media utilizzato per fare affidamento sul runtime di Flash per supportare lo streaming video adattivo su client e browser desktop è diventato più aggressivo nello streaming di contenuti basati su flash. Con l’introduzione di HLS (protocollo di distribuzione video HTTP Live Streaming Apple), ora è possibile inviare in streaming i contenuti senza ricorrere al flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Anteprima nel lettore video non Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

Il supporto del browser HLS è il seguente: per i browser non supportati si basa sulla distribuzione progressiva dei video

>[!NOTE]
>
> Dynamic Media Hybrid NON supporta lo streaming video su Internet Explorer 11 a partire dal 15 marzo 2022. Aggiorna a 6.5.12 o superiore per tornare alla riproduzione progressiva su IE 11.

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
   <td> <p>Modalità Dynamic Media - Scene 7: Streaming video HLS</p> 
        <p>Dynamic Media - Modalità ibrida: Download progressivo</p>
   </td>
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
