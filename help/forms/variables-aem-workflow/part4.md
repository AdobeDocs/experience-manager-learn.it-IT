---
title: Variabili nel flusso di lavoro AEM[Parte4]
seo-title: Variabili nel flusso di lavoro AEM[Parte4]
description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro aem
seo-description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro aem
feature: Flusso di lavoro
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# Variabile ArrayList nel flusso di lavoro AEM

Le variabili di tipo ArrayList sono state introdotte in AEM Forms 6.5. Un caso d&#39;uso comune per l&#39;utilizzo della variabile ArrayList è quello di definire percorsi personalizzati da utilizzare nell&#39;oggetto AssignTask.

Per utilizzare la variabile ArrayList in un flusso di lavoro AEM, è necessario creare un modulo adattivo che generi elementi ripetuti nei dati inviati. Una pratica comune consiste nel definire uno schema contenente un elemento array. Ai fini di questo articolo, ho creato un semplice schema JSON contenente elementi array. Il caso d&#39;uso è quello di un dipendente che compila una nota spese. Nella nota spese, acquisiamo il nome del responsabile del mittente e il nome del manager del responsabile. I nomi del manager vengono memorizzati in un array denominato managerchain. La schermata seguente mostra il modulo della nota spese e i dati dell’invio di Moduli adattivi.

![rapporto costi](assets/expensereport.jpg)

Di seguito sono riportati i dati dell’invio del modulo adattivo. Il modulo adattivo era basato sullo schema JSON, i dati associati allo schema vengono memorizzati nell&#39;elemento dati dell&#39;elemento afBoundData. La catena di gestione è un array e dobbiamo compilare ArrayList con l&#39;elemento nome dell&#39;oggetto all&#39;interno dell&#39;array della catena di gestione.

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

Per inizializzare la variabile ArrayList della stringa di sottotipo è possibile utilizzare la modalità di mappatura JSON Dot Notation o XPath. La schermata seguente mostra la compilazione di una variabile ArrayList denominata CustomRoutes utilizzando la notazione del punto JSON. Assicurati di puntare a un elemento in un oggetto array come mostrato nella schermata sottostante. Compilazione dell&#39;oggetto ArrayList CustomRoutes con i nomi dell&#39;oggetto array managerchain in corso.
L&#39;ArrayList CustomRoutes viene quindi utilizzato per popolare i cicli nel componente AssignTask
![percorsi personalizzati](assets/arraylist.jpg)
Una volta inizializzata la variabile ArrayList CustomRoutes con i valori dei dati inviati, i percorsi del componente AssignTask vengono quindi compilati utilizzando la variabile CustomRoutes. La schermata seguente mostra i percorsi personalizzati in un oggetto AssignTask
![asingtask](assets/customactions.jpg)

Per testare questo flusso di lavoro sul sistema, segui i seguenti passaggi

* Scarica e salva il file ArrayListVariable.zip nel file system
* [Importare il ](assets/arraylistvariable.zip) file zip utilizzando Gestione pacchetti AEM
* [Apri il modulo TravelExpenseReport](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Inserire un paio di spese e i nomi dei 2 manager
* Premi il pulsante Invia
* [Apri la inbox](http://localhost:4502/aem/inbox)
* Verrà visualizzata una nuova attività denominata &quot;Assegna a amministratore spese&quot;
* Apri il modulo associato all’attività
* Dovresti visualizzare due percorsi personalizzati con i nomi dei manager
   [Esplora il flusso di lavoro ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Questo flusso di lavoro utilizza la variabile ArrayList, la variabile di tipo JSON, l&#39;editor di regole nel componente Or-Split
