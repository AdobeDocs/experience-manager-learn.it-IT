---
title: Panoramica di Dynamic Media con AEM Assets
description: Questa serie video offre una panoramica della modalità di gestione e accesso dei contenuti multimediali tramite Adobe Experience Manager Dynamic Media come servizio di content serving. Dynamic Media consente di gestire e pubblicare esperienze digitali dinamiche, una funzione unica di Experience Manager Assets. Il nostro framework e suite di componenti consentono agli addetti al marketing di personalizzare e fornire esperienze multimediali interattive su tutti i dispositivi.
feature: Smart Crop, Video Profiles, Image Profiles, Viewer Presets, 360 VR Video, Image Sets, Spin Sets
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 59462cb4-d379-4e58-b786-ff8dbae6191c
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '871'
ht-degree: 0%

---

# Utilizzo di Dynamic Media con AEM Assets {#understanding-aem-dynamic-media}

Questa serie video in più parti offre una panoramica della gestione e dell’accesso ai contenuti multimediali tramite Adobe Experience Manager Dynamic Media come servizio di content serving. Dynamic Media consente di gestire e pubblicare esperienze digitali dinamiche, una funzione unica di Experience Manager Assets. Il nostro framework e suite di componenti consentono agli addetti al marketing di personalizzare e fornire esperienze multimediali interattive su tutti i dispositivi.

## Panoramica di Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/27144/?quality=9&learn=on)

>[!NOTE]
>
>La funzionalità qui dimostrata è disponibile con la modalità di esecuzione Dynamic Media DMS7, la nostra modalità di esecuzione attualmente supportata, non necessariamente la modalità di esecuzione DMHybrid, che DMS7 ha sostituito.

Questo video descrive come gestire e accedere ai contenuti multimediali utilizzando Adobe Experience Manager Dynamic Media come servizio di content serving. Dynamic Media opera con una singola metodologia Master Asset in cui si carica una risorsa immagine o video che può essere richiesta per soddisfare un set illimitato di varianti di consumo necessarie o rappresentazioni derivate. Incluso:

* Spiegazione della singola risorsa principale a URL del prodotto consegnabile
* Opzioni di elaborazione delle immagini
* Opzioni del visualizzatore del contenuto
* Copiare gli URL nelle immagini e nei visualizzatori reattivi
* Varianti di ritaglio avanzato per gli URL

## Dynamic Media con AEM Sites e qualsiasi altro CMS

>[!VIDEO](https://video.tv.adobe.com/v/27145/?quality=9&learn=on)

>[!NOTE]
>
>La funzionalità qui dimostrata è disponibile con la modalità di esecuzione Dynamic Media DMS7, la nostra modalità di esecuzione attualmente supportata, non necessariamente la modalità di esecuzione DMHybrid, che DMS7 ha sostituito. Questo video fa riferimento ai concetti descritti nella parte 1 del video (Panoramica di Dynamic Media).

Questo video descrive come i contenuti multimediali vengono gestiti in Adobe Experience Manager Dynamic Media e possono essere facilmente utilizzati in AEM Sites, con un componente, per essere semplici e ritagliati automaticamente in modo da ottimizzarli in base alla larghezza reattiva della pagina. Crea facilmente un banner immagine interattivo e genera l’URL della copia da utilizzare in qualsiasi sistema di gestione dei contenuti.

* Flessibilità dei componenti di AEM Sites Dynamic Media
* Download locale con predefiniti immagine
* Creazione e pubblicazione di un banner interattivo

## Dynamic Media per la creazione di raccolte di file multimediali diversi e visualizzatore personalizzato

>[!VIDEO](https://video.tv.adobe.com/v/27146/?quality=9&learn=on)

>[!NOTE]
>
>La funzionalità qui dimostrata è disponibile con la modalità di esecuzione Dynamic Media DMS7, la nostra modalità di esecuzione attualmente supportata, non necessariamente la modalità di esecuzione DMHybrid, che DMS7 ha sostituito. Questo video fa riferimento ai concetti descritti nella parte 1 del video (Panoramica di Dynamic Media).

Questo video descrive il semplice processo di creazione di una raccolta di risorse multimediali per visualizzatori di file multimediali diversi, tra cui un set 360 gradi, un video e una raccolta di immagini di prodotto. Aggiungi contenuti al set di file multimediali diversi e crea un visualizzatore personalizzato tra cui scegliere nell’URL di copia finale o nel componente AEM Sites.

* Crea set 360 gradi da foto di prodotto appropriate
* Caricare e codificare video master per video Dynamic Media
* Crea set di file multimediali diversi da set 360 gradi, video e foto
* Modificare e utilizzare un visualizzatore di file multimediali diversi personalizzato

## Predefiniti immagini Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/27320/?quality=9&learn=on)

Questo video descrive come vengono creati i predefiniti per immagini e cos’è un predefinito per immagini, un abbreviatore di URL per una serie di argomenti server immagini che operano su un’immagine ogni volta che un URL la richiede. Scopri le tecniche utili per estendere e modificare i predefiniti per immagini.

* Raccolta abbreviata del predefinito per immagini che nasconde la raccolta di comandi espliciti di Image Server
* Utilizza una sola dimensione pixel - larghezza O altezza - per adattare nuove immagini ridimensionate senza spaziatura
* Usa sempre nitidezza
* Campo modificatore URL per aggiungere ulteriori comandi per il ridimensionamento del predefinito immagine

## Modificatori URL avanzati Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/27319/?quality=9&learn=on)

Questo video descrive come andare oltre il ridimensionamento delle immagini per sfruttare le funzioni del file sorgente stesso - trasparenza di sfondo, tracciati di ritaglio incorporati e ritagli e testo come variabili - con i modificatori URL di Dynamic Media.

* Utilizzo dei modificatori URL nel campo modificatore di Dynamic Media
* Modifica del colore di sfondo sulle immagini con trasparenza
* Ritaglio di un tracciato immagine
* Ritaglio in un percorso immagine
* Creazione di un modello di testo da un file Photoshop

## Controllo Dynamic Media delle dimensioni dei file JPEG in Kilobytes

>[!VIDEO](https://video.tv.adobe.com/v/27404/?quality=9&learn=on)


>[!NOTE]
>
>La qualità dell&#39;immagine viene misurata in percentuali di compressione inversa, dove il 100% di qualità è meno compresso e ciò determina immagini di alta qualità ma di dimensioni relativamente grandi. La compressione Jpeg è uno schema di compressione con perdita di dati in cui le impostazioni di compressione determinano la qualità dell&#39;immagine e le dimensioni del file.

Bilancia la qualità dell&#39;immagine jpeg rispetto alle dimensioni del file risultante (in kilobyte) per migliorare la velocità di caricamento della pagina, utilizzando 2 comandi per regolare le impostazioni di compressione jpeg. QLT definisce la qualità dell&#39;immagine regolando le impostazioni di qualità della compressione jpeg. Il comando Dimensione di JPEG consente di specificare la dimensione del file da ottenere utilizzando la compressione.

## Aggiungere sottotitoli codificati in video Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/28074/?quality=9&learn=on)

Aggiungi facilmente i sottotitoli codificati al video Dynamic Media aggiungendo l’URL della copia per puntare a un ulteriore documento del file sottotitoli codificati, un file sidecar web.VTT, contenente le informazioni CC per qualsiasi video.

## Utilizzo della nitidezza delle immagini con AEM Dynamic Media

Questo video spiega perché la nitidezza di un&#39;immagine è fondamentale per mantenere la fedeltà delle immagini e come utilizzare impostazioni avanzate per creare l&#39;immagine perfetta.

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
