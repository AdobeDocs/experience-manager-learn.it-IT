---
title: Crea Modelli Di Flusso Di Lavoro AEM Forms Riutilizzabili.
description: modelli di workflow indipendenti da Adaptive Forms.
feature: Flusso di lavoro
version: 6.5
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# Creare modelli di flussi di lavoro AEM Forms riutilizzabili{#create-re-usable-aem-forms-workflow-models}

A partire dalla versione 6.5 di AEM Forms, ora è possibile creare modelli di flusso di lavoro non legati a un modulo adattivo specifico. Grazie a questa funzionalità, ora puoi creare un modello di flusso di lavoro che può essere richiamato su diversi invii di moduli adattivi. Grazie a questa funzionalità, è possibile disporre di un flusso di lavoro generico per gestire tutti gli invii di moduli adattivi per la revisione e l’approvazione.

Per progettare un flusso di lavoro di questo tipo, effettua le seguenti operazioni

1. Accedi a AEM
1. Posiziona il browser sul [modello di flusso di lavoro](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Fai clic su Crea | Crea modello per aggiungere un modello di flusso di lavoro
1. Fornisci il Nome e il Titolo appropriati al modello di flusso di lavoro, quindi fai clic su Fine
1. Apri il modello appena creato in modalità di modifica
1. Trascina il componente Assegna attività sul modello di flusso di lavoro
1. Apri le proprietà di configurazione del componente Assegna attività
1. Passa alla scheda Forms e Documenti
1. Selezionare il tipo Modulo adattivo o Modulo adattivo di sola lettura.

Sono disponibili 3 modi per specificare il percorso del modulo

1. Disponibile in un percorso assoluto : il flusso di lavoro sarà strettamente associato al modulo adattivo. Questo non è quello che vogliamo qui
1. **Inviato al flusso di lavoro**  - Questo significa che quando il modulo adattivo viene inviato, il motore del flusso di lavoro estrarrà il nome del modulo dai dati inviati. Questa è l’opzione che deve essere selezionata
1. Disponibile in un percorso in una variabile - Questo significa che il modulo adattivo verrà prelevato dalla variabile del flusso di lavoro
La schermata seguente mostra l’opzione corretta che è necessario scegliere per il flusso di lavoro di disaccoppiamento da modulo adattivo

![modello workflow](assets/workflomodel.PNG)