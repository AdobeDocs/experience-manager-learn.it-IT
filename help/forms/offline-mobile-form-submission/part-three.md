---
title: Attiva flusso di lavoro AEM per l’invio di moduli HTML5
seo-title: Attiva flusso di lavoro AEM sull’invio di moduli HTML5
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare il flusso di lavoro AEM
seo-description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare il flusso di lavoro AEM
feature: moduli mobili
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 2%

---


# Flusso di lavoro per rivedere e approvare il PDF inviato

L’ultimo e ultimo passaggio consiste nel creare un flusso di lavoro AEM che genererà un PDF statico o non interattivo da rivedere e approvare. Il flusso di lavoro verrà attivato tramite un modulo di avvio AEM configurato sul nodo `/content/pdfsubmissions`.

La schermata seguente mostra i passaggi coinvolti nel flusso di lavoro.

![workflow](assets/workflow.PNG)

## Passaggio del flusso di lavoro Genera PDF non interattivo

Il modello XDP e i dati da unire con il modello sono specificati qui. I dati da unire sono i dati inviati dal PDF. I dati inviati vengono memorizzati sotto il nodo `/content/pdfsubmissions`.

![workflow](assets/generate-pdf1.PNG)

Il PDF generato viene assegnato alla variabile del flusso di lavoro denominata `submittedPDF`.

![workflow](assets/generate-pdf2.PNG)

### Assegnare il pdf generato per la revisione e l&#39;approvazione

Il componente Flusso di lavoro di assegnazione viene utilizzato qui per assegnare il PDF generato per la revisione e l’approvazione. La variabile `submittedPDF` viene utilizzata nella scheda Moduli e documenti del componente del flusso di lavoro Assegna attività .

![workflow](assets/assign-task.PNG)
