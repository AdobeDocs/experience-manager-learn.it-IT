---
title: Variabili nel flusso di lavoro AEM[Parte2]
seo-title: Variabili nel flusso di lavoro AEM[Parte2]
description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro AEM
seo-description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro AEM
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Variabili di tipo JSON nel flusso di lavoro AEM

A partire da  AEM Forms 6.5, ora è possibile creare variabili di tipo JSON in AEM Workflow. In genere si creano variabili di tipo JSON se si sta inviando Forms adattivo basato su schema JSON a un flusso di lavoro AEM o si desidera memorizzare i risultati di un&#39;operazione di chiamata a un modello dati modulo. Il seguente video illustra i passaggi necessari per creare e usare una variabile di tipo JSON nel flusso di lavoro AEM
>[!NOTE]

**Se utilizzate  AEM Forms 6.5.0**

Quando si crea una variabile di tipo JSON per acquisire i dati inviati nel modello di workflow, non associare lo schema JSON alla variabile. Questo perché quando si invia un modulo adattivo basato sullo schema JSON i dati inviati non sono conformi allo schema JSON. I dati di reclamo dello schema JSON sono racchiusi nell&#39;elemento afData.afBoundData.data.

**Se utilizzate  AEM Forms 6.5.1 e versioni successive**

È possibile mappare lo schema con la variabile di tipo JSON nel modello di workflow. È quindi possibile utilizzare il browser dello schema per mappare gli elementi dello schema con le variabili stringa/numero nel modello di workflow

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)

**La possibilità di espandere gli elementi dello schema e mappare l&#39;elemento dello schema alla variabile del flusso di lavoro è disponibile solo  AEM Forms 6.5.1 in avanti.**

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Per utilizzare le risorse sul sistema, effettuate le seguenti operazioni:

* [Scaricate e importate le risorse in AEM utilizzando il gestore pacchetti](assets/jsonandstringvariable.zip)
* [Esplora il modello](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) di workflow per comprendere le variabili utilizzate nel flusso di lavoro
* [Configurare il servizio e-mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Aprire il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia il modulo
