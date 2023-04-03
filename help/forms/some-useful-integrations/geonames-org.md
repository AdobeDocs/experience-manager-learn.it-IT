---
title: Elenco a discesa a cascata
description: Compilare elenchi a discesa in base a una selezione precedente di elenchi a discesa.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# Elenchi a discesa a cascata

Un elenco a discesa a cascata è una serie di controlli DropDownList dipendenti in cui un controllo DropDownList dipende dai controlli DropDownList principali o precedenti. Gli elementi del controllo DropDownList vengono compilati in base a un elemento selezionato dall&#39;utente da un altro controllo DropDownList.

## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=12&learn=on)

Ai fini di questa esercitazione, ho utilizzato [API REST Geonames](http://api.geonames.org/) dimostrare questa funzionalità.
Esistono diverse organizzazioni che forniscono questo tipo di servizio e, purché abbiano ben documentato API REST, è possibile integrarsi facilmente con AEM Forms utilizzando la funzionalità di integrazione dei dati

Sono stati seguiti i seguenti passaggi per implementare elenchi a discesa a cascata in AEM Forms

## Creare un account sviluppatore

Crea un account sviluppatore con [Geonames](https://www.geonames.org/login). Prendi nota del nome utente. Questo nome utente è necessario per richiamare le API REST di geonames.org.

## Crea file Swagger/OpenAPI

La specifica OpenAPI (precedentemente specifica Swagger) è un formato di descrizione API per le API REST. Un file OpenAPI ti consente di descrivere l’intera API, tra cui:

* Endpoint disponibili (/users) e operazioni su ciascun endpoint (GET/users, POST/users)
* Parametri operativi Input ed output per ogni operazione Metodi di autenticazione
* Informazioni di contatto, licenza, termini di utilizzo e altre informazioni.
* Le specifiche API possono essere scritte in YAML o JSON. Il formato è facile da imparare e leggibile sia per gli umani che per le macchine.

Per creare il tuo primo file swagger/OpenAPI, segui [Documentazione OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms supporta la specifica OpenAPI versione 2.0 (Swagger FKA).

Utilizza la [editor swagger](https://editor.swagger.io/) per creare il file swagger per descrivere le operazioni che recuperano tutti i paesi e gli elementi figlio del paese o dello stato. Il file swagger può essere creato in formato JSON o YAML. Il file swagger completato può essere scaricato da [qui](assets/swagger-files.zip)
I file swagger descrivono la seguente API REST
* [Ottieni tutti i paesi](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [Oggetto Get Children of Geoname](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## Creazione di origini dati

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [crea origine dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) nella configurazione dei servizi cloud. Utilizza la [file swagger](assets/swagger-files.zip) per creare le origini dati.
Sarà necessario creare 2 origini dati (una per recuperare tutti i paesi e altre per ottenere elementi figlio)


## Crea modello dati modulo

L’integrazione dei dati di AEM Forms offre un’interfaccia utente intuitiva con cui creare e lavorare [modelli di dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Basare il modello dati del modulo sulle origini dati create nel passaggio precedente. Modello dati modulo con 2 origini dati

![fdm](assets/geonames-fdm.png)


## Creare un modulo adattivo

Per compilare gli elenchi a discesa, è necessario integrare le chiamate GET del modello dati modulo con il modulo adattivo.
Crea un modulo adattivo con 2 elenchi a discesa. Uno per elencare i paesi e uno per elencare gli stati/province a seconda del paese selezionato.

### Elenco a discesa Popolare paesi

L’elenco dei paesi viene compilato quando il modulo viene inizializzato per la prima volta. La schermata seguente mostra l’editor di regole configurato per popolare le opzioni dell’elenco a discesa del paese. Dovrai fornire al tuo nome utente l&#39;account dei nomi geografici per far sì che questo funzioni.
![paesi](assets/get-countries-rule-editor.png)

#### Compilare l’elenco a discesa Regione/Provincia

È necessario compilare l’elenco a discesa Stato/Provincia in base al paese selezionato. La seguente schermata mostra la configurazione dell’editor di regole
![provincia-opzioni](assets/state-province-options.png)

### Esercizio

Aggiungi 2 elenchi a discesa denominati contee e città nel modulo per elencare le contee e la città in base al paese e allo stato/provincia selezionati.
![esercizio](assets/cascading-drop-down-exercise.png)
