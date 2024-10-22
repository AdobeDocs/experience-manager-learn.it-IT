---
title: Utilizzo del sistema di stili in AEM Forms
description: Creare il progetto tema
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: a0de7eaa391749b6b0d90e7cf3e363c2d5a232b5
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# Verifica le modifiche

Crea un modulo adattivo basato sul modello **&quot;Blank with Core Components&quot;**. Trascinare e rilasciare 3 pulsanti sul modulo e assegnare loro l&#39;etichetta &quot;Corporate&quot;, &quot;Marketing&quot; e &quot;Default&quot;.
Assegnare le varianti di stile appropriate ai pulsanti Aziendale e Marketing selezionando il pennello di disegno come mostrato di seguito.

![stili](assets/marketing-variation.png)

Al terzo pulsante verrà applicato lo stile predefinito.

## Creare il progetto tema

Il passaggio successivo è quello di costruire il progetto tematico. Passa alla cartella principale del progetto tema ed esegui il comando _**npm run build**_ come mostrato nella schermata seguente

![build-theme](assets/build-theme.png)

Una volta creato correttamente il progetto tematico, puoi testare le modifiche.

## Metodo semplice e veloce per testare il css

* Apri il file theme.css che si trova nella cartella Dist del progetto tema.Seleziona e copia l’intero contenuto del file.
* Visualizzare in anteprima il modulo creato nel passaggio precedente.
* Fai clic con il pulsante destro del mouse su uno dei pulsanti e seleziona Inspect per aprire la console per sviluppatori.
* Dalla console per sviluppatori, fai clic su theme.css per aprire theme.css
* Seleziona ed elimina l’intero contenuto di theme.css utilizzando il CTR-A e premi il pulsante Elimina.
* Copia e incolla il contenuto di theme.css creato nel passaggio precedente.
* I pulsanti devono essere aggiornati con gli stili appropriati, come illustrato di seguito.

![pulsanti finali](assets/final-state-buttons.png)

