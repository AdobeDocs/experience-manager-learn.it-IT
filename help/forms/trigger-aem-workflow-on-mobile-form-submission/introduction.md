---
title: Attivare il flusso di lavoro AEM all’introduzione dell’invio del modulo HTML5
description: Attivare un flusso di lavoro AEM all’invio di moduli mobili
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: c6ffa8f7a398b01fc12e1e2efe4382c941900496
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# Attivare il flusso di lavoro AEM in seguito all’invio di un modulo mobile

Un caso d’uso comune è quello di eseguire il rendering di XDP come HTML per le attività di acquisizione dati. All’invio di questo modulo potrebbe essere necessario attivare un flusso di lavoro AEM. Nel flusso di lavoro AEM, puoi unire i dati con il modello XDP e presentare le PDF generate per la revisione e l’approvazione. Il modulo viene eseguito su un’istanza pubblicata e il flusso di lavoro viene attivato su un’istanza di elaborazione AEM.

Nel caso di utilizzo sono coinvolti i seguenti passaggi

* L’utente compila e invia un modulo HTML5 (rendering HTML5 di XDP).
* L’invio viene gestito da un servlet nell’istanza Publish.
* Il servlet archivia i dati in una cartella predefinita nell’archivio dell’istanza di elaborazione AEM.
* Il modulo di avvio dei flussi di lavoro è configurato per attivare un flusso di lavoro AEM ogni volta che viene aggiunto un nuovo file in una particolare cartella.

Questo tutorial illustra i passaggi necessari per eseguire il caso d’uso precedente. Il codice di esempio e le risorse correlate a questa esercitazione sono [disponibili qui.](./deploy-assets.md)


## Passaggi successivi

[Gestire l’invio di moduli](./handle-form-submission.md)
