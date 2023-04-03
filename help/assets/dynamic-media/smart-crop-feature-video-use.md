---
title: Utilizzo del ritaglio avanzato con AEM Assets Dynamic Media
description: Smart Crop utilizza Adobe Sensei per eliminare le attività dispendiose in termini di tempo e costi legate al ritaglio dei contenuti per un design reattivo.
feature: Smart Crop, Image Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 295bbfb6-241f-41c0-972d-d9688863cea1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 2%

---

# Utilizzo del ritaglio avanzato con AEM Assets Dynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop utilizza Adobe Sensei per eliminare le attività dispendiose in termini di tempo e costi legate al ritaglio dei contenuti per un design reattivo.

>[!VIDEO](https://video.tv.adobe.com/v/21519?quality=12&learn=on)

>[!NOTE]
>
>Il video presuppone che l&#39;istanza AEM sia in esecuzione in modalità Dynamic Media S7. [Le istruzioni sulla configurazione di AEM con Dynamic Media sono disponibili qui.](https://helpx.adobe.com/it/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## La funzionalità di ritaglio avanzato Dynamic Media di Adobe Experience Manager include

* Gli amministratori delle risorse AEM possono creare facilmente profili immagine per il ritaglio avanzato in base alla larghezza e all’altezza del dispositivo.
* Il ritaglio avanzato può essere eseguito per una singola risorsa o per tutte le risorse all’interno di una cartella.
* Il layout di modifica avanzata del ritaglio può essere ridimensionato per una migliore visibilità.
* Il componente Dynamic Media di AEM Sites supporta il ritaglio avanzato.
* L’URL pubblicato per la risorsa ritagliata avanzata è disponibile per l’utilizzo con applicazioni di terze parti che accettano le risorse in hosting.

>[!NOTE]
>
>Le coordinate di ritaglio avanzato dipendono dalle proporzioni. In altre parole, per le varie impostazioni di ritaglio avanzato in un profilo immagine, se le proporzioni sono le stesse per le dimensioni aggiunte nel profilo immagine, le stesse proporzioni vengono inviate a Dynamic Media. Per questo motivo, nell’Editor ritaglio avanzato viene suggerita la stessa area di ritaglio. Ad esempio, un’impostazione di ritaglio di 100x100 e 200x200 comporterebbe la generazione dello stesso ritaglio avanzato da parte del sistema.
