---
title: Invia messaggio tramite SendGrid
description: Attiva un messaggio di posta elettronica con un collegamento al modulo salvato
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 1%

---

# Integrare con SendGrid

L’integrazione dei dati di AEM Forms consente di configurare e collegare diverse origini dati con AEM Forms. Fornisce un’interfaccia utente intuitiva per creare uno schema unificato di rappresentazione dei dati di entità business e servizi tra origini dati connesse.

Abbiamo utilizzato l’API SendGrid per inviare e-mail utilizzando un modello dinamico. Si presume che tu abbia familiarità con l’API SendGrid per l’invio di e-mail utilizzando modelli dinamici. Nell’ambito di questa esercitazione, ti è stato fornito un file swagger per descrivere l’API.

## Creare l’integrazione

Per creare l’integrazione tra AEM Forms e SendGrid, effettua le seguenti operazioni

* Creare un’origine dati RESTful utilizzando [file swagger](./assets/SendGridWithDynamicTemplate.yaml). [Segui questo video per istruzioni dettagliate](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) sulla creazione di origini dati in AEM Forms
* Crea modello dati modulo in base all’origine dati creata nel passaggio precedente.[Segui la documentazione dettagliata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) durante la creazione di un modello dati modulo.

Il modello dati del modulo creato per questa esercitazione è incluso nell’articolo risorse.

### Passaggi successivi

[Crea integrazione archiviazione Azure](./create-fdm.md)

