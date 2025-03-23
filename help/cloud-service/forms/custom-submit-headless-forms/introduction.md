---
title: Inviare un modulo headless a un servizio di invio personalizzato
description: Personalizzare la risposta in base ai dati inviati
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 78fe677c-d5ab-40f6-a381-800f24e227ae
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 2%

---

# Personalizzare la risposta in base ai dati inviati

Dopo l’invio del modulo , è importante fornire all’utente un feedback sull’esito dell’invio. La risposta di invio potrebbe includere un ID transazione o semplicemente una risposta personalizzata. Per soddisfare questo caso d’uso, in AEM Forms viene scritto un servizio di invio personalizzato e il modulo headless viene inviato a questo servizio di invio personalizzato.

## Prerequisiti

Per implementare correttamente questa funzionalità, si consiglia di acquisire familiarità con quanto segue

* Esperienza con Git
* Esperienza con AEM Cloud Manager
* Maven(questo articolo è stato testato con la versione 3.8.6)
* Istanza Autore locale pronta per AEM Forms Cloud
* Accesso all’ambiente AEM Forms as Cloud Service
* IntelliJ o qualsiasi altro IDE


## Passaggi successivi

[Scrivi il servizio di invio personalizzato](./custom-submit-service.md)
