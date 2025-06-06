---
title: Elenchi a discesa a cascata
description: Compila gli elenchi a discesa in base a una selezione precedente.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
duration: 185
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 0%

---

# Elenchi a discesa a cascata

Un elenco a discesa a cascata è una serie di controlli DropDownList dipendenti in cui un controllo DropDownList dipende dai controlli DropDownList padre o precedente. Gli elementi del controllo DropDownList vengono compilati in base a un elemento selezionato dall&#39;utente da un altro controllo DropDownList.

## Dimostrazione del caso d’uso

>[!VIDEO](https://video.tv.adobe.com/v/3437317?quality=12&learn=on&captions=ita)

Ai fini di questa esercitazione, ho utilizzato [API REST Geonames](https://www.geonames.org/export/web-services.html) per dimostrare questa funzionalità.
Esistono diverse organizzazioni che forniscono questo tipo di servizio e, purché dispongano di API REST ben documentate, puoi facilmente integrarle con AEM Forms utilizzando la funzionalità di integrazione dei dati

Sono stati seguiti i seguenti passaggi per implementare gli elenchi a discesa a cascata in AEM Forms

## Crea account sviluppatore

Crea un account sviluppatore con [Geonames](https://www.geonames.org/login). Prendi nota del nome utente. Questo nome utente è necessario per richiamare le API REST di geonames.org.

## Crea file Swagger/OpenAPI

OpenAPI Specification (precedentemente Swagger Specification) è un formato di descrizione API per le API REST. Un file OpenAPI ti consente di descrivere l’intera API, tra cui:

* Endpoint disponibili (/users) e operazioni su ciascun endpoint (GET /users, POST /users)
* Parametri di funzionamento Input e output per ogni operazione
Metodi di autenticazione
* Informazioni di contatto, licenza, condizioni d’uso e altre informazioni.
* Le specifiche API possono essere scritte in YAML o JSON. Il formato è facile da imparare e leggibile sia per gli esseri umani che per le macchine.

Per creare il primo file swagger/OpenAPI, segui la [documentazione OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms supporta la versione 2.0 delle specifiche OpenAPI (FKA Swagger).

Utilizza l&#39;[editor Swagger](https://editor.swagger.io/) per creare il file Swagger e descrivere le operazioni di recupero di tutti i paesi e degli elementi figlio del paese o dello stato. Il file swagger può essere creato in formato JSON o YAML.

## Creare origini dati

Per integrare AEM/AEM Forms con applicazioni di terze parti, è necessario [creare l&#39;origine dati](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=it) nella configurazione dei servizi cloud. Utilizza i [file Swagger](assets/geonames-swagger-files.zip) per creare le tue origini dati.
Dovrai creare 2 origini dati (una per recuperare tutti i paesi e l’altra per ottenere elementi figlio)


## Crea modello dati modulo

L&#39;integrazione dei dati di AEM Forms fornisce un&#39;interfaccia utente intuitiva per la creazione e l&#39;utilizzo di [modelli di dati modulo](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=it). Basare il modello dati del modulo sulle origini dati create nel passaggio precedente. Modello dati modulo con 2 origini dati

![fdm](assets/geonames-fdm.png)


## Creare un modulo adattivo

Integra le chiamate GET del modello dati del modulo con il modulo adattivo per compilare gli elenchi a discesa.
Crea un modulo adattivo con 2 elenchi a discesa. Uno per elencare i paesi e uno per elencare gli stati/province a seconda del paese selezionato.

### Elenco a discesa Popola paesi

L&#39;elenco dei paesi viene compilato la prima volta che il modulo viene inizializzato. La schermata seguente mostra l’editor di regole configurato per compilare le opzioni dell’elenco a discesa Paese. Affinché ciò funzioni, devi fornire al tuo nome utente l’account dei nomi geografici.
![get-countries](assets/get-countries-rule-editor.png)

#### Popolare l’elenco a discesa Stato/Provincia

È necessario compilare l’elenco a discesa Stato/Provincia in base al paese selezionato. La schermata seguente mostra la configurazione dell’editor di regole
![state-province-options](assets/state-province-options.png)

### Esercizio

Aggiungi 2 elenchi a discesa denominati contee e città nel modulo per elencare le contee e le città in base al paese e allo stato/provincia selezionati.
![esercizio](assets/cascading-drop-down-exercise.png)


### Assets di esempio

Scarica le seguenti risorse per iniziare subito a creare l’esempio dell’elenco a discesa a cascata
I file Swagger completati possono essere scaricati da [qui](assets/geonames-swagger-files.zip)
I file swagger descrivono la seguente API REST
* [Ottieni tutti i paesi](https://secure.geonames.org/countryInfoJSON?username=yourusername)
* [Ottieni elementi figlio dell&#39;oggetto Geoname](https://secure.geonames.org/children?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

Il [modello dati modulo completato può essere scaricato da qui](assets/geonames-api-form-data-model.zip)
