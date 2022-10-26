---
title: Utilizzo dei video Dynamic Media 360 e delle miniature video personalizzate con AEM Assets
description: I miglioramenti al visualizzatore Dynamic Media nella AEM 6.5 includono il supporto per il rendering video 360, 360 visualizzatori multimediali (video360Social e video360VR) e la possibilità di selezionare miniature video personalizzate.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 4%

---

# Utilizzo dei video Dynamic Media 360 e delle miniature video personalizzate con AEM Assets

I miglioramenti al visualizzatore Dynamic Media nella AEM 6.5 includono il supporto per il rendering video 360, 360 visualizzatori multimediali (video360Social e video360VR) e la possibilità di selezionare miniature video personalizzate.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>Il video presuppone che l&#39;istanza AEM sia in esecuzione in modalità Dynamic Media S7.  [Le istruzioni sulla configurazione di AEM con Dynamic Media sono disponibili qui](https://helpx.adobe.com/it/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Quando carichi un video, per impostazione predefinita, Dynamic Media elabora il filmato come video 360, se ha un rapporto di formato di 2:1. Ad esempio, il rapporto larghezza/altezza è 2:1.

>[!NOTE]
>
>I componenti Media di Dynamic Media 360 supportano solo 360 video.

## Video su Dynamic Media 360

I video a 360 gradi, noti anche come video sferici, sono registrazioni video in cui una visualizzazione in ogni direzione viene registrata contemporaneamente, ripresi utilizzando una telecamera omnidirezionale o una raccolta di telecamere. Durante la riproduzione su un display piatto, l&#39;utente ha il controllo della direzione di visualizzazione e la riproduzione su dispositivi mobili solitamente si basa sul controllo giroscopio integrato.  Permette di espandersi oltre i limiti della fotografia singola. Gli addetti al marketing possono fornire agli utenti un’esperienza coinvolgente con l’aiuto di 360 video.  Cominciamo. Il criterio del rapporto di formato dell’immagine panoramica può essere modificato nella configurazione DMS7 della società specificando la doppia proprietà s7PanoramicAR in /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Video su Dynamic Media 360

Il video Dynamic Media supporta ora la possibilità di selezionare una miniatura personalizzata per il video. Un utente può selezionare una risorsa esistente da AEM Assets oppure un fotogramma video come miniatura.

## Visualizzatori Dynamic Media 360

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Visualizzatore video360Social**</td>
      <td>**Visualizzatore video360VR**</td>
   </tr>
   <tr>
      <td>Modalità di esecuzione Dynamic Media</td>
      <td>Solo modalità Dynamic Media Scene7</td>
      <td>Solo modalità Dynamic Media Scene7<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Caso d’uso </td>
      <td>
         <p>Per siti web e dispositivi che non supportano giroscopio</p>
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
      <td>Punto di navigazione</td>
      <td>
         <ul>
            <li>Trascinamento del mouse (su sistemi desktop)</li>
            <li>Swipe (dispositivi touch)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Le opzioni del mouse e del trascinamento sono disabilitate</li>
            <li>Utilizza il giroscopio integrato</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>Lettore HTML5</td>
      <td>Sì</td>
      <td>Sì</td>
   </tr>
   <tr>
      <td>Opzioni di condivisione social network</td>
      <td>Sì</td>
      <td>No</td>
   </tr>
</tbody>
</table>

## Risorse aggiuntive{#additional-resources}

[Configurazione di Dynamic Media in modalità Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
