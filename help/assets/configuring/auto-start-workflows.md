---
title: Flussi di lavoro con avvio automatico
description: I flussi di lavoro con avvio automatico estendono l’elaborazione delle risorse richiamando automaticamente il flusso di lavoro personalizzato al momento del caricamento o della rielaborazione.
feature: Asset Compute Microservices, Workflow
version: Experience Manager as a Cloud Service
jira: KT-4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
doc-type: Feature Video
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
duration: 385
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Flussi di lavoro con avvio automatico

I flussi di lavoro con avvio automatico estendono l’elaborazione delle risorse in AEM as a Cloud Service richiamando automaticamente il flusso di lavoro personalizzato al momento del caricamento o della rielaborazione al termine dell’elaborazione delle risorse.

>[!VIDEO](https://video.tv.adobe.com/v/326767?quality=12&learn=on&captions=ita)

>[!NOTE]
>
>Utilizza l’avvio automatico dei flussi di lavoro per personalizzare le risorse nella post-elaborazione anziché utilizzare i moduli di avvio dei flussi di lavoro. I flussi di lavoro con avvio automatico sono _solo_ richiamati al termine dell&#39;elaborazione di una risorsa, anziché moduli di avvio che possono essere richiamati più volte durante l&#39;elaborazione della risorsa.

## Personalizzazione del flusso di lavoro di post-elaborazione

Per personalizzare il flusso di lavoro di post-elaborazione, copia il modello di flusso di lavoro predefinito [Post-elaborazione](../../foundation/workflow/use-the-workflow-editor.md) di Assets Cloud.

1. Inizia dalla schermata Modelli flusso di lavoro passando a _Strumenti_ > _Flusso di lavoro_ > _Modelli_
2. Trova e seleziona il _modello di flusso di lavoro di post-elaborazione di Assets Cloud_<br/>
   ![Seleziona il modello di flusso di lavoro di post-elaborazione di Assets Cloud](assets/auto-start-workflow-select-workflow.png)
3. Seleziona il pulsante _Copia_ per creare il flusso di lavoro personalizzato
4. Seleziona il modello di flusso di lavoro Now (che sarà denominato _Post-elaborazione Assets Cloud1_) e fai clic sul pulsante _Modifica_ per modificare il flusso di lavoro
5. In Proprietà flusso di lavoro, assegna un nome significativo al flusso di lavoro di post-elaborazione personalizzato<br/>
   ![Modifica del nome](assets/auto-start-workflow-change-name.png)
6. Aggiungi i passaggi per soddisfare i requisiti aziendali, in questo caso aggiungendo un’attività al termine dell’elaborazione delle risorse. Assicurati che l&#39;ultimo passaggio del flusso di lavoro sia sempre il _passaggio del flusso di lavoro completato_<br/>
   ![Aggiungi passaggi flusso di lavoro](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >I flussi di lavoro con avvio automatico vengono eseguiti con ogni caricamento o rielaborazione di risorse; pertanto, è necessario considerare attentamente le implicazioni in termini di scalabilità dei passaggi del flusso di lavoro, in particolare per le operazioni in blocco, come [Importazioni in blocco](../../cloud-service/migration/bulk-import.md) o migrazioni.

7. Seleziona il pulsante _Sincronizza_ per salvare le modifiche e sincronizzare il modello di flusso di lavoro

## Utilizzo di un flusso di lavoro di post-elaborazione personalizzato

La post-elaborazione personalizzata è configurata nelle cartelle. Per configurare un flusso di lavoro di post-elaborazione personalizzato su una cartella:

1. Seleziona la cartella per la quale vuoi configurare il flusso di lavoro e modificare le proprietà della cartella
2. Passa alla scheda _Elaborazione risorse_
3. Seleziona il flusso di lavoro di post-elaborazione personalizzato nella casella di selezione _Avvia flusso di lavoro automatico_<br/>
   ![Imposta il flusso di lavoro di post-elaborazione](assets/auto-start-workflow-set-workflow.png)
4. Salva le modifiche

Ora il flusso di lavoro di post-elaborazione personalizzato verrà eseguito per tutte le risorse caricate o rielaborate in tale cartella.
