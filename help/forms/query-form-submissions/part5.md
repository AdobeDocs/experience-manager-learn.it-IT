---
title: Distribuire l’esempio sul server locale
description: Tutorial in più parti che illustra i passaggi necessari per eseguire query sugli invii di moduli memorizzati nel portale di Azure
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Distribuire l’esempio sul server locale

Per utilizzare questo caso sul server locale, eseguire la procedura seguente.Si presume che l&#39;istanza AEM sia in esecuzione su localhost, porta 4502.

* [Installare il pacchetto](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) utilizzo di gestione pacchetti.

* Fornisci le credenziali del portale di Azure utilizzando OSGi configMgr
  ![azure-portal](assets/azure-portal-config.png)
Verificare che l&#39;URI di archiviazione termini con una barra e che il token SAS inizi con un ?
* Accedi a [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Modifica le impostazioni di autenticazione delle seguenti 3 origini dati in base all’ambiente in uso
  ![origini dati](assets/fdm-data-sources.png)

* Anteprima e invio [Modulo ContactUs](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Eseguire una query sull’invio del modulo](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

