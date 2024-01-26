---
title: Utilizzo del lettore video nel Dynamic Medie dell’AEM
description: Il lettore video AEM Dynamic Medie si basava su runtime di Flash per supportare lo streaming video adattivo su client desktop e browser, che diventava più aggressivo con lo streaming di contenuti basati su flash. Con l’introduzione di HLS (HTTP Live Streaming video delivery protocol di Apple), il contenuto può ora essere inviato in streaming senza ricorrere al flash.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
duration: 608
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# Utilizzo del lettore video nel Dynamic Medie dell’AEM{#using-the-video-player-in-aem-dynamic-media}

Il lettore video AEM Dynamic Medie si basava su runtime di Flash per supportare lo streaming video adattivo su client desktop e browser, che diventava più aggressivo con lo streaming di contenuti basati su flash. Con l’introduzione di HLS (HTTP Live Streaming video delivery protocol di Apple), il contenuto può ora essere inviato in streaming senza ricorrere al flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## Ricerca rapida nel lettore video non di Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

Il supporto del browser HLS è il seguente: per i browser non supportati si utilizza la distribuzione progressiva di video

>[!NOTE]
>
> Dynamic Medie Hybrid NON supporta lo streaming video su Internet Explorer 11 dal 15 marzo 2022. Effettua l’aggiornamento a 6.5.12 o versione successiva per tornare alla riproduzione progressiva su IE 11.

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
   <td> <p>Dynamic Medie - Modalità Scene 7: streaming video HLS</p> 
        <p>Dynamic Medie - Modalità ibrida: download progressivo</p>
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
   <td> <p>Chrome (Android 6 o versioni precedenti)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 7 o versione successiva)</p> </td>
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
