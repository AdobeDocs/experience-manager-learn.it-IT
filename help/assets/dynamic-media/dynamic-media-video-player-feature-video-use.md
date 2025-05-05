---
title: Utilizzo del lettore video in AEM Dynamic Media
description: Il lettore video AEM Dynamic Media, utilizzato per supportare lo streaming video adattivo su client desktop e browser, era più aggressivo nello streaming di contenuti basati su flash. Con l’introduzione di HLS (HTTP Live Streaming video delivery protocol di Apple), il contenuto può ora essere inviato in streaming senza ricorrere al flash.
feature: Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
duration: 568
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# Utilizzo del lettore video in AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

Il lettore video AEM Dynamic Media, utilizzato per supportare lo streaming video adattivo su client desktop e browser, era più aggressivo nello streaming di contenuti basati su flash. Con l’introduzione di HLS (HTTP Live Streaming video delivery protocol di Apple), il contenuto può ora essere inviato in streaming senza ricorrere al flash.

>[!VIDEO](https://video.tv.adobe.com/v/41666?quality=12&learn=on&captions=ita)

## Ricerca rapida in Video Player non Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

Di seguito è riportato il supporto del browser HLS. Per i browser non supportati viene eseguito il fallback alla distribuzione progressiva dei video

>[!NOTE]
>
> Dynamic Media Hybrid NON supporta lo streaming video su Internet Explorer 11 dal 15 marzo 2022. Effettua l’aggiornamento a 6.5.12 o versione successiva per tornare alla riproduzione progressiva su IE 11.

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
   <td> <p>Dynamic Media - Modalità Scene 7: streaming video HLS</p> 
        <p>Dynamic Media - Modalità ibrida: download progressivo</p>
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
   <td> <p>Dispositivo mobile</p> </td>
   <td> <p>Chrome (Android 6 o versioni precedenti)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo mobile</p> </td>
   <td> <p>Chrome (Android 7 o versione successiva)</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo mobile</p> </td>
   <td> <p>Android (browser predefinito)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo mobile</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo mobile</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo mobile</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>Streaming video HLS</p> </td>
  </tr>
 </tbody>
</table>
