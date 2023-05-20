---
title: Inviare allegati del modulo adattivo
description: Inviare allegati del modulo adattivo tramite il componente Invia e-mail
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 2%

---

# Introduzione



Un caso d’uso comune consiste nell’inviare gli allegati del modulo adattivo utilizzando il componente Invia e-mail in un flusso di lavoro AEM.
In genere, i clienti comprimono gli allegati del modulo o li inviano come file singoli utilizzando il componente Invia e-mail.

## Inviare gli allegati del modulo in un file zip

Per eseguire il caso d’uso è stato scritto un passaggio di processo del flusso di lavoro personalizzato. In questo passaggio del processo personalizzato viene creato un file zip con gli allegati del modulo creati e memorizzati nella cartella del payload in un file denominato *zipped_attachments.zip*

![invia-allegati-modulo](assets/send-form-attachments.JPG)

## Inviare singolarmente gli allegati del modulo

Per eseguire questo caso d’uso è stato scritto un passaggio di processo del flusso di lavoro personalizzato. In questo passaggio del processo personalizzato vengono compilate le variabili del flusso di lavoro di tipo ArrayList of Documents e ArrayList of Strings.

![invia-elenco-di-documenti](assets/send-list-of-documents.JPG)

## Passaggi successivi

[Allegati modulo ZIP](./custom-process-step.md)
