---
title: Creare un componente per elencare i dati del modulo
description: Esercitazione per la creazione di un componente di riepilogo per la revisione dei dati del modulo prima dell’invio.
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# Creare un componente per riepilogare i dati del modulo

È stato creato un semplice componente per elencare i dati del modulo da rivedere. La [funzione di visita dell’API guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) è stato utilizzato per eseguire iterazioni nei campi del modulo. Il codice nella libreria client associata a questo componente ottiene i componenti pannello/tabella nel modulo. Dagli elementi secondari di questo pannello/componenti tabella il titolo dei campi modulo, il valore e l’espressione SOM vengono estratti utilizzando i metodi API di GuidBridge. Viene quindi creata una semplice tabella HTML con il titolo, il valore e l’espressione SOM affinché l’utente finale possa rivedere/modificare i dati del modulo prima di inviarlo.

Ad esempio, la schermata sottostante mostra la tabella creata per elencare i campi e i relativi valori **YourDetails**. L’ultimo TD nel TR viene utilizzato per modificare il valore del campo utilizzando l’espressione SOM dei campi.

![visit-func](assets/visit-function.png)

## Passaggi successivi

[Verificare la soluzione sul sistema locale](./deploy-on-your-system.md)
