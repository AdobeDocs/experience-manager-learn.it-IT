---
title: Inviare un modulo headless a un servizio di invio personalizzato
description: Personalizzare la risposta in base ai dati inviati
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 1%

---


# Personalizzare la risposta in base ai dati inviati

Dopo l’invio del modulo , è importante fornire all’utente un feedback sull’esito dell’invio. La risposta di invio potrebbe includere un ID transazione o semplicemente una risposta personalizzata. Per soddisfare questo caso d’uso, in AEM Forms viene scritto un servizio di invio personalizzato e il modulo headless viene inviato a questo servizio di invio personalizzato.

## Prerequisiti

Per implementare correttamente questa funzionalità, si consiglia di acquisire familiarità con quanto segue

* Esperienza con Git
* Esperienza con AEM Cloud Manager
* Maven(questo articolo è stato testato con la versione 3.8.6)
* Istanza Autore locale pronta per AEM Forms Cloud
* Accesso ad AEM Forms come ambiente di Cloud Service
* IntelliJ o qualsiasi altro IDE


## Passaggi successivi

[Scrivi il servizio di invio personalizzato](./custom-submit-service.md)