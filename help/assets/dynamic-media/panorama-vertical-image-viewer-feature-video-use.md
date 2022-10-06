---
title: Utilizzo dei visualizzatori per immagini panoramiche e verticali con AEM Assets Dynamic Media
description: I miglioramenti apportati al visualizzatore Dynamic Media in AEM 6.4 includono l’aggiunta di visualizzatore di immagini panoramiche, visualizzatore di immagini panoramiche per realtà virtuale e visualizzatore di immagini verticali. Panoramic Viewer offre un modo semplice per offrire un'esperienza coinvolgente e coinvolgente della stanza, della proprietà, della posizione o del paesaggio senza alcun sviluppo personalizzato.
sub-product: dynamic-media
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 2%

---

# Utilizzo dei visualizzatori per immagini panoramiche e verticali con AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

I miglioramenti apportati al visualizzatore Dynamic Media in AEM 6.4 includono l’aggiunta di visualizzatore di immagini panoramiche, visualizzatore di immagini panoramiche per realtà virtuale e visualizzatore di immagini verticali. Panoramic Viewer offre un modo semplice per offrire un&#39;esperienza coinvolgente e coinvolgente della stanza, della proprietà, della posizione o del paesaggio senza alcun sviluppo personalizzato.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>Il video presuppone che l&#39;istanza AEM sia in esecuzione in modalità Dynamic Media S7. [Le istruzioni sulla configurazione di AEM con Dynamic Media sono disponibili qui.](https://helpx.adobe.com/it/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visualizzatore VR panoramico e panoramico

Un’immagine viene considerata panoramica in base alle sue proporzioni o parole chiave. Per impostazione predefinita, un&#39;immagine con proporzioni pari a 2 viene considerata un&#39;immagine panoramica. I predefiniti per visualizzatori di immagini panoramiche diventano disponibili per un’anteprima dell’immagine se soddisfano i criteri di cui sopra. Il criterio del rapporto di formato dell’immagine panoramica può essere modificato nella configurazione DMS7 della società specificando la doppia proprietà s7PanoramicAR in /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Le parole chiave sono memorizzate nella proprietà dc:keyword del nodo di metadati della risorsa. Se le parole chiave contengono una delle seguenti combinazioni:

* equirettangolare,
* sferico + panoramico,
* sferico + panorama,

viene considerata una risorsa immagine panoramica indipendentemente dalle sue proporzioni.

## Visualizzatore di immagini verticali

Con i campioni orizzontali, a seconda delle dimensioni dello schermo del desktop del consumatore, a volte i campioni non sarebbero visibili finché l’utente non scorre la pagina verso il basso. Utilizzando il visualizzatore di immagini verticale e posizionando i campioni verticali, i campioni vengono visualizzati indipendentemente dalle dimensioni dello schermo. Inoltre, massimizza le dimensioni dell&#39;immagine principale. Con i campioni orizzontali, era necessario riservare spazio sulla pagina per garantire che avessero un’alta probabilità di essere visibili e avrebbe ridotto le dimensioni dell’immagine principale. Con un layout verticale, non è necessario preoccuparsi di allocare questo spazio e quindi può massimizzare le dimensioni dell&#39;immagine principale.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visualizzatore panoramico e VR</td>
   <td>Visualizzatore di immagini verticali</td>
  </tr>
  <tr>
   <td>Modalità di esecuzione Dynamic Media</td>
   <td>Solo modalità Dynamic Media Scene7</td>
   <td>DMS7 e Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso d’uso </td>
   <td><p>Il visualizzatore panoramico e il visualizzatore Virtual Reality forniscono agli utenti un’esperienza più coinvolgente. Un utente può controllare una stanza d'albergo anche prima di effettuare una prenotazione, o controllare una proprietà di noleggio senza dover pianificare un appuntamento. Un utente può controllare una posizione e molte altre possibilità. L'obiettivo principale qui è quello di fornire al consumatore un'esperienza migliore quando visita il tuo sito web e alla fine aumentare il tasso di conversione.</p> <p> </p> </td> 
   <td><p>Il visualizzatore di immagini verticali consente di massimizzare l'esperienza di visualizzazione delle immagini dei prodotti per offrire ai consumatori la migliore rappresentazione del prodotto, promuovendo in tal modo la conversione e minimizzando i ritorni.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponibile </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configurazione di Dynamic Media in modalità Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Configurazione di Dynamic Media in modalità ibrida](https://helpx.adobe.com/it/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>I visualizzatori panoramici lavorano con immagini panoramiche e non sono destinati ad essere utilizzati con immagini normali.
