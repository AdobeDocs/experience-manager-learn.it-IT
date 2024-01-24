---
title: Integrazione con [!DNL ServiceNow]
description: Crea e visualizza tutti i problemi utilizzando il modello dati modulo.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 56
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Integrare AEM Forms con [!DNL ServiceNow]

Crea e visualizza l’incidente in [!DNL ServiceNow] utilizzo del modello dati modulo in AEM Forms.

## Prerequisiti

* [!DNL ServiceNow] account.
* Familiare con [creazione di origini dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiare con [Modello dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Risorse di esempio

Le risorse di esempio fornite con questo articolo includono:

* Configurazione servizio cloud
* File Swagger per creare un incidente e recuperare tutti gli incidenti
* Modello dati modulo basato sui file Swagger
* Modulo adattivo da creare ed elencare [!DNL ServiceNow] incidenti

## Distribuire le risorse sul server

* Scarica il file [risorse di esempio](assets/service-now.zip)
* Importa le risorse in AEM utilizzando [gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Il file swagger utilizzato per questa integrazione si trova sotto ```/conf/9957/settings/cloudconfigs/fdm``` cartella nell’archivio crx
* Modifica il [Configurazione del servizio cloud CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)per corrispondere all’istanza ServiceNow.
* Modifica il [Configurazione servizio cloud GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) per corrispondere all’istanza ServiceNow. È necessario modificare l&#39;host, il nome utente e la password in modo che corrispondano alle credenziali dell&#39;istanza ServiceNow.

## Accedere alle credenziali dell&#39;istanza ServiceNow

* Fai clic sul profilo utente
  ![fai clic sul profilo utente](assets/snow-1.png)

* Fai clic su Gestisci password istanza
* I dettagli dell’istanza sono mostrati come segue
  ![dettagli dell’istanza](assets/snow-3.png)

## Testare l’integrazione

* [Aprire il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Immettere alcuni valori nel campo descrizione e commenti e fare clic sul pulsante Crea incidente
* L&#39;ID dell&#39;incidente appena creato deve essere inserito nel campo di testo e la tabella seguente deve elencare tutti gli incidenti.
