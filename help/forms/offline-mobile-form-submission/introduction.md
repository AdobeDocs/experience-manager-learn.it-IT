---
title: Attivare il flusso di lavoro AEM all’introduzione dell’invio del modulo HTM5
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare il flusso di lavoro AEM
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
duration: 342
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Download di un modulo mobile parzialmente completato e invio al flusso di lavoro AEM

Un caso d’uso comune prevede la possibilità di eseguire il rendering di XDP come HTML per le attività di acquisizione dati. Questo funziona bene quando i moduli sono semplici e possono essere compilati e inviati online. Tuttavia, se il modulo è complesso, gli utenti potrebbero non essere in grado di compilarlo online. È necessario consentire ai moduli di scaricare una versione interattiva del modulo da compilare in modalità offline utilizzando Acrobat/Reader. Una volta compilato il modulo, l’utente può essere online per inviarlo.
Per eseguire questo caso d’uso è necessario effettuare i seguenti passaggi:

* Possibilità di generare PDF interattivi/compilabili con i dati immessi nel modulo mobile
* Gestire l’invio PDF da Acrobat/Reader
* Attiva il flusso di lavoro Adobe Experience Manager (AEM) per rivedere il PDF inviato

Questo tutorial illustra i passaggi necessari per eseguire il caso d’uso precedente. Il codice di esempio e le risorse correlate a questa esercitazione sono [disponibile qui.](part-four.md)

Il video seguente offre una panoramica del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
