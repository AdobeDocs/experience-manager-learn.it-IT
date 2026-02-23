---
title: Crea modello dati modulo per documento IC
description: Scopri come creare un modello dati modulo in AEM Forms per recuperare in modo dinamico i dati per il documento di comunicazione interattiva.
version: Experience Manager as a Cloud Service
feature: Interactive Communication
role: Developer
level: Intermediate
doc-type: Feature Video
duration: 170
last-substantial-update: 2026-02-20T00:00:00Z
jira: KT-20353
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---


# Crea modello dati modulo per documento IC

Crea un modello dati Forms per integrare origini dati esterne con la comunicazione interattiva in Adobe AEM. Questo processo comporta la configurazione di un servizio RESTful, il caricamento di un file Swagger e la configurazione di endpoint di servizio per recuperare e associare dinamicamente i dati. Scopri come connettersi in modo sicuro a servizi esterni e testare il modello per garantire il corretto recupero dei dati.

È stato implementato un server API fittizio che simula il servizio Orders a scopo di sviluppo e test. Espone un endpoint per recuperare gli ordini per un determinato utente (ad esempio, per ID utente), restituendo dati di ordine predefiniti o generati in modo dinamico nello stesso schema dell’API di produzione.

Il file swagger utilizzato per creare il modello dati del modulo può essere [scaricato da qui](assets/UsersAndOrders.json)

>[!VIDEO](https://video.tv.adobe.com/v/3480005/?learn=on&enablevpops)

## Passaggi successivi

[Creare un modello](./create-template.md)
