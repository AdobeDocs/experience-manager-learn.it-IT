---
title: Inviare allegati di moduli adattivi
description: Inviare allegati ai moduli adattivi utilizzando il componente Invia e-mail
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Introduzione



Un caso d’uso comune è quello di inviare gli allegati al modulo adattivo utilizzando il componente Invia e-mail in un flusso di lavoro AEM.
In genere, i clienti comprimerebbero gli allegati del modulo o li invierebbero come singoli file utilizzando il componente Invia e-mail.

## Inviare gli allegati del modulo in un file zip

Per eseguire il caso d’uso è stato scritto un passaggio del processo di flusso di lavoro personalizzato. In questo passaggio del processo personalizzato, un file zip con gli allegati del modulo in creato e memorizzato sotto la cartella payload in un file denominato *zip_attachment.zip*

![allegati di moduli di invio](assets/send-form-attachments.JPG)

## Inviare singolarmente gli allegati del modulo

Per eseguire questo caso d’uso è stato scritto un passaggio di processo di flusso di lavoro personalizzato. In questo passaggio del processo personalizzato vengono compilate le variabili del flusso di lavoro di tipo ArrayList of Documents e ArrayList of Strings.

![invia elenco dei documenti](assets/send-list-of-documents.JPG)
