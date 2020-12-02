---
title: Crea Modelli Di Flusso Di Lavoro AEM Forms  Riutilizzabili.
seo-title: Crea Modelli Di Flusso Di Lavoro AEM Forms  Riutilizzabili.
description: modelli di workflow indipendenti da Forms adattivo.
seo-description: Modelli di flussi di lavoro indipendenti da Forms adattivo.
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---


# Crea modelli di flussi di lavoro AEM Forms  riutilizzabili{#create-re-usable-aem-forms-workflow-models}

A partire  versione AEM Forms 6.5, ora è possibile creare modelli di flussi di lavoro non associati a un modulo adattivo specifico. Grazie a questa funzionalità, è ora possibile creare un modello di flusso di lavoro che può essere richiamato per diversi invii di moduli adattivi. Grazie a questa funzionalità, è possibile disporre di un flusso di lavoro generico per gestire tutte le invii di moduli adattivi per la revisione e l&#39;approvazione.

Per progettare un flusso di lavoro di questo tipo, effettuare le seguenti operazioni

1. Accedi a AEM
1. Posizionare il browser sul [modello di flusso di lavoro](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Fate clic su Crea | Crea modello per aggiungere il modello di workflow
1. Specificare il nome e il titolo appropriati per il modello di workflow, quindi fare clic su Fine
1. Aprire il modello appena creato in modalità di modifica
1. Trascinare il componente Assegna attività sul modello di workflow
1. Aprire le proprietà di configurazione del componente Assegna attività
1. Passare alla scheda Forms e Documenti
1. Selezionare il tipo - Modulo adattivo o Modulo adattivo di sola lettura.

Sono disponibili 3 modi per specificare il percorso del modulo

1. Disponibile su un percorso assoluto: il flusso di lavoro verrà associato strettamente al modulo adattivo. Questo non è quello che vogliamo qui
1. **Inviato al flusso di lavoro**  - Questo significa che quando il modulo adattivo viene inviato, il motore del flusso di lavoro estrae il nome del modulo dai dati inviati. Questa è l&#39;opzione che deve essere selezionata
1. Disponibile in un percorso in una variabile. Questo significa che il modulo adattivo verrà prelevato dalla variabile del flusso di lavoro.
La schermata seguente mostra l’opzione corretta che è necessario scegliere per sganciare il flusso di lavoro dal modulo adattivo

![workflowmodel](assets/workflomodel.PNG)