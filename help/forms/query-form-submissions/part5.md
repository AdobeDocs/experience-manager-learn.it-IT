---
title: Distribuire l’esempio sul server locale
description: Tutorial in più parti che illustra i passaggi necessari per eseguire query sugli invii di moduli memorizzati nel portale di Azure
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 44841a3c-85e0-447f-85e2-169a451d9c68
duration: 20
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Distribuire l’esempio sul server locale

Per utilizzare questo caso sul server locale, segui i passaggi elencati di seguito.Si presume che l’istanza di AEM sia in esecuzione su localhost, porta 4502.

* [Installare il pacchetto](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) utilizzando Gestione pacchetti.

* Fornisci le credenziali del portale di Azure utilizzando OSGi configMgr
  ![azure-portal](assets/azure-portal-config.png)
Verificare che l&#39;URI di archiviazione termini con una barra e che il token SAS inizi con un ?
* Passa a [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Modifica le impostazioni di autenticazione delle seguenti 3 origini dati in base all’ambiente in uso
  ![origini dati](assets/fdm-data-sources.png)

* Anteprima e invio del [modulo Contattaci](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Eseguire una query sull&#39;invio del modulo](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)
