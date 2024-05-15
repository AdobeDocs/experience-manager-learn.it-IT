---
title: Invia dati a elenco SharePoint tramite passaggio del flusso di lavoro
description: Inserire dati nell'elenco di SharePoint utilizzando il passaggio del flusso di lavoro FDM di richiamo
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Inserire dati nell’elenco SharePoint utilizzando il passaggio del flusso di lavoro FDM di richiamo


Questo articolo illustra i passaggi necessari per inserire i dati nell’elenco di SharePoint utilizzando il passaggio FDM di richiamo nel flusso di lavoro AEM.

Questo articolo presuppone che tu abbia [modulo adattivo configurato correttamente per inviare dati all’elenco SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)


## Creazione di un modello dati modulo basato sull&#39;origine dati dell&#39;elenco SharePoint

* Crea un nuovo modello dati modulo basato sull&#39;origine dati dell&#39;elenco di SharePoint.
* Aggiungi il modello appropriato e il servizio get del modello dati del modulo.
* Configura il servizio di inserimento per inserire l’oggetto modello di livello principale.
* Testare il servizio di inserimento.


## Crea un flusso di lavoro

* Crea un flusso di lavoro semplice con il passaggio FDM di richiamo.
* Configura la fase FDM di richiamo per utilizzare il modello dati del modulo creato nella fase precedente.
* ![fdm-associato](assets/fdm-insert-1.png)

* ![map-input-parameters](assets/fdm-insert-2.png)
* Osserva l’utilizzo della notazione in punti JSON. I dati inviati sono nel formato seguente e stiamo estraendo l’oggetto ContactUS dai dati inviati.

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```



## Configurare un modulo adattivo per attivare il flusso di lavoro AEM

* Crea un modulo adattivo utilizzando il modello dati modulo creato nel passaggio precedente.
* Trascinare alcuni campi dall&#39;origine dati al modulo.
* Configura l’azione di invio del modulo come illustrato di seguito
* ![submit-action](assets/configure-af.png)



## Testare il modulo

Visualizza l&#39;anteprima del modulo creato nel passaggio precedente. Compila il modulo e invia. I dati del modulo devono essere inseriti nell’elenco di SharePoint.
