---
title: Flusso di lavoro di attivazione AEM per l’invio di moduli HTML5
seo-title: Flusso di lavoro AEM trigger sull’invio di moduli HTML5
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare AEM flusso di lavoro
seo-description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare AEM flusso di lavoro
feature: Forms Mobile
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 1%

---


# Download del modulo mobile completato parzialmente e invio al flusso di lavoro AEM

Un caso d’uso comune è quello di avere la capacità di eseguire il rendering XDP come HTML per le attività di acquisizione dei dati. Questo funziona bene quando i moduli sono semplici e possono essere compilati e inviati online. Tuttavia, se il modulo è complesso e gli utenti potrebbero non essere in grado di completarlo online, è necessario fornire la possibilità ai compilatori di scaricare la versione interattiva del modulo da compilare utilizzando Acrobat/Reader in modalità offline. Una volta compilato il modulo, l’utente può essere online per inviarlo.
Per eseguire questo caso d’uso è necessario eseguire i seguenti passaggi:

* Possibilità di generare PDF interattivi/compilabili con i dati immessi nel modulo mobile
* Gestire l’invio dei PDF da Acrobat/Reader
* Attiva il flusso di lavoro Adobe Experience Manager (AEM) per rivedere il PDF inviato

Questa esercitazione descrive i passaggi necessari per eseguire il caso d’uso precedente. Il codice di esempio e le risorse relative a questa esercitazione sono [disponibili qui.](part-four.md)

Il video seguente offre una panoramica del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

