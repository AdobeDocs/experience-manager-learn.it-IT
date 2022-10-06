---
title: Modelli di flusso di lavoro AEM Forms riutilizzabili
description: Scopri come creare modelli di flusso di lavoro indipendenti da Adaptive Forms.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# Creare modelli di flussi di lavoro AEM Forms riutilizzabili{#create-re-usable-aem-forms-workflow-models}

A partire dalla versione 6.5 di AEM Forms, ora è possibile creare modelli di flusso di lavoro non legati a un modulo adattivo specifico. Grazie a questa funzionalità, ora puoi creare un modello di flusso di lavoro che può essere richiamato su diversi invii di moduli adattivi. Grazie a questa funzionalità, è possibile disporre di un flusso di lavoro generico per gestire tutti gli invii di moduli adattivi per la revisione e l’approvazione.

Per progettare un flusso di lavoro di questo tipo, effettua le seguenti operazioni

1. Accedi a AEM
1. Posiziona il browser su [modello di flusso di lavoro](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Fai clic su __Crea > Crea modello__ per aggiungere un modello di flusso di lavoro
1. Fornisci il Nome e il Titolo appropriati al modello di flusso di lavoro, quindi fai clic su Fine
1. Apri il modello appena creato in modalità di modifica
1. Trascina il componente Assegna attività sul modello di flusso di lavoro
1. Apri le proprietà di configurazione del componente Assegna attività
1. Passa alla scheda Forms e Documenti
1. Selezionare il tipo Modulo adattivo o Modulo adattivo di sola lettura.

Il percorso del modulo può essere specificato in tre modi

1. Disponibile in un percorso assoluto : il flusso di lavoro è strettamente associato al modulo adattivo. Questo non è quello che vogliamo qui
1. **Inviato al flusso di lavoro** - Questo significa che quando il modulo adattivo viene inviato, il motore del flusso di lavoro estrae il nome del modulo dai dati inviati. Questa è l’opzione che deve essere selezionata
1. Disponibile in un percorso in una variabile - Questo significa che il modulo adattivo viene prelevato dalla variabile del flusso di lavoro La schermata seguente mostra l&#39;opzione corretta che è necessario scegliere per il flusso di lavoro di de-accoppiamento da modulo adattivo

![Modelli di flusso di lavoro AEM Forms riutilizzabili](assets/workflomodel.PNG)
