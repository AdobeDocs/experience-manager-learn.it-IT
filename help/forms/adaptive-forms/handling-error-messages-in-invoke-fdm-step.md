---
title: Acquisizione dei messaggi di errore nel servizio del modello dati del modulo come passaggio nel flusso di lavoro
description: A partire da AEM Forms 6.5.1, ora è possibile acquisire i messaggi di errore generati utilizzando il Servizio modello dati modulo di chiamata come passaggio nel flusso di lavoro AEM. Flusso di lavoro
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
duration: 61
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# Acquisizione dei messaggi di errore nel passaggio del servizio Richiama modello dati modulo

A partire da AEM Forms 6.5.1, ora è possibile acquisire i messaggi di errore e specificare le opzioni di convalida. Il passaggio Richiama servizio modello dati modulo è stato migliorato per fornire le seguenti funzionalità.

* Fornire un’opzione per la convalida a 3 livelli (&quot;OFF&quot;, &quot;BASIC&quot; e &quot;FULL&quot;) per gestire le eccezioni riscontrate durante la chiamata al servizio del modello dati del modulo. Le tre opzioni indicano in successione una versione più rigorosa del controllo dei requisiti specifici del database.
  ![livelli di convalida](assets/validation-level.PNG)

* Casella di controllo per personalizzare l&#39;esecuzione del flusso di lavoro. Pertanto, l’utente ora ha la flessibilità di procedere con l’esecuzione del flusso di lavoro, anche se il passaggio Richiama modello dati modulo genera eccezioni.

* Memorizzazione di informazioni importanti di errore causato da eccezioni di convalida. Sono stati incorporati tre selettori di variabili di tipo Completamento automatico per selezionare le variabili rilevanti per memorizzare ErrorCode(String), ErrorMessage(String) e ErrorDetails(JSON). ErrorDetails, tuttavia, viene impostato su null nel caso in cui l&#39;eccezione non sia DermisValidationException.
  ![acquisizione dei messaggi di errore](assets/fdm-error-details.PNG)

Con queste modifiche, il passaggio del servizio Richiama modello dati modulo assicura che i valori di input siano conformi ai vincoli di dati forniti nel file swagger. Ad esempio, il seguente messaggio di errore viene generato quando i valori accountId e balance non sono conformi ai vincoli di dati specificati nel file swagger.

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
