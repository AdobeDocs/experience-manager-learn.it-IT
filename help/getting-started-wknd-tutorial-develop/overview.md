---
title: Guida introduttiva ad AEM Sites - Tutorial WKND
description: Scopri come implementare un sito AEM per un brand fittizio del settore lifestyle, con nome WKND. Ottieni una descrizione dettagliata di argomenti fondamentali su Experience Manager come la configurazione del progetto, gli archetipi Maven, i componenti core, i modelli modificabili, le librerie client e lo sviluppo di componenti.
version: Experience Manager as a Cloud Service
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
doc-type: Catalog
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: ht
source-wordcount: '577'
ht-degree: 100%

---

# Guida introduttiva ad AEM Sites - Tutorial WKND {#introduction}

{{traditional-aem}}

Ti diamo il benvenuto in un tutorial in più parti, progettato per sviluppatori che utilizzano per la prima volta Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio: WKND. Il tutorial tratta elementi fondamentali come la configurazione del progetto, i componenti core, i modelli modificabili, le librerie lato client e lo sviluppo di componenti con Adobe Experience Manager Sites.

## Panoramica {#wknd-tutorial-overview}

L’obiettivo di questo tutorial in più parti è quello di insegnare a uno sviluppatore come implementare un sito web utilizzando gli standard e le tecnologie più recenti in Adobe Experience Manager (AEM). Dopo aver completato questo tutorial, uno sviluppatore dovrebbe comprendere le nozioni di base della piattaforma e i modelli di progettazione comuni di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/36052?quality=12&learn=on&captions=ita)

## Opzioni per avviare un progetto Sites

Esistono due approcci di base per avviare un progetto AEM Sites.

**Archetipo di progetto AEM**: approccio tradizionale allo sviluppo di AEM che genera un progetto AEM minimo utilizzando un modello Maven. Questo approccio è consigliato per i progetti AEM 6.5/6.4 e per i progetti AEM as a Cloud Service che prevedono personalizzazioni importanti. Questo tutorial offre informazioni più approfondite sullo sviluppo di AEM.

[Avviare il tutorial con l’Archetipo di progetto AEM](./project-archetype/overview.md)

**Modelli di sito AEM**: anche noto come Creazione rapida di siti, un approccio a basso codice per generare un sito AEM utilizzando un modello di sito predefinito. Utilizza componenti e modelli predefiniti per rendere rapidamente operativo un sito. Utilizza un flusso di lavoro per tema per applicare stili e personalizzazioni specifici per il brand solo con CSS e JavaScript. Consigliato per nuovi progetti e sviluppatori. Disponibile solo per AEM as a Cloud Service.

[Avviare il tutorial utilizzando un modello di sito](./site-template/create-site.md)

## Kit interfaccia utente di Adobe XD

Per rendere questo tutorial uno scenario più realistico, i designer UX di Adobe hanno creato i modelli per il sito utilizzando [Adobe XD](https://www.adobe.com/it/products/xd.html). Nel corso del tutorial, vari elementi dei progetti vengono implementati in un sito AEM completamente personalizzabile dall’autore. Un ringraziamento speciale a **Lorenzo Buosi** e **Kilian Amendola** che hanno realizzato la progettazione del sito WKND.

Scarica i kit dell’interfaccia utente di XD:

* [Kit interfaccia utente dei componenti core di AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit interfaccia utente WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Sito di riferimento {#reference-site}

È disponibile anche una versione finita del sito WKND come riferimento: [https://wknd.site/](https://wknd.site/)

Il tutorial illustra le principali competenze di sviluppo necessarie per uno sviluppatore AEM, ma *non* creerà l’intero sito end-to-end. Il sito di riferimento finito è un’altra ottima risorsa per esplorare e visualizzare ulteriori funzionalità di AEM predefinite.

Per testare il codice più recente prima di passare al tutorial, scarica e installa la **[versione più recente da GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Basato su Adobe Analytics

Molte delle immagini nel sito web di riferimento WKND provengono da [Adobe Stock](https://stock.adobe.com/it) e sono materiale di terze parti come definito nelle condizioni aggiuntive della risorsa demo all’indirizzo [https://www.adobe.com/it/legal/terms.html](https://www.adobe.com/it/legal/terms.html). Se desideri utilizzare un’immagine di Adobe Stock per altri scopi oltre alla visualizzazione del sito web demo, ad esempio presentarla su un sito web o nel materiale di marketing, puoi acquistare una licenza su Adobe Stock.

Con Adobe Stock, puoi accedere a più di 140 milioni di immagini di alta qualità e esenti da royalty, tra cui foto, grafica, video e modelli, per avviare subito i tuoi progetti creativi.

## Passaggi successivi {#next-steps}

Scopri come [generare un nuovo progetto Adobe Experience Manager utilizzando l’Archetipo di progetto AEM](./project-archetype/overview.md) o [creare un sito utilizzando un Modello di sito](./site-template/create-site.md).
