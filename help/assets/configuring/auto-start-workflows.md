---
title: Flussi di lavoro con avvio automatico
description: I flussi di lavoro con avvio automatico estendono l’elaborazione delle risorse richiamando automaticamente il flusso di lavoro personalizzato al momento del caricamento o della rielaborazione.
feature: Asset Compute Microservices, Workflow
version: Cloud Service
jira: KT-4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
doc-type: Feature Video
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
duration: 385
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Flussi di lavoro con avvio automatico

I flussi di lavoro con avvio automatico estendono l’elaborazione delle risorse in AEM as a Cloud Service richiamando automaticamente il flusso di lavoro personalizzato al momento del caricamento o della rielaborazione al termine dell’elaborazione delle risorse.

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)

>[!NOTE]
>
>Utilizza l’avvio automatico dei flussi di lavoro per personalizzare le risorse nella post-elaborazione anziché utilizzare i moduli di avvio dei flussi di lavoro. I flussi di lavoro con avvio automatico sono _solo_ richiamato una volta completata l’elaborazione di una risorsa, anziché moduli di avvio che possono essere richiamati più volte durante l’elaborazione della risorsa.

## Personalizzazione del flusso di lavoro di post-elaborazione

Per personalizzare il flusso di lavoro di post-elaborazione, copia il post-elaborazione predefinito di Assets Cloud [modello di flusso di lavoro](../../foundation/workflow/use-the-workflow-editor.md).

1. Inizia dalla schermata Modelli di flusso di lavoro passando a _Strumenti_ > _Flusso di lavoro_ > _Modelli_
2. Trova e seleziona la _Post-elaborazione Assets cloud_ modello di flusso di lavoro<br/>
   ![Seleziona il modello di flusso di lavoro di post-elaborazione di Assets Cloud](assets/auto-start-workflow-select-workflow.png)
3. Seleziona la _Copia_ per creare un flusso di lavoro personalizzato
4. Seleziona il modello di flusso di lavoro Now (che verrà chiamato _Post-elaborazione Assets cloud1_) e fare clic su _Modifica_ per modificare il workflow
5. Dalla scheda Proprietà flusso di lavoro, assegna un nome significativo al flusso di lavoro di post-elaborazione personalizzato<br/>
   ![Modifica del nome](assets/auto-start-workflow-change-name.png)
6. Aggiungi i passaggi per soddisfare i requisiti aziendali, in questo caso aggiungendo un’attività al termine dell’elaborazione delle risorse. Assicurati che l’ultimo passaggio del flusso di lavoro sia sempre il _Flusso di lavoro completato_ passaggio<br/>
   ![Aggiungi passaggi del flusso di lavoro](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >I flussi di lavoro con avvio automatico vengono eseguiti con ogni caricamento o rielaborazione di risorse; pertanto, è necessario considerare attentamente le implicazioni in termini di scalabilità dei passaggi del flusso di lavoro, in particolare per le operazioni in blocco come [Importazioni in blocco](../../cloud-service/migration/bulk-import.md) o migrazioni.

7. Seleziona la _Sincronizza_ per salvare le modifiche e sincronizzare il modello di workflow

## Utilizzo di un flusso di lavoro di post-elaborazione personalizzato

La post-elaborazione personalizzata è configurata nelle cartelle. Per configurare un flusso di lavoro di post-elaborazione personalizzato su una cartella:

1. Seleziona la cartella per la quale vuoi configurare il flusso di lavoro e modificare le proprietà della cartella
2. Passa a _Elaborazione risorse_ scheda
3. Seleziona il flusso di lavoro di post-elaborazione personalizzato in _Avvia flusso di lavoro automaticamente_ casella di selezione<br/>
   ![Impostare il flusso di lavoro di post-elaborazione](assets/auto-start-workflow-set-workflow.png)
4. Salva le modifiche

Ora il flusso di lavoro di post-elaborazione personalizzato verrà eseguito per tutte le risorse caricate o rielaborate in tale cartella.
