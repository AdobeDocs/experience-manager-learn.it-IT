---
title: Tutorial Integrare Analytics con Commerce
description: Scopri come integrare Analytics con Commerce.
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="Integrazione" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---


# Integrare Analytics con Commerce

* Verifica che l’account abbia accesso ad Adobe Analytics.

* Crea un progetto in Adobe Analytics.

* Crea uno schema.
   * È necessario selezionarlo tra le opzioni nei passaggi successivi. Per creare uno schema, cerca nella colonna a sinistra in &quot;Gestione dati&quot; e trova Schemi. Ora in alto a sinistra, fai clic su &quot;Crea schema&quot;. Seleziona ExperienceEvent XDM.
   * Sulla sinistra trova gruppi di campi, fai clic su Aggiungi
      * Nella ricerca, puoi filtrare immettendo `ExperienceEvent Commerce`
      * Cerca `Adobe Analytics ExperienceEvent Commerce` e seleziona la casella
      * Assicurati di fare clic su `Add field groups` in alto a destra per salvare e continuare
* Crea un set di dati, necessario quando configuri il successivo &quot;DataStream&quot;.
   * Il set di dati si trova nella colonna sinistra &quot;Gestione dati&quot; e cerca &quot;Set di dati&quot;.
   * Fai clic su &quot;Crea set di dati&quot; in alto a destra. Crea il set di dati dallo schema.
   * cerca e utilizza lo schema creato in precedenza
* Creare uno stream di dati. Puoi accedervi utilizzando &quot;Raccolta dati nella colonna a sinistra&quot; e cercando &quot;Flussi di dati&quot;.
* Crea tabelle con pannelli e segmenti. Questo è un modo per essere complicato per questa esercitazione, hai bisogno dell&#39;assistenza di una persona Analytics esperta.


Infine, per visualizzare il rapporto, vai su experience.adobe.com trova il progetto Workspace, fai clic sul collegamento del progetto che desideri visualizzare e dovresti vedere qualcosa di simile a questa immagine

![Schermata di Analytics relativa ad alcuni dati di e-commerce](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)