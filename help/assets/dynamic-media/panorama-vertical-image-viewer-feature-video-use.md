---
title: Utilizzo del visualizzatore di immagini panoramiche e verticali con  AEM Assets Dynamic Media
seo-title: Utilizzo del visualizzatore di immagini panoramiche e verticali con  AEM Assets Dynamic Media
description: I miglioramenti apportati al visualizzatore per contenuti multimediali dinamici nella AEM 6.4 includono il visualizzatore per immagini panoramiche, il visualizzatore per immagini panoramiche per realtà virtuale e il visualizzatore per immagini verticali. Il visualizzatore panoramico fornisce un modo semplice per offrire un'esperienza coinvolgente e coinvolgente di stanza, proprietà, posizione o paesaggio senza alcuno sviluppo personalizzato.
seo-description: I miglioramenti apportati al visualizzatore per contenuti multimediali dinamici nella AEM 6.4 includono il visualizzatore per immagini panoramiche, il visualizzatore per immagini panoramiche per realtà virtuale e il visualizzatore per immagini verticali. Il visualizzatore panoramico fornisce un modo semplice per offrire un'esperienza coinvolgente e coinvolgente di stanza, proprietà, posizione o paesaggio senza alcuno sviluppo personalizzato.
sub-product: dynamic-media
feature: video-profiles, video-profiles, vr-360
topics: videos, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# Utilizzo del visualizzatore di immagini panoramiche e verticali con  AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

I miglioramenti apportati al visualizzatore per contenuti multimediali dinamici nella AEM 6.4 includono il visualizzatore per immagini panoramiche, il visualizzatore per immagini panoramiche per realtà virtuale e il visualizzatore per immagini verticali. Il visualizzatore panoramico fornisce un modo semplice per offrire un&#39;esperienza coinvolgente e coinvolgente di stanza, proprietà, posizione o paesaggio senza alcuno sviluppo personalizzato.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>Il video presuppone che l’istanza AEM sia in esecuzione in modalità Dynamic Media S7. [Le istruzioni sulla configurazione AEM con Dynamic Media sono disponibili qui.](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visualizzatore VR panoramico e panoramico

Un’immagine viene considerata panoramica in base alle sue proporzioni o parole chiave. Per impostazione predefinita, un’immagine con proporzioni pari a 2 viene considerata un’immagine panoramica. I predefiniti per visualizzatori di immagini panoramiche saranno disponibili per un’anteprima delle immagini se soddisfano i criteri di cui sopra. Il criterio del rapporto di formato dell’immagine panoramica può essere modificato nella configurazione DMS7 della società specificando la proprietà doppia s7PanoramicAR in /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Le parole chiave sono memorizzate nella proprietà dc:keyword del nodo di metadati della risorsa. Se le parole chiave contengono una delle seguenti combinazioni:

* equirettangolare,
* sferico + panoramico,
* sferico + panorama,

sarà considerata una risorsa immagine panoramica, a prescindere dalle sue proporzioni.

## Visualizzatore immagini verticale

Con i campioni orizzontali, a seconda delle dimensioni dello schermo del desktop del consumatore, a volte i campioni non sarebbero visibili finché l’utente non scorre la pagina verso il basso. Usando il visualizzatore immagine verticale e posizionando i campioni verticali, i campioni vengono visualizzati indipendentemente dalle dimensioni dello schermo. Consente inoltre di massimizzare le dimensioni dell’immagine principale. Con i campioni orizzontali, era necessario riservare spazio sulla pagina per assicurare che avessero un’elevata probabilità di essere visibili e che riducessero le dimensioni dell’immagine principale. Con un layout verticale, non è necessario preoccuparsi di allocare questo spazio e pertanto è possibile massimizzare le dimensioni dell&#39;immagine principale.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visualizzatore panoramico e VR</td>
   <td>Visualizzatore immagini verticale</td>
  </tr>
  <tr>
   <td>Modalità esecuzione elementi multimediali dinamici</td>
   <td>Solo modalità Scene7 per contenuti multimediali dinamici</td>
   <td>DMS7 e Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso d’uso </td>
   <td><p>Il visualizzatore panoramico e il visualizzatore Virtual Reality offrono agli utenti un'esperienza più coinvolgente. Un utente può controllare una stanza d'albergo anche prima di effettuare una prenotazione, o controllare una proprietà di affitto senza dover pianificare un appuntamento. Un utente può controllare una posizione e molte altre possibilità. L'obiettivo principale qui è quello di fornire al consumatore un'esperienza migliore quando visita il tuo sito Web e alla fine aumentare il tasso di conversione.</p> <p> </p> </td> 
   <td><p>Il visualizzatore per immagini verticali consente di massimizzare l'esperienza di visualizzazione delle immagini dei prodotti per offrire ai consumatori la migliore rappresentazione del prodotto, promuovendo la conversione e riducendo al minimo i rendimenti.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponibile </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configurazione di contenuti multimediali dinamici in modalità Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Configurazione di Dynamic Media in modalità ibrida](https://helpx.adobe.com/it/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>I visualizzatori panoramici utilizzano immagini panoramiche e non sono utilizzabili con immagini normali.
