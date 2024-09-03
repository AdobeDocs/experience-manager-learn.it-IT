---
title: Attivare il flusso di lavoro AEM per l’invio del modulo HTML5 - Revisione e approvazione di PDF
description: Flusso di lavoro per rivedere il PDF inviato
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 32
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# Flusso di lavoro per rivedere e approvare il PDF inviato

L’ultimo e ultimo passaggio consiste nel creare un flusso di lavoro AEM che genererà un PDF statico, o non interattivo, per la revisione e l’approvazione. Il flusso di lavoro viene attivato tramite un modulo di avvio AEM configurato sul nodo `/content/formsubmissions`.

La schermata seguente mostra i passaggi coinvolti nel flusso di lavoro.

![flusso di lavoro](assets/workflow.PNG)

## Genera passaggio del flusso di lavoro di PDF non interattivo

Il modello XDP e i dati da unire con il modello sono specificati qui. I dati da unire sono i dati inviati dal PDF. I dati inviati sono archiviati nel nodo ```/content/formsubmissions```

![flusso di lavoro](assets/generate-pdf1.PNG)

Il PDF generato è assegnato alla variabile del flusso di lavoro denominata `submittedPDF`.

![flusso di lavoro](assets/generate-pdf2.PNG)

### Assegna il PDF generato per la revisione e l’approvazione

Il componente Assegna flusso di lavoro delle attività viene utilizzato qui per assegnare il PDF generato per la revisione e l’approvazione. La variabile `submittedPDF` è utilizzata nella scheda Forms e Documenti del componente del flusso di lavoro Assegna attività.

![flusso di lavoro](assets/assign-task.PNG)


## Passaggi successivi

[Distribuire le risorse nell’ambiente](./deploy-assets.md)