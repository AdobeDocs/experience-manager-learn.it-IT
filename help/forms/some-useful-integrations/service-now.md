---
title: Integrazione con il servizio ora
description: Crea e visualizza tutti gli incidenti utilizzando il modello dati del modulo.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 2%

---

# Integrare AEM Forms con servicenow

Crea e visualizza un problema in ServiceNow utilizzando il modello dati modulo in AEM Forms.

## Prerequisiti

* Account ServiceNow.
* Familiare con [creazione di origini dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiare con [Modello dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Risorse di esempio

Le risorse di esempio fornite con questo articolo includono quanto segue
* Configurazione del servizio cloud
* File Swagger per creare un incidente e recuperare tutti gli incidenti
* Modello dati modulo basato sui file swagger
* Modulo adattivo per creare ed elencare i nuovi problemi di servizio

## Distribuire le risorse sul server

* Scarica la [risorse di esempio](assets/service-now.zip)
* Importa le risorse in AEM utilizzando [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Modifica le [Configurazione del servizio cloud CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)per corrispondere all&#39;istanza ServiceNow.
* Modifica le [Configurazione del servizio cloud GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) per corrispondere all’istanza ServiceNow


## Verificare l’integrazione

* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Immetti alcuni valori nel campo descrizione e commenti e fai clic sul pulsante Crea incidente .
* L&#39;ID dell&#39;incidente appena creato dovrebbe essere popolato nel campo di testo e la tabella seguente dovrebbe elencare tutti gli incidenti.
