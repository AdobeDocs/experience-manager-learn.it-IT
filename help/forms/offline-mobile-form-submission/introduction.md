---
title: Attiva il flusso di lavoro AEM all’invio del modulo PDF
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare il flusso di lavoro AEM
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 342
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# Download di un modulo mobile parzialmente completato e invio per attivare un flusso di lavoro AEM

Un caso d’uso comune prevede la possibilità di eseguire il rendering di XDP come HTML per le attività di acquisizione dati. Questo funziona bene quando i moduli sono semplici e possono essere compilati e inviati online. Tuttavia, se il modulo è complesso, gli utenti potrebbero non essere in grado di compilarlo online. È necessario consentire ai moduli di scaricare una versione interattiva del modulo da compilare in modalità offline utilizzando Acrobat/Reader. Una volta compilato il modulo, l’utente può essere online per inviarlo.
Per eseguire questo caso d’uso è necessario effettuare i seguenti passaggi:

* Possibilità di generare PDF interattivi/compilabili con i dati immessi nel modulo mobile
* Gestire l’invio PDF da Acrobat/Reader
* Attiva il flusso di lavoro Adobe Experience Manager (AEM) per rivedere il PDF inviato

Questo tutorial illustra i passaggi necessari per eseguire il caso d’uso precedente. Il codice di esempio e le risorse correlate a questa esercitazione sono [disponibili qui.](./deploy-assets.md)

Il video seguente offre una panoramica del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)

## Passaggi successivi

[Crea profilo personalizzato](./custom-profile.md)