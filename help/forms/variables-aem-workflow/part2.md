---
title: Variabili nel flusso di lavoro dell’AEM[Parte2]
description: Utilizzo di variabili di tipo XML, JSON, ArrayList, Document in un flusso di lavoro AEM
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 360
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Variabili di tipo JSON nel flusso di lavoro AEM

A partire da AEM Forms 6.5, ora è possibile creare variabili di tipo JSON nel flusso di lavoro dell’AEM. In genere, si creano variabili di tipo JSON se si invia un Forms adattivo basato sullo schema JSON a un flusso di lavoro AEM o si desidera memorizzare i risultati di un’operazione di richiamo di un modello di dati modulo. Il video seguente illustra i passaggi necessari per creare e utilizzare una variabile di tipo JSON nel flusso di lavoro AEM

**Se utilizzi AEM Forms 6.5.0**

Quando crei una variabile di tipo JSON per acquisire i dati inviati nel modello di flusso di lavoro, non associare lo schema JSON alla variabile. Questo perché quando invii un modulo adattivo basato su schema JSON, i dati inviati non sono conformi allo schema JSON. I dati del reclamo dello schema JSON sono racchiusi nell&#39;elemento afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Se si utilizza AEM Forms 6.5.1 e versioni successive**

Puoi mappare lo schema con la variabile di tipo JSON nel modello di flusso di lavoro. Puoi quindi utilizzare il browser schemi per mappare gli elementi dello schema con le variabili stringa/numero nel modello di flusso di lavoro

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Per far sì che le risorse funzionino sul sistema, segui i passaggi seguenti:

* [Scaricare e importare le risorse in AEM utilizzando Gestione pacchetti](assets/jsonandstringvariable.zip)
* [Esplora il modello di flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) per comprendere le variabili utilizzate nel flusso di lavoro
* [Configurare il servizio e-mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Aprire il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia il modulo
