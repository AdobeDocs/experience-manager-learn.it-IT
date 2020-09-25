---
title: Utilizzo di video Dynamic Media 360 e miniatura video personalizzata con  AEM Assets
seo-title: Utilizzo di video Dynamic Media 360 e miniatura video personalizzata con  AEM Assets
description: I miglioramenti apportati al visualizzatore per contenuti multimediali dinamici nella AEM 6.5 includono il supporto per il rendering video 360, i visualizzatori per contenuti multimediali 360 (video360Social e video360VR) e la possibilità di selezionare miniature video personalizzate.
seo-description: I miglioramenti apportati al visualizzatore per contenuti multimediali dinamici nella AEM 6.5 includono il supporto per il rendering video 360, i visualizzatori per contenuti multimediali 360 (video360Social e video360VR) e la possibilità di selezionare miniature video personalizzate.
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: dynamic-media
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# Utilizzo di video Dynamic Media 360 e miniatura video personalizzata con  AEM Assets

I miglioramenti apportati al visualizzatore per contenuti multimediali dinamici nella AEM 6.5 includono il supporto per il rendering video 360, i visualizzatori per contenuti multimediali 360 (video360Social e video360VR) e la possibilità di selezionare miniature video personalizzate.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>Il video presuppone che l’istanza AEM sia in esecuzione in modalità Dynamic Media S7.  [Le istruzioni sulla configurazione AEM con Dynamic Media sono disponibili qui](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Quando caricate un video, per impostazione predefinita viene elaborato come video 360 il contenuto multimediale dinamico, se ha proporzioni pari a 2:1. ad esempio, il rapporto larghezza/altezza è 2:1.

>[!NOTE]
>
>I componenti per contenuti multimediali dinamici 360 supportano solo 360 video.

## Video Dynamic Media 360

I video a 360 gradi, noti anche come video sferici, sono registrazioni video in cui viene contemporaneamente registrata una visualizzazione in ogni direzione, ripresi utilizzando una telecamera omnidirezionale o una raccolta di telecamere. Durante la riproduzione su uno schermo piatto, l&#39;utente ha il controllo della direzione di visualizzazione, e la riproduzione su dispositivi mobili solitamente utilizza il controllo giroscopio integrato.  Consente di espandere oltre i limiti della fotografia singola. Gli addetti al marketing possono fornire agli utenti un&#39;esperienza coinvolgente grazie all&#39;aiuto di 360 video.  Cominciamo. Il criterio del rapporto di formato dell’immagine panoramica può essere modificato nella configurazione DMS7 della società specificando la proprietà doppia s7PanoramicAR in /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Video Dynamic Media 360

I video per contenuti multimediali dinamici ora supportano la possibilità di selezionare una miniatura personalizzata per il video. Un utente può selezionare una risorsa esistente da  AEM Assets o selezionare un fotogramma video come miniatura.

## Visualizzatori per contenuti multimediali dinamici 360

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Visualizzatore video360VR**</td>
   </tr>
   <tr>
      <td>Modalità esecuzione elementi multimediali dinamici</td>
      <td>Solo modalità Scene7 per contenuti multimediali dinamici</td>
      <td>Solo modalità Scene7 per contenuti multimediali dinamici<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Caso d’uso </td>
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
      <td>Navigazione nel punto di vista</td>
      <td>
         <ul>
            <li>Trascinamento del mouse (sui sistemi desktop)</li>
            <li>Scorrimento (dispositivi touch)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Le opzioni relative al mouse e al trascinamento sono disattivate</li>
            <li>Utilizza il giroscopio incorporato</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>Lettore HTML5</td>
      <td>Sì</td>
      <td>Sì</td>
   </tr>
   <tr>
      <td>Opzioni di condivisione per social network</td>
      <td>Sì</td>
      <td>No</td>
   </tr>
</tbody>
</table>

## Risorse aggiuntive{#additional-resources}

[Configurazione di contenuti multimediali dinamici in modalità Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
