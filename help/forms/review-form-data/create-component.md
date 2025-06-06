---
title: Creare un componente per elencare i dati del modulo
description: Tutorial per creare un componente di riepilogo per la revisione dei dati del modulo prima dell’invio.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 33
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# Crea un componente per riepilogare i dati del modulo

È stato creato un componente semplice per elencare i dati del modulo per la revisione. La funzione di visita dell&#39;API [guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) è stata utilizzata per eseguire iterazioni nei campi modulo. Il codice nella libreria client associato a questo componente genera i componenti pannello/tabella nel modulo. Dagli elementi secondari di questo pannello/componenti di tabella vengono estratti il titolo, il valore e l’espressione SOM dei campi modulo utilizzando i metodi API GuidBridge. Viene quindi creata una semplice tabella di HTML con il titolo, il valore e l’espressione SOM per consentire all’utente finale di rivedere/modificare i dati del modulo prima di inviarlo.

Ad esempio, la schermata seguente mostra la tabella creata per elencare i campi e i relativi valori di **YourDetails**. L&#39;ultima TD nel TR viene utilizzata per modificare il valore del campo utilizzando l&#39;espressione SOM dei campi.

![visit-func](assets/visit-function.png)

## Passaggi successivi

[Test della soluzione sul sistema locale](./deploy-on-your-system.md)
