---
title: Integrazione con [!DNL ServiceNow]
description: Crea e visualizza tutti gli incidenti utilizzando il modello dati del modulo.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 2%

---

# Integrare AEM Forms con [!DNL ServiceNow]

Crea e visualizza l&#39;incidente in [!DNL ServiceNow] utilizzo di Form Data Model in AEM Forms.

## Prerequisiti

* [!DNL ServiceNow] conto.
* Familiare con [creazione di origini dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiare con [Modello dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Risorse di esempio

Le risorse di esempio fornite con questo articolo includono quanto segue

* Configurazione del servizio cloud
* File Swagger per creare un incidente e recuperare tutti gli incidenti
* Modello dati modulo basato sui file swagger
* Modulo adattivo da creare ed elencare [!DNL ServiceNow] incidenti

## Distribuire le risorse sul server

* Scarica la [risorse di esempio](assets/service-now.zip)
* Importa le risorse in AEM utilizzando [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Il file swagger utilizzato per questa integrazione si trova sotto la ```/conf/9957/settings/cloudconfigs/fdm``` cartella nell&#39;archivio crx
* Modifica le [Configurazione del servizio cloud CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)per corrispondere all&#39;istanza ServiceNow.
* Modifica le [Configurazione del servizio cloud GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) per corrispondere all&#39;istanza ServiceNow. Dovrai cambiare l&#39;host, il nome utente e la password per far corrispondere le credenziali dell&#39;istanza ServiceNow.

## Credenziali dell&#39;istanza di Access ServiceNow

* Fai clic sul tuo profilo utente
   ![fare clic sul profilo utente](assets/snow-1.png)

* Fai clic su Gestisci password istanza
* I dettagli dell’istanza sono visualizzati come segue
   ![dettagli dell&#39;istanza](assets/snow-3.png)

## Verificare l’integrazione

* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Immetti alcuni valori nel campo descrizione e commenti e fai clic sul pulsante Crea incidente .
* L&#39;ID dell&#39;incidente appena creato dovrebbe essere popolato nel campo di testo e la tabella seguente dovrebbe elencare tutti gli incidenti.
