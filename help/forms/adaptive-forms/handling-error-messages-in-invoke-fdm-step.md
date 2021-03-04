---
title: Acquisizione di messaggi di errore nel servizio Form Data Model come passaggio nel flusso di lavoro
seo-title: Acquisizione di messaggi di errore nel servizio Form Data Model come passaggio nel flusso di lavoro
description: A partire da AEM Forms 6.5.1, ora abbiamo la possibilità di acquisire messaggi di errore generati utilizzando l’invocazione di Form Data Model Service come passaggio nel flusso di lavoro AEM. Flusso di lavoro.
seo-description: A partire da AEM Forms 6.5.1, ora abbiamo la possibilità di acquisire messaggi di errore generati utilizzando l’invocazione di Form Data Model Service come passaggio nel flusso di lavoro AEM. Flusso di lavoro.
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---


# Acquisizione dei messaggi di errore nel passaggio del servizio del modello dati del modulo

A partire da AEM Forms 6.5.1, ora è possibile acquisire messaggi di errore e specificare le opzioni di convalida. È stato migliorato il passaggio del Servizio modello dati modulo per richiamare le funzionalità seguenti.

* È disponibile un&#39;opzione per la convalida a 3 livelli (&quot;OFF&quot;, &quot;BASIC&quot; e &quot;FULL&quot;) per gestire le eccezioni rilevate durante l&#39;avvio del servizio Form Data Model. Le 3 opzioni indicano successivamente una versione più rigorosa del controllo dei requisiti specifici del database.
   ![livelli di convalida](assets/validation-level.PNG)

* Casella di controllo per personalizzare l’esecuzione del flusso di lavoro. Pertanto, l’utente avrà la flessibilità di procedere con l’esecuzione del flusso di lavoro, anche se il passaggio Invoke Form Data Model genera eccezioni.

* Memorizzazione di informazioni importanti sull&#39;errore che si verifica a causa di eccezioni di convalida. Sono stati incorporati tre selettori di variabili di tipo Autocomplete per selezionare le variabili rilevanti in modo da memorizzare i valori ErrorCode(String), ErrorMessage(String) e ErrorDetails(JSON). ErrorDetails viene tuttavia impostato su null se l&#39;eccezione non è un DermisValidationException.
   ![acquisizione di messaggi di errore](assets/fdm-error-details.PNG)

Con queste modifiche, il passaggio Invoke Form Data Model Service verifica che i valori di input siano conformi ai vincoli dati forniti nel file swagger. Ad esempio, il seguente messaggio di errore verrà inviato quando i valori accountId e balance non sono conformi ai vincoli dati specificati nel file swagger.

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


