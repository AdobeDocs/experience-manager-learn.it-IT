---
title: Creare un componente Immagine cliccabile
description: Crea componenti immagine cliccabili in AEM Forms as a Cloud Service.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Introduzione alle immagini cliccabili

L’utilizzo di immagini cliccabili in Forms può creare un’esperienza utente più coinvolgente, intuitiva e visivamente attraente. Ai fini di questo articolo, abbiamo utilizzato SVG per le immagini cliccabili, in quanto offre diversi vantaggi in particolare in termini di flessibilità di progettazione, prestazioni ed esperienza utente.
SVG può essere creato utilizzando Adobe Illustrator o uno qualsiasi degli strumenti online gratuiti. Ho usato la [Mappa USA da](https://simplemaps.com/resources/svg-us)mappe semplici per dimostrare il caso d&#39;uso.

## Caso d’uso per l’utilizzo di una mappa Stati Uniti cliccabile

La mappa cliccabile degli Stati Uniti d&#39;America permette agli utenti di esplorare lo stato specifico moduli inviati. Quando un utente fa clic su uno stato, vengono elencati gli invii provenienti da tale stato, con l’opzione per aprire un invio specifico.
