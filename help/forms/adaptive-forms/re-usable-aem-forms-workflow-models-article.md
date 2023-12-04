---
title: Modelli di flusso di lavoro AEM Forms riutilizzabili
description: Scopri come creare modelli di flusso di lavoro indipendenti da Adaptive Forms.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 83
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Creare modelli di flussi di lavoro AEM Forms riutilizzabili{#create-re-usable-aem-forms-workflow-models}

A partire dalla versione 6.5 di AEM Forms, ora è possibile creare modelli di flusso di lavoro non legati a un modulo adattivo specifico. Grazie a questa funzionalità, ora è possibile creare un modello di flusso di lavoro che può essere richiamato in seguito all’invio di diversi moduli adattivi. Con questa funzionalità, puoi disporre di un flusso di lavoro generico per gestire tutti i moduli adattivi inviati per la revisione e l’approvazione.

Per progettare tale flusso di lavoro, esegui i seguenti passaggi

1. Accedere a AEM
1. Puntare il browser a [modello di flusso di lavoro](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Clic __Crea > Crea modello__ per aggiungere un modello di workflow
1. Specifica il nome e il titolo appropriati per il modello di flusso di lavoro, quindi fai clic su Fine
1. Apri il modello appena creato in modalità di modifica
1. Trascina il componente Assegna attività sul modello di flusso di lavoro
1. Aprire le proprietà di configurazione del componente Assegna attività
1. Scheda della scheda Forms e documenti
1. Seleziona Tipo: modulo adattivo o modulo adattivo di sola lettura.

È possibile specificare il percorso del modulo in tre modi

1. Disponibile in un percorso assoluto: ciò significa che il flusso di lavoro è strettamente associato al modulo adattivo. Questo non è quello che vogliamo qui
1. **Inviato al flusso di lavoro** - Questo significa che quando il modulo adattivo viene inviato, il motore del flusso di lavoro estrae il nome del modulo dai dati inviati. Questa è l’opzione che deve essere selezionata
1. Disponibile in un percorso in una variabile: ciò significa che il modulo adattivo viene raccolto dalla variabile del flusso di lavoro. La schermata seguente mostra l’opzione corretta da scegliere per separare il flusso di lavoro dal modulo adattivo

![Modelli di flusso di lavoro AEM Forms riutilizzabili](assets/workflomodel.PNG)
