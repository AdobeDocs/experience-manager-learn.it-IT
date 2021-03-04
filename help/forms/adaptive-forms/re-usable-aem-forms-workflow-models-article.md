---
title: Crea modelli di flusso di lavoro AEM Forms riutilizzabili.
seo-title: Crea modelli di flusso di lavoro AEM Forms riutilizzabili.
description: modelli di flusso di lavoro indipendenti da Moduli adattivi.
seo-description: Modelli di flusso di lavoro indipendenti da Moduli adattivi.
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# Crea modelli di flussi di lavoro AEM Forms riutilizzabili{#create-re-usable-aem-forms-workflow-models}

A partire dalla versione 6.5 di AEM Forms, ora è possibile creare modelli di flusso di lavoro non legati a un modulo adattivo specifico. Grazie a questa funzionalità, ora puoi creare un modello di flusso di lavoro che può essere richiamato su diversi invii di moduli adattivi. Grazie a questa funzionalità, è possibile disporre di un flusso di lavoro generico per gestire tutti gli invii di moduli adattivi per la revisione e l’approvazione.

Per progettare un flusso di lavoro di questo tipo, effettua le seguenti operazioni

1. Accedi ad AEM
1. Posiziona il browser sul [modello di flusso di lavoro](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Fai clic su Crea | Crea modello per aggiungere un modello di flusso di lavoro
1. Fornisci il Nome e il Titolo appropriati al modello di flusso di lavoro, quindi fai clic su Fine
1. Apri il modello appena creato in modalità di modifica
1. Trascina il componente Assegna attività sul modello di flusso di lavoro
1. Apri le proprietà di configurazione del componente Assegna attività
1. Passare alla scheda Moduli e documenti
1. Selezionare il tipo Modulo adattivo o Modulo adattivo di sola lettura.

Sono disponibili 3 modi per specificare il percorso del modulo

1. Disponibile in un percorso assoluto : il flusso di lavoro sarà strettamente associato al modulo adattivo. Questo non è quello che vogliamo qui
1. **Inviato al flusso di lavoro**  - Questo significa che quando il modulo adattivo viene inviato, il motore del flusso di lavoro estrarrà il nome del modulo dai dati inviati. Questa è l’opzione che deve essere selezionata
1. Disponibile in un percorso in una variabile - Questo significa che il modulo adattivo verrà prelevato dalla variabile del flusso di lavoro
La schermata seguente mostra l’opzione corretta che è necessario scegliere per il flusso di lavoro di disaccoppiamento da modulo adattivo

![modello workflow](assets/workflomodel.PNG)