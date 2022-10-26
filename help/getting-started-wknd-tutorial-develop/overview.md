---
title: Guida introduttiva ad AEM Sites - Esercitazione WKND
description: Scopri come implementare un sito AEM per un brand fittizio di lifestyle denominato WKND. Scopri tutti gli argomenti di Experience Manager fondamentali come la configurazione del progetto, gli archetipi Maven, i componenti core, i modelli modificabili, le librerie client e lo sviluppo dei componenti.
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 3%

---

# Guida introduttiva ad AEM Sites - Esercitazione WKND {#introduction}

Ti diamo il benvenuto in un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato Adobe Experience Manager (AEM). Questa esercitazione illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio, il WKND. L’esercitazione tratta argomenti fondamentali come la configurazione del progetto, i componenti core, i modelli modificabili, le librerie lato client e lo sviluppo dei componenti con Adobe Experience Manager Sites.

## Panoramica {#wknd-tutorial-overview}

L’obiettivo di questa esercitazione in più parti è quello di insegnare agli sviluppatori come implementare un sito web utilizzando gli standard e le tecnologie più recenti in Adobe Experience Manager (AEM). Dopo aver completato questa esercitazione, uno sviluppatore deve comprendere le basi di base della piattaforma e i pattern di progettazione comuni in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Opzioni per l’avvio di un progetto Sites

Esistono due approcci di base all’avvio di un progetto AEM Sites.

**Archetipo di progetto AEM** - Approccio tradizionale allo sviluppo AEM generando un progetto AEM minimo utilizzando un modello Maven. Questo è l’approccio consigliato per progetti AEM 6.5/6.4 e AEM progetti as a Cloud Service che anticipano una forte personalizzazione. L’esercitazione offre un’immersione più approfondita nello sviluppo AEM .

[Avvia l’esercitazione con AEM Project Archetype](./project-archetype/overview.md)

**Modelli di sito AEM** - nota anche come Creazione rapida di siti, approccio a basso codice per generare un sito AEM utilizzando un modello di sito predefinito. Utilizza i componenti e i modelli predefiniti per rendere rapidamente operativo un sito. Utilizza un flusso di lavoro a tema per applicare stili e personalizzazioni specifici del brand con solo CSS e JavaScript. Consigliato per nuovi progetti e sviluppatori. Disponibile solo per AEM as a Cloud Service.

[Avvia l’esercitazione utilizzando un modello di sito](./site-template/create-site.md)

## Kit interfaccia utente Adobe XD

Per avvicinare questa esercitazione a uno scenario reale, i designer UX di talento di Adobe hanno creato i modelli per il sito utilizzando [Adobe XD](https://www.adobe.com/products/xd.html). Nel corso dell&#39;esercitazione, diverse parti delle progettazioni vengono implementate in un sito AEM completamente modificabile. Ringraziamenti speciali **Lorenzo Buosi** e **Kilian Amendola** che ha creato il bel design per il sito WKND.

Scarica i kit di interfaccia utente XD:

* [Kit interfaccia utente per componenti core AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit interfaccia utente WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Sito di riferimento {#reference-site}

Una versione completa del sito WKND è disponibile anche come riferimento: [https://wknd.site/](https://wknd.site/)

L’esercitazione illustra le principali competenze di sviluppo necessarie per uno sviluppatore di AEM, ma *not* crea l’intero sito end-to-end. Il sito di riferimento finale è un&#39;altra grande risorsa per esplorare e vedere di più AEM funzionalità predefinite.

Per testare il codice più recente prima di saltare nell&#39;esercitazione, scarica e installa il **[ultima versione di GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Alimentato da Adobe Stock

Molte delle immagini del sito web WKND Reference provengono da [Adobe Stock](https://stock.adobe.com/) e sono materiale di terze parti come definito nei Termini aggiuntivi per le risorse demo all&#39;indirizzo [https://www.adobe.com/legal/terms.html](https://www.adobe.com/it/legal/terms.html). Se desideri utilizzare un’immagine Adobe Stock per altri scopi oltre a visualizzare questo sito web dimostrativo, ad esempio la sua funzione su un sito web o nel materiale di marketing, puoi acquistare una licenza su Adobe Stock.

Con Adobe Stock, puoi accedere a oltre 140 milioni di immagini di alta qualità e prive di royalty, incluse foto, grafica, video e modelli per dare un nuovo impulso ai tuoi progetti creativi.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Scopri come [generare un nuovo progetto Adobe Experience Manager utilizzando AEM Project Archetype](./project-archetype/overview.md) o [creare un sito utilizzando un modello di sito](./site-template/create-site.md).
