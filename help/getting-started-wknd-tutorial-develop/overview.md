---
title: 'Guida introduttiva ad AEM Sites: tutorial WKND'
description: Scopri come implementare un sito AEM per un brand fittizio del settore lifestyle denominato WKND. Ottieni una descrizione dettagliata di Experienci Manager fondamentali come la configurazione del progetto, gli archetipi Maven, i componenti core, i modelli modificabili, le librerie client e lo sviluppo di componenti.
topics: development
version: Cloud Service
activity: develop
audience: developer
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 5f43db64f68218ed3d349508a2c4fd232601a9ef
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---

# Guida introduttiva ad AEM Sites: tutorial WKND {#introduction}

Benvenuti in un tutorial in più parti progettato per sviluppatori e sviluppatrici che non hanno mai utilizzato Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio, WKND. Il tutorial tratta elementi fondamentali come la configurazione del progetto, i componenti core, i modelli modificabili, le librerie lato client e lo sviluppo di componenti con Sites di Adobe Experience Manager.

## Panoramica {#wknd-tutorial-overview}

L’obiettivo di questo tutorial in più parti è insegnare a uno sviluppatore come implementare un sito web utilizzando gli standard e le tecnologie più recenti in Adobe Experience Manager (AEM). Dopo aver completato questa esercitazione, uno sviluppatore deve comprendere le basi di base della piattaforma e i modelli di progettazione comuni in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Opzioni per avviare un progetto Sites

Esistono due approcci di base per avviare un progetto AEM Sites.

**Archetipo progetto AEM** - Approccio tradizionale allo sviluppo dell’AEM grazie alla generazione di un progetto AEM minimo utilizzando un modello Maven. Questo è l’approccio consigliato per i progetti AEM 6.5/6.4 e per i progetti AEM as a Cloud Service che prevedono una pesante personalizzazione. Questo tutorial offre informazioni più approfondite sullo sviluppo dell’AEM.

[Inizia l’esercitazione con l’archetipo del progetto AEM](./project-archetype/overview.md)

**Modelli per sito AEM** - anche noto come Creazione Rapida dei Siti, un approccio low-code per la generazione di un Sito AEM utilizzando un Modello di Sito Predefinito. Utilizza componenti e modelli predefiniti per rendere rapidamente operativo un sito. Utilizza un flusso di lavoro per temi per applicare stili e personalizzazioni specifici per il brand solo con CSS e JavaScript. Consigliato per nuovi progetti e sviluppatori. Disponibile solo per AEM as a Cloud Service.

[Avvia l’esercitazione utilizzando un modello di sito](./site-template/create-site.md)

## Kit interfaccia utente di Adobe XD

Per avvicinare questo tutorial a uno scenario reale, i talentuosi designer UX dell’Adobe hanno creato i modelli per il sito utilizzando [Adobe XD](https://www.adobe.com/products/xd.html). Nel corso del tutorial vari pezzi dei progetti sono implementati in un sito AEM completamente modificabile. Un ringraziamento speciale a **Lorenzo Buosi** e **Kilian Amendola** che ha creato il bellissimo design del sito WKND.

Scarica i kit dell’interfaccia utente dell’XD:

* [Kit interfaccia utente dei componenti core AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit interfaccia WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Sito di riferimento {#reference-site}

È inoltre disponibile una versione finale del sito WKND come riferimento: [https://wknd.site/](https://wknd.site/)

Il tutorial illustra le principali competenze di sviluppo necessarie per uno sviluppatore AEM, ma *non* creare l&#39;intero sito end-to-end. Il sito di riferimento completo è un’altra ottima risorsa per esplorare e visualizzare più funzionalità AEM pronte all’uso.

Per testare il codice più recente prima di passare all&#39;esercitazione, scarica e installa **[ultima versione da GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Con tecnologia Adobe Stock

Molte delle immagini nel sito web di riferimento WKND provengono da [Adobe Stock](https://stock.adobe.com/) e sono Materiale di terze parti come definito nei Termini aggiuntivi della risorsa demo in [https://www.adobe.com/legal/terms.html](https://www.adobe.com/it/legal/terms.html). Se desideri utilizzare un’immagine Adobe Stock per altri scopi oltre alla visualizzazione del sito web demo, ad esempio visualizzarla su un sito web o nel materiale di marketing, puoi acquistare una licenza su Adobe Stock.

Con Adobe Stock, puoi accedere a più di 140 milioni di immagini di alta qualità e gratuite, tra cui foto, grafica, video e modelli, per avviare subito i tuoi progetti creativi.

## Passaggi successivi {#next-steps}

Cosa state aspettando?! Scopri come [generare un nuovo progetto Adobe Experience Manager utilizzando l’archetipo progetto AEM](./project-archetype/overview.md) o [creare un sito utilizzando un modello di sito](./site-template/create-site.md).
