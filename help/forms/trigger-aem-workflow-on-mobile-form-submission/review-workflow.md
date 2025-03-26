---
title: Attivare il flusso di lavoro di AEM per l’invio di moduli HTML5 - Rivedere e approvare PDF
description: Flusso di lavoro per rivedere il PDF inviato
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ec60d017-8b29-4185-a097-d809e18df4a7
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 1%

---

# Flusso di lavoro per rivedere e approvare il PDF inviato

L’ultimo e ultimo passaggio consiste nel creare un flusso di lavoro AEM che genererà un PDF statico o non interattivo da rivedere e approvare. Il flusso di lavoro viene attivato tramite un modulo di avvio di AEM configurato sul nodo `/content/formsubmissions`.

La schermata seguente mostra i passaggi coinvolti nel flusso di lavoro.

![flusso di lavoro](assets/workflow.PNG)

## Passaggio del flusso di lavoro Genera PDF non interattivo

Il modello XDP e i dati da unire con il modello sono specificati qui. I dati da unire sono i dati inviati da PDF. I dati inviati sono archiviati nel nodo ```/content/formsubmissions```

![flusso di lavoro](assets/generate-pdf1.PNG)

Il PDF generato è assegnato alla variabile del flusso di lavoro denominata `submittedPDF`.

![flusso di lavoro](assets/generate-pdf2.PNG)

### Assegna il PDF generato per la revisione e l’approvazione

Il componente Assegna flusso di lavoro attività viene utilizzato qui per assegnare il PDF generato per la revisione e l’approvazione. La variabile `submittedPDF` è utilizzata nella scheda Forms e Documenti del componente del flusso di lavoro Assegna attività.

![flusso di lavoro](assets/assign-task.PNG)


## Passaggi successivi

[Distribuire le risorse nell’ambiente](./deploy-assets.md)
