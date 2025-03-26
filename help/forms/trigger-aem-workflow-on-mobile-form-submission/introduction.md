---
title: Attivare il flusso di lavoro di AEM all’introduzione dell’invio del modulo HTML5
description: Attivare un flusso di lavoro AEM all’invio di moduli per dispositivi mobili
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ce51b25f-6069-4799-9a61-98c0a672e821
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# Attivare il flusso di lavoro di AEM per l’invio di un modulo mobile

Un caso d’uso comune è quello di eseguire il rendering di XDP come HTML per le attività di acquisizione dati. All’invio di questo modulo, potrebbe essere necessario attivare un flusso di lavoro AEM. Nel flusso di lavoro di AEM, puoi unire i dati con il modello XDP e presentare il PDF generato per la revisione e l’approvazione. Il modulo viene eseguito su un’istanza pubblicata e il flusso di lavoro viene attivato su un’istanza di elaborazione AEM.

Nel caso di utilizzo sono coinvolti i seguenti passaggi

* L’utente compila e invia un modulo HTML5 (rendering HTML5 di XDP).
* L’invio viene gestito da un servlet nell’istanza Publish.
* Il servlet archivia i dati in una cartella predefinita nell’archivio dell’istanza di elaborazione di AEM.
* Il modulo di avvio dei flussi di lavoro è configurato per attivare un flusso di lavoro AEM ogni volta che viene aggiunto un nuovo file in una particolare cartella.

Questo tutorial illustra i passaggi necessari per eseguire il caso d’uso precedente. Il codice di esempio e le risorse correlate a questa esercitazione sono [disponibili qui.](./deploy-assets.md)


## Passaggi successivi

[Gestire l’invio di moduli](./handle-form-submission.md)
