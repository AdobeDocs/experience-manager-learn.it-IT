---
title: Acquisizione dei messaggi di errore nel servizio del modello dati modulo come passaggio nel flusso di lavoro
seo-title: Acquisizione dei messaggi di errore nel servizio del modello dati modulo come passaggio nel flusso di lavoro
description: A partire da  AEM Forms 6.5.1, ora è possibile acquisire i messaggi di errore generati utilizzando il servizio Richiama Form Data Model Service come passaggio AEM Workflow. Flusso di lavoro.
seo-description: A partire da  AEM Forms 6.5.1, ora è possibile acquisire i messaggi di errore generati utilizzando il servizio Richiama Form Data Model Service come passaggio AEM Workflow. Flusso di lavoro.
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---


# Acquisizione dei messaggi di errore nel passaggio del servizio del modello dati del modulo di chiamata

A partire da  AEM Forms 6.5.1, ora è possibile acquisire i messaggi di errore e specificare le opzioni di convalida. È stato migliorato il passaggio del servizio Richiama il modello dati del modulo per fornire le seguenti funzionalità.

* È disponibile un&#39;opzione per la convalida a 3 livelli (&quot;OFF&quot;, &quot;BASIC&quot; e &quot;FULL&quot;) per gestire le eccezioni rilevate durante la chiamata al servizio Form Data Model. Le 3 opzioni indicano successivamente una versione più rigorosa del controllo dei requisiti specifici del database.
   ![livelli di convalida](assets/validation-level.PNG)

* Casella di controllo per personalizzare l&#39;esecuzione del flusso di lavoro. Di conseguenza, l&#39;utente avrà ora la flessibilità di procedere con l&#39;esecuzione del flusso di lavoro, anche se il passaggio del modello dati del modulo Richiama genera eccezioni.

* Memorizzazione di importanti informazioni relative agli errori derivanti da eccezioni di convalida. Sono stati incorporati tre selettori di variabili di tipo Autocomplete per selezionare le variabili rilevanti per memorizzare ErrorCode(String), ErrorMessage(String) e ErrorDetails(JSON). ErrorDetails, tuttavia, viene impostato su null se l&#39;eccezione non è DermisValidationException.
   ![acquisizione di messaggi di errore](assets/fdm-error-details.PNG)

Con queste modifiche, il passaggio Richiama servizio modello dati modulo assicurerà che i valori di input siano conformi ai vincoli dati forniti nel file swagger. Ad esempio, il seguente messaggio di errore verrà visualizzato quando i valori accountId e balance non sono conformi ai vincoli dati specificati nel file swagger.

```json
{

"errorCode": "AEM-FDM-001-049"

"errorMessage": "Input validations failed during operation execution"

"violations": {

"/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],

"/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]

}

}
```


