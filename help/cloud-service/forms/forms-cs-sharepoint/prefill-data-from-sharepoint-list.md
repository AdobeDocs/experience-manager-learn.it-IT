---
title: Precompilare un modulo adattivo con i dati dell’elenco di SharePoint
description: Scopri come precompilare un modulo adattivo utilizzando il modello di dati del modulo supportato da un elenco di punti di condivisione
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14795
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 46
exl-id: 9abe9f9d-8fb3-4e01-a830-1dad1c27274d
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Precompila il modulo adattivo con i dati dell’elenco dei punti di condivisione

Nella versione precedente di AEM Form(6.5), era necessario scrivere il codice personalizzato per precompilare il modulo adattivo basato sul modello di dati del modulo utilizzando l’attributo di richiesta. In AEM Forms as a Cloud Service non è più necessario scrivere codice personalizzato.

Questo articolo illustra i passaggi necessari per precompilare/precompilare un modulo adattivo con dati recuperati dall’elenco di SharePoint utilizzando il servizio di precompilazione del modello dati del modulo.

In questo articolo si presuppone che [il modulo adattivo sia stato configurato correttamente per l&#39;invio di dati all&#39;elenco di SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=it#connect-af-sharepoint-list)

Di seguito sono riportati i dati dell&#39;elenco SharePoint
![elenco SharePoint](assets/list-data.png)

Per precompilare un modulo adattivo con i dati associati a un determinato GUID, è necessario eseguire i seguenti passaggi

## Configurare il servizio GET

* Creare un servizio get per l’oggetto di livello superiore del modello dati del modulo utilizzando l’attributo guid
  ![get-service](assets/mapping-request-attribute.png)

In questa schermata, la colonna GUID è associata tramite un attributo di richiesta denominato `submissionid`.

Il servizio GET completamente configurato si presenta così

![get-service](assets/fdm-request-attribute.png)

## Configurare il modulo adattivo per l’utilizzo del servizio di precompilazione del modello di dati del modulo

* Apri un modulo adattivo basato sul modello dati del modulo elenco punti di condivisione. Associa il servizio di precompilazione del modello di dati del modulo
  ![modulo-precompilato-servizio](assets/form-prefill-service.png)

## Testare il modulo

Visualizza l&#39;anteprima del modulo includendo `submissionid` nell&#39;URL come mostrato di seguito

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```
