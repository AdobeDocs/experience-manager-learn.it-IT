---
title: Attivare il flusso di lavoro AEM per l’invio di moduli HTM5 - Rivedere e approvare PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare il flusso di lavoro AEM
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# Flusso di lavoro per rivedere e approvare il PDF inviato

L’ultimo e ultimo passaggio consiste nel creare un flusso di lavoro AEM che genererà un PDF statico, o non interattivo, per la revisione e l’approvazione. Il flusso di lavoro viene attivato tramite un modulo di avvio AEM configurato sul nodo `/content/pdfsubmissions`.

La schermata seguente mostra i passaggi coinvolti nel flusso di lavoro.

![flusso di lavoro](assets/workflow.PNG)

## Genera passaggio del flusso di lavoro di PDF non interattivo

Il modello XDP e i dati da unire con il modello sono specificati qui. I dati da unire sono i dati inviati dal PDF. I dati inviati vengono memorizzati nel nodo `/content/pdfsubmissions`.

![flusso di lavoro](assets/generate-pdf1.PNG)

Il PDF generato viene assegnato alla variabile del flusso di lavoro denominata `submittedPDF`.

![flusso di lavoro](assets/generate-pdf2.PNG)

### Assegna il PDF generato per la revisione e l’approvazione

Il componente Assegna flusso di lavoro delle attività viene utilizzato qui per assegnare il PDF generato per la revisione e l’approvazione. La variabile `submittedPDF` viene utilizzato nella scheda Forms and Documents del componente Assegna attività del flusso di lavoro.

![flusso di lavoro](assets/assign-task.PNG)
