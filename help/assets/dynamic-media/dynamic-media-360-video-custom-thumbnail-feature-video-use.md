---
title: Utilizzo di video Dynamic Medie 360 e miniatura video personalizzata con AEM Assets
description: I miglioramenti apportati al visualizzatore Dynamic Medie nell'AEM 6.5 includono il supporto per il rendering video 360, 360 visualizzatori di contenuti multimediali (video360Social e video360VR) e la possibilità di selezionare miniature video personalizzate.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 691
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 3%

---

# Utilizzo di video Dynamic Medie 360 e miniatura video personalizzata con AEM Assets

I miglioramenti apportati al visualizzatore Dynamic Medie nell&#39;AEM 6.5 includono il supporto per il rendering video 360, 360 visualizzatori di contenuti multimediali (video360Social e video360VR) e la possibilità di selezionare miniature video personalizzate.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>Video presuppone che l’istanza AEM sia in esecuzione in modalità Dynamic Medie S7.  [Le istruzioni per la configurazione di AEM con Dynamic Medie sono disponibili qui](https://helpx.adobe.com/it/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Quando caricate un video, per impostazione predefinita, Dynamic Medie elabora il metraggio come video 360, se ha proporzioni 2:1. ovvero un rapporto larghezza/altezza di 2:1.

>[!NOTE]
>
>I componenti Dynamic Medie 360 Media supportano solo video 360.

## Video su Dynamic Medie 360

I video a 360 gradi, noti anche come video sferici, sono registrazioni video in cui una visione in ogni direzione viene registrata contemporaneamente, girata utilizzando una fotocamera omnidirezionale o una raccolta di telecamere. Durante la riproduzione su uno schermo piatto, l&#39;utente ha il controllo della direzione di visualizzazione, e la riproduzione su dispositivi mobili in genere sfrutta il controllo integrato del giroscopio.  Ti consente di andare oltre i limiti della fotografia singola. Gli addetti al marketing possono fornire agli utenti un’esperienza coinvolgente con l’aiuto di 360 video.  Cominciamo. Il criterio delle proporzioni dell’immagine panoramica può essere modificato nella configurazione DMS7 dell’azienda specificando la doppia proprietà s7PanoramicAR in /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Video su Dynamic Medie 360

Il video Dynamic Medie ora supporta la possibilità di selezionare una miniatura personalizzata per il video. L’utente può selezionare una risorsa esistente da AEM Assets oppure selezionare un fotogramma video come miniatura.

## Visualizzatori Dynamic 360 Media

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Visualizzatore social network**</td>
      <td>**Visualizzatore Video360VR**</td>
   </tr>
   <tr>
      <td>Modalità di esecuzione Dynamic Medie</td>
      <td>Solo modalità Scene7 Dynamic Medie</td>
      <td>Solo modalità Scene7 Dynamic Medie<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Caso d’uso</td>
      <td>
         <p>Per siti Web e dispositivi che non supportano il giroscopio</p>
         <p> </p>
      </td>
      <td>
         <p>Fornisce un'esperienza di realtà virtuale per un dispositivo che supporta il giroscopio </p>
      </td>
   </tr>
   <tr>
      <td>Audio - Modalità stereo</td>
      <td>No</td>
      <td>Sì</td>
   </tr>
   <tr>
      <td>Riproduzione video</td>
      <td>Sì</td>
      <td>Sì</td>
   </tr>
   <tr>
      <td>Navigazione punto di vista</td>
      <td>
         <ul>
            <li>Trascinamento del mouse (su sistemi desktop)</li>
            <li>Scorrimento rapido (dispositivi touch)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Le opzioni di trascinamento e mouse sono disattivate</li>
            <li>Utilizza un giroscopio integrato</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>Lettore HTML5</td>
      <td>Sì</td>
      <td>Sì</td>
   </tr>
   <tr>
      <td>Opzioni di condivisione social</td>
      <td>Sì</td>
      <td>No</td>
   </tr>
</tbody>
</table>

## Risorse aggiuntive{#additional-resources}

[Configurazione di Dynamic Medie in modalità Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
