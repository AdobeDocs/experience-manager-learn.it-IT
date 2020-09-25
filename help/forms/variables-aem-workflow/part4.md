---
title: Variabili nel flusso di lavoro AEM[Parte4]
seo-title: Variabili nel flusso di lavoro AEM[Parte4]
description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro AEM
seo-description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro AEM
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# Variabile ArrayList nel flusso di lavoro AEM

Le variabili di tipo ArrayList sono state introdotte in  AEM Forms 6.5. Un caso d&#39;uso comune per l&#39;utilizzo della variabile ArrayList è definire percorsi personalizzati da utilizzare nell&#39;oggetto AssignTask.

Per utilizzare la variabile ArrayList in un flusso di lavoro AEM, è necessario creare un modulo adattivo che generi elementi ripetuti nei dati inviati. È pratica comune definire uno schema contenente un elemento di array. Ai fini di questo articolo, ho creato un semplice schema JSON contenente elementi di array. Il caso d&#39;uso è di un dipendente che compila una nota spese. Nella nota spese, acquisiamo il nome del manager dell&#39;autore e il nome del manager dell&#39;autore. I nomi del manager sono memorizzati in un array denominato management chain. La schermata seguente mostra il modulo di nota spese e i dati dall&#39;invio di Forms adattivo.

![costReport](assets/expensereport.jpg)

Di seguito sono riportati i dati provenienti dall&#39;invio del modulo adattivo. Il modulo adattivo era basato sullo schema JSON e i dati associati allo schema sono memorizzati nell&#39;elemento dati dell&#39;elemento afBoundData. Il gestore è un array e dobbiamo compilare ArrayList con l&#39;elemento nome dell&#39;oggetto all&#39;interno dell&#39;array del gestore.

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

Per inizializzare la variabile ArrayList della stringa del sottotipo, è possibile utilizzare la modalità di mappatura JSON Dot Notation o XPath. La schermata seguente mostra la compilazione di una variabile ArrayList denominata CustomRoutes tramite la notazione punto JSON. Assicurarsi di puntare a un elemento in un oggetto array, come mostrato nella schermata sottostante. Stiamo compilando l&#39;ArrayList CustomRoutes con i nomi dell&#39;oggetto array managerchain.
L&#39;ArrayList CustomRoutes viene quindi utilizzato per comporre i cicli nel componente![AssignTask cicli](assets/arraylist.jpg)personalizzati Una volta inizializzata la variabile ArrayList CustomRoutes con i valori dei dati inviati, i cicli del componente AssignTask vengono compilati utilizzando la variabile CustomRoutes. La schermata seguente mostra i percorsi personalizzati in un&#39;attività![AssegnaAttività](assets/customactions.jpg)

Per testare il flusso di lavoro sul sistema, attenetevi alla seguente procedura

* Scaricare e salvare il file ArrayListVariable.zip nel file system
* [Importare il file](assets/arraylistvariable.zip) zip utilizzando AEM Package Manager
* [Aprire il modulo TravelExpenseReport](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Inserire un paio di spese e i nomi dei 2 manager
* Premi il pulsante Invia
* [Aprite la inbox](http://localhost:4502/aem/inbox)
* Dovrebbe essere visualizzata una nuova attività denominata &quot;Assegna a amministratore spese&quot;
* Aprire il modulo associato all&#39;attività
* Dovresti visualizzare due route personalizzate con i nomi dei manager
   [Esplora il flusso di lavoro ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Questo flusso di lavoro utilizza la variabile ArrayList, la variabile di tipo JSON, l&#39;editor di regole nel componente O-Split
