---
title: Attivazione AEM flusso di lavoro per l’invio di moduli HTML5
seo-title: Attiva AEM flusso di lavoro per l’invio di moduli HTML5
description: Continuare a compilare il modulo per dispositivi mobili in modalità offline e inviare il modulo per dispositivi mobili per attivare AEM flusso di lavoro
seo-description: Continuare a compilare il modulo per dispositivi mobili in modalità offline e inviare il modulo per dispositivi mobili per attivare AEM flusso di lavoro
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c56942831614b981684861ea78f1bd15f3bb1ab9
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 2%

---


# Flusso di lavoro per esaminare e approvare il PDF inviato

L&#39;ultimo passaggio consiste nel creare AEM flusso di lavoro che genererà un PDF statico, o non interattivo, da sottoporre a revisione e approvazione. Il flusso di lavoro verrà attivato tramite un AEM Launcher configurato sul nodo `/content/pdfsubmissions`.

La schermata seguente mostra i passaggi coinvolti nel flusso di lavoro.

![workflow](assets/workflow.PNG)

## Passaggio del flusso di lavoro Genera PDF non interattivo

Il modello XDP e i dati da unire con il modello sono specificati qui. I dati da unire sono i dati inviati dal PDF. I dati inviati vengono memorizzati nel nodo `/content/pdfsubmissions`.

![workflow](assets/generate-pdf1.PNG)

Il PDF generato viene assegnato alla variabile del flusso di lavoro denominata `submittedPDF`.

![workflow](assets/generate-pdf2.PNG)

### Assegnare il pdf generato per la revisione e l&#39;approvazione

Il componente per l’assegnazione del flusso di lavoro attività viene utilizzato qui per assegnare il PDF generato per la revisione e l’approvazione. La variabile `submittedPDF` viene utilizzata nella scheda Forms e Documenti del componente Flusso di lavoro Assegna attività.

![workflow](assets/assign-task.PNG)
