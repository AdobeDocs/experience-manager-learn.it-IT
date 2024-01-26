---
title: Creare un componente per elencare i dati del modulo
description: Tutorial per creare un componente di riepilogo per la revisione dei dati del modulo prima dell’invio.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 40
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# Crea un componente per riepilogare i dati del modulo

È stato creato un componente semplice per elencare i dati del modulo per la revisione. Il [funzione di visita dell’API guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) è stato utilizzato per eseguire iterazioni nei campi modulo. Il codice nella libreria client associato a questo componente genera i componenti pannello/tabella nel modulo. Dagli elementi secondari di questo pannello/componenti di tabella vengono estratti il titolo, il valore e l’espressione SOM dei campi modulo utilizzando i metodi API GuidBridge. Viene quindi creata una semplice tabella HTML con il titolo, il valore e l&#39;espressione SOM per consentire all&#39;utente finale di rivedere/modificare i dati del modulo prima di inviarlo.

Ad esempio, la schermata seguente mostra la tabella creata per elencare i campi e i relativi valori del **I tuoi dettagli**. L&#39;ultima TD nel TR viene utilizzata per modificare il valore del campo utilizzando l&#39;espressione SOM dei campi.

![visit-func](assets/visit-function.png)

## Passaggi successivi

[Test della soluzione sul sistema locale](./deploy-on-your-system.md)
