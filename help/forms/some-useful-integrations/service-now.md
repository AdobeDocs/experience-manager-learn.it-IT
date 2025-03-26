---
title: Integrazione con  [!DNL ServiceNow]
description: Crea e visualizza tutti i problemi utilizzando il modello dati modulo.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Integrare AEM Forms con [!DNL ServiceNow]

Crea e visualizza l&#39;incidente in [!DNL ServiceNow] utilizzando il modello dati modulo in AEM Forms.

## Prerequisiti

* Account [!DNL ServiceNow].
* Ha familiarità con [creazione origini dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Ha familiarità con [Modello dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Assets di esempio

Le risorse di esempio fornite con questo articolo includono:

* Configurazione servizio cloud
* File Swagger per creare un incidente e recuperare tutto   incidenti
* Modello dati modulo basato sui file Swagger
* Modulo adattivo per creare ed elencare [!DNL ServiceNow] incidenti

## Distribuire le risorse sul server

* Scarica le [risorse di esempio](assets/service-now.zip)
* Importa le risorse in AEM utilizzando [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Il file swagger utilizzato per questa integrazione si trova nella cartella ```/conf/9957/settings/cloudconfigs/fdm``` nell&#39;archivio crx
* Modifica la [configurazione del servizio cloud CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)in modo che corrisponda alla tua istanza ServiceNow.
* Modifica la configurazione del servizio cloud [GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) in modo che corrisponda alla tua istanza ServiceNow. È necessario modificare l&#39;host, il nome utente e la password in modo che corrispondano alle credenziali dell&#39;istanza ServiceNow.

## Accedere alle credenziali dell&#39;istanza ServiceNow

* Fai clic sul profilo utente
  ![fai clic sul profilo utente](assets/snow-1.png)

* Fai clic su Gestisci password istanza
* I dettagli dell’istanza sono mostrati come segue
  ![dettagli istanza](assets/snow-3.png)

## Testare l’integrazione

* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Immettere alcuni valori nel campo descrizione e commenti e fare clic sul pulsante Crea incidente
* L&#39;ID dell&#39;incidente appena creato deve essere inserito nel campo di testo e la tabella seguente deve elencare tutti gli incidenti.
