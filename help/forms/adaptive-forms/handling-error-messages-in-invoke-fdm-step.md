---
title: Acquisizione di messaggi di errore nel servizio Form Data Model come passaggio nel flusso di lavoro
description: A partire da AEM Forms 6.5.1, ora è possibile acquisire i messaggi di errore generati utilizzando il servizio di chiamata a Form Data Model come passaggio in AEM flusso di lavoro. Flusso di lavoro.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# Acquisizione dei messaggi di errore nel passaggio del servizio del modello dati del modulo

A partire da AEM Forms 6.5.1, ora è possibile acquisire messaggi di errore e specificare le opzioni di convalida. È stato migliorato il passaggio del Servizio modello dati modulo per richiamare le funzionalità seguenti.

* È disponibile un&#39;opzione per la convalida a 3 livelli (&quot;OFF&quot;, &quot;BASIC&quot; e &quot;FULL&quot;) per gestire le eccezioni rilevate durante l&#39;avvio di Form Data Model Service. Le 3 opzioni indicano successivamente una versione più rigorosa del controllo dei requisiti specifici del database.
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
