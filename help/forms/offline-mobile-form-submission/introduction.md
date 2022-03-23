---
title: Flusso di lavoro di attivazione AEM per l’introduzione dell’invio di moduli HTML5
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare AEM flusso di lavoro
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Download del modulo mobile completato parzialmente e invio al flusso di lavoro AEM

Un caso d’uso comune è quello di avere la capacità di rappresentare XDP come HTML per le attività di acquisizione dei dati. Questo funziona bene quando i moduli sono semplici e possono essere compilati e inviati online. Tuttavia, se il modulo è complesso e gli utenti potrebbero non essere in grado di completarlo online, è necessario fornire la possibilità ai compilatori di scaricare la versione interattiva del modulo da compilare utilizzando Acrobat/Reader in modalità offline. Una volta compilato il modulo, l’utente può essere online per inviarlo.
Per eseguire questo caso d’uso è necessario eseguire i seguenti passaggi:

* Possibilità di generare PDF interattivi/compilabili con i dati immessi nel modulo mobile
* Gestire l’invio di PDF da Acrobat/Reader
* Attiva il flusso di lavoro Adobe Experience Manager (AEM) per rivedere il PDF inviato

Questa esercitazione descrive i passaggi necessari per eseguire il caso d’uso precedente. Il codice di esempio e le risorse relative a questa esercitazione sono [disponibile qui.](part-four.md)

Il video seguente offre una panoramica del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)
