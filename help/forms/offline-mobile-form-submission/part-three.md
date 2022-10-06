---
title: Flusso di lavoro di attivazione AEM per l’invio di moduli HTML5 - Revisione e approvazione di PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare AEM flusso di lavoro
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

# Flusso di lavoro per rivedere e approvare PDF inviato

L’ultimo e ultimo passaggio consiste nel creare AEM flusso di lavoro che genererà un PDF statico o non interattivo per la revisione e l’approvazione. Il flusso di lavoro viene attivato tramite un AEM Launcher configurato sul nodo `/content/pdfsubmissions`.

La schermata seguente mostra i passaggi coinvolti nel flusso di lavoro.

![workflow](assets/workflow.PNG)

## Passaggio del flusso di lavoro Genera PDF non interattivo

Il modello XDP e i dati da unire con il modello sono specificati qui. I dati da unire sono i dati inviati da PDF. I dati inviati vengono memorizzati sotto il nodo `/content/pdfsubmissions`.

![workflow](assets/generate-pdf1.PNG)

Il PDF generato viene assegnato alla variabile del flusso di lavoro denominata `submittedPDF`.

![workflow](assets/generate-pdf2.PNG)

### Assegnare il pdf generato per la revisione e l&#39;approvazione

Il componente del flusso di lavoro di assegnazione attività viene utilizzato qui per assegnare il PDF generato per la revisione e l’approvazione. Variabile `submittedPDF` viene utilizzato nella scheda Forms e documenti del componente del flusso di lavoro Assegna attività .

![workflow](assets/assign-task.PNG)
