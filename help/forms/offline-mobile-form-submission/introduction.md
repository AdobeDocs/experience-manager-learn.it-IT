---
title: Attivazione AEM flusso di lavoro per l’invio di moduli HTML5
seo-title: Attiva AEM flusso di lavoro per l’invio di moduli HTML5
description: Continuare a compilare il modulo per dispositivi mobili in modalità offline e inviare il modulo per dispositivi mobili per attivare AEM flusso di lavoro
seo-description: Continuare a compilare il modulo per dispositivi mobili in modalità offline e inviare il modulo per dispositivi mobili per attivare AEM flusso di lavoro
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# Download del modulo mobile completato parzialmente e invio al flusso di lavoro AEM

Un caso d’uso comune è la capacità di rappresentare l’XDP come HTML per le attività di acquisizione dei dati. Questo funziona bene quando i moduli sono semplici e possono essere compilati e inviati online. Tuttavia, se il modulo è complesso e gli utenti potrebbero non essere in grado di completarlo online, è necessario consentire agli utenti che compilano il modulo di scaricare la versione interattiva del modulo da compilare utilizzando  Acrobat/Reader in modalità offline. Una volta compilato il modulo, l&#39;utente può essere online per inviare il modulo.
Per eseguire questo caso di utilizzo, è necessario eseguire i seguenti passaggi:

* Generazione di PDF interattivi/compilabili con i dati immessi nel modulo per dispositivi mobili
* Gestione dell’invio PDF da  Acrobat/Reader
* Avvia flusso di lavoro Adobe Experience Manager (AEM) per esaminare il PDF inviato

Questa esercitazione descrive i passaggi necessari per eseguire le operazioni descritte sopra. Il codice di esempio e le risorse correlate a questa esercitazione sono [disponibili qui.](part-four.md)

Il seguente video offre una panoramica del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

