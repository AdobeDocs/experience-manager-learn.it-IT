---
title: Estrarre un documento da un elenco di documenti in un flusso di lavoro AEM
description: Componente flusso di lavoro personalizzato per estrarre un documento specifico da un elenco di documenti
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
exl-id: b0baac71-3074-49d5-9686-c9955b096abb
duration: 56
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Estrai documento da elenco di documenti

Un caso d’uso comune consiste nell’inviare i dati del modulo e il relativo allegato a un sistema esterno utilizzando il passaggio richiama modello dati modulo in un flusso di lavoro AEM. Ad esempio, quando si crea un caso in ServiceNow, è necessario inviare i dettagli del caso con un documento di supporto. Gli allegati aggiunti al modulo adattivo vengono memorizzati in una variabile di tipo array list of documents. Per estrarre un documento specifico da questo array list, è necessario scrivere un codice personalizzato.

Questo articolo illustra i passaggi necessari per utilizzare il componente del flusso di lavoro personalizzato per estrarre e memorizzare il documento in una variabile di documento.

## Crea flusso di lavoro

È necessario creare un flusso di lavoro per gestire l’invio del modulo. Il flusso di lavoro deve avere le seguenti variabili definite

* Variabile di tipo ArrayList of Document (questa variabile conterrà gli allegati del modulo aggiunti dall&#39;utente)
* Variabile di tipo Document.(Questa variabile contiene il documento estratto da ArrayList)

* Aggiungi il componente personalizzato al flusso di lavoro e configurane le proprietà
  ![estrarre-elemento-flusso di lavoro](assets/extract-document-array-list.png)

## Configurare un modulo adattivo

* Configurare l’azione di invio del modulo adattivo per attivare il flusso di lavoro AEM
  ![submit-action](assets/store-attachments.png)

## Testare la soluzione

[Distribuire il bundle personalizzato utilizzando la console web OSGi](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[Importare il componente del flusso di lavoro tramite Gestione pacchetti](assets/Extract-item-from-documents-list.zip)

[Importa il flusso di lavoro di esempio](assets/extract-item-sample-workflow.zip)

[Importare il modulo adattivo](assets/test-attachment-extractions-adaptive-form.zip)

[Anteprima modulo](http://localhost:4502/content/dam/formsanddocuments/testattachmentsextractions/jcr:content?wcmmode=disabled)

Aggiungere un allegato al modulo e inviarlo.

>[!NOTE]
>
>Il documento estratto può quindi essere utilizzato in qualsiasi altro passaggio del flusso di lavoro, ad esempio Invia e-mail o Richiama passaggio FDM
