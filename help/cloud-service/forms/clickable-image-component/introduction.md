---
title: Creazione del componente immagine cliccabile
description: Creazione del componente immagine cliccabile nel Cloud Service AEM Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Introduzione

L’utilizzo di immagini cliccabili in Forms può creare un’esperienza utente più coinvolgente, intuitiva e visivamente attraente. Ai fini di questo articolo, abbiamo utilizzato SVG per le immagini cliccabili, in quanto offre diversi vantaggi in particolare in termini di flessibilità di progettazione, prestazioni ed esperienza utente.
SVG può essere creato utilizzando Adobe Illustrator o uno qualsiasi degli strumenti online gratuiti. Ho usato la [Mappa USA da](https://simplemaps.com/resources/svg-us)mappe semplici per dimostrare il caso d&#39;uso.

## Caso d’uso per l’utilizzo di una mappa Stati Uniti cliccabile

La mappa cliccabile degli Stati Uniti d&#39;America permette agli utenti di esplorare lo stato specifico moduli inviati. Quando un utente fa clic su uno stato, vengono elencati gli invii provenienti da tale stato, con l’opzione per aprire un invio specifico.
