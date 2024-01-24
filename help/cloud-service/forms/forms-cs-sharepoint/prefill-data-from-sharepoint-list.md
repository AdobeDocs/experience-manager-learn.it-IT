---
title: Precompilare un modulo adattivo con i dati dell’elenco di SharePoint
description: Scopri come precompilare un modulo adattivo utilizzando il modello di dati del modulo supportato da un elenco di punti di condivisione
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14795
duration: 53
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# Precompila il modulo adattivo con i dati dell’elenco dei punti di condivisione

Nella versione precedente di AEM Form(6.5), era necessario scrivere un codice personalizzato per precompilare un modulo adattivo basato sul modello di dati del modulo utilizzando l’attributo di richiesta. In AEM Forms as a Cloud Service non è più necessario scrivere codice personalizzato.

Questo articolo illustra i passaggi necessari per precompilare/precompilare un modulo adattivo con dati recuperati dall’elenco di SharePoint utilizzando il servizio di precompilazione del modello dati del modulo.

Questo articolo presuppone che tu abbia [modulo adattivo configurato correttamente per inviare dati a sharepoint list.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)

Di seguito sono riportati i dati dell&#39;elenco SharePoint
![sharepoint-list](assets/list-data.png)

Per precompilare un modulo adattivo con i dati associati a un determinato GUID, è necessario eseguire i seguenti passaggi

## Configurare il servizio GET

* Creare un servizio get per l’oggetto di livello superiore del modello dati del modulo utilizzando l’attributo guid
  ![get-service](assets/mapping-request-attribute.png)

In questa schermata, la colonna GUID è associata tramite un attributo di richiesta denominato `submissionid`.

Il servizio GET completamente configurato si presenta così

![get-service](assets/fdm-request-attribute.png)

## Configurare il modulo adattivo per l’utilizzo del servizio di precompilazione del modello di dati del modulo

* Apri un modulo adattivo basato sul modello dati del modulo elenco punti di condivisione. Associa il servizio di precompilazione del modello di dati del modulo
  ![form-prefill-service](assets/form-prefill-service.png)

## Testare il modulo

Visualizzare l’anteprima del modulo includendo `submissionid` nell’URL, come illustrato di seguito

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```




