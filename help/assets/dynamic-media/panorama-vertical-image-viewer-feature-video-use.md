---
title: Utilizzo di visualizzatori panoramici e verticali di immagini con AEM Assets Dynamic Media
description: I miglioramenti apportati al visualizzatore Dynamic Media in AEM 6.4 includono l'aggiunta del visualizzatore di immagini panoramiche, del visualizzatore di immagini panoramiche di realtà virtuale e del visualizzatore di immagini verticali. Il visualizzatore panoramico offre un modo semplice per offrire un'esperienza coinvolgente e coinvolgente della stanza, della proprietà, della posizione o del paesaggio senza alcuno sviluppo personalizzato.
feature: Video Profiles, Video Profiles, 360 VR Video
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
duration: 535
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# Utilizzo di visualizzatori panoramici e verticali di immagini con AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

I miglioramenti apportati al visualizzatore Dynamic Media in AEM 6.4 includono l&#39;aggiunta del visualizzatore di immagini panoramiche, del visualizzatore di immagini panoramiche di realtà virtuale e del visualizzatore di immagini verticali. Il visualizzatore panoramico offre un modo semplice per offrire un&#39;esperienza coinvolgente e coinvolgente della stanza, della proprietà, della posizione o del paesaggio senza alcuno sviluppo personalizzato.

>[!VIDEO](https://video.tv.adobe.com/v/327278?quality=12&learn=on&captions=ita)

>[!NOTE]
>
>Video presuppone che l’istanza di AEM sia in esecuzione in modalità Dynamic Media S7. [Le istruzioni per la configurazione di AEM con Dynamic Media sono disponibili qui.](https://helpx.adobe.com/it/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visualizzatore VR panoramico e panoramico

Un’immagine è considerata panoramica in base alle sue proporzioni o parole chiave. Per impostazione predefinita, un’immagine con proporzioni pari a 2 è considerata come immagine panoramica. I predefiniti visualizzatore immagini panoramiche diventano disponibili per l&#39;anteprima di un&#39;immagine se soddisfa i criteri di cui sopra. Il criterio delle proporzioni dell’immagine panoramica può essere modificato nella configurazione DMS7 dell’azienda specificando la doppia proprietà s7PanoramicAR in /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Le parole chiave vengono memorizzate nella proprietà dc:keyword del nodo di metadati della risorsa. Se le parole chiave contengono una delle seguenti combinazioni:

* equirettangolare,
* sferico + panoramico,
* sferico + panorama,

è considerata una risorsa Immagine panoramica indipendentemente dalle sue proporzioni.

## Visualizzatore immagini verticale

Con i campioni orizzontali, a seconda delle dimensioni dello schermo del desktop del consumatore, a volte i campioni non sono visibili fino a quando l’utente non scorre la pagina verso il basso. Il visualizzatore di immagini verticale e il posizionamento di campioni verticali garantiscono la visibilità dei campioni indipendentemente dalle dimensioni dello schermo. Inoltre, ingrandisce le dimensioni dell&#39;immagine principale. Con i campioni orizzontali, era necessario riservare spazio sulla pagina per garantire che avessero un’alta probabilità di essere visibili e che diminuissero le dimensioni dell’immagine principale. Con un layout verticale, non dovrete preoccuparvi di allocare questo spazio e quindi potete massimizzare la dimensione dell&#39;immagine principale.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visualizzatore panoramico e VR</td>
   <td>Visualizzatore immagini verticale</td>
  </tr>
  <tr>
   <td>Modalità di esecuzione Dynamic Media</td>
   <td>Solo modalità Dynamic Media Scene7</td>
   <td>DMS7 e Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso d’uso</td>
   <td><p>Il visualizzatore panoramico e il visualizzatore realtà virtuale offrono agli utenti un’esperienza più coinvolgente. Un utente può prenotare una camera d’albergo anche prima di effettuare una prenotazione, o controllare una proprietà in affitto senza dover fissare un appuntamento. Un utente può controllare una posizione e molte altre possibilità. In questo caso, l’obiettivo principale è fornire al consumatore un’esperienza migliore quando visita il sito web e alla fine aumenta il tasso di conversione.</p> <p> </p> </td> 
   <td><p>Il visualizzatore di immagini verticale aiuta a massimizzare l’esperienza di visualizzazione delle immagini del prodotto per fornire ai consumatori la migliore rappresentazione del prodotto, favorendo in tal modo la conversione e riducendo al minimo i rendimenti.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponibile </td>
   <td>PRECONFIGURATO</td>
   <td>PRECONFIGURATO</td>
  </tr>
 </tbody>
</table>

[Configurazione di Dynamic Media in modalità Scene7](https://helpx.adobe.com/it/experience-manager/6-5/assets/using/config-dms7.html)

[Configurazione di Dynamic Media in modalità ibrida](https://helpx.adobe.com/it/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>I visualizzatori panoramici funzionano con immagini panoramiche e non sono destinati a essere utilizzati con immagini normali.
