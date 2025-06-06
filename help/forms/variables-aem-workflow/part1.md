---
title: Variabili nel flusso di lavoro di AEM[Part1]
description: Utilizzo di variabili di tipo XML, JSON, ArrayList, Document in un flusso di lavoro di AEM
feature: Adaptive Forms, Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# Variabili XML nel flusso di lavoro AEM

Le variabili di tipo XML vengono in genere utilizzate quando si dispone di un modulo adattivo basato su XSD e si desidera estrarre i valori dall’invio del modulo adattivo nel flusso di lavoro.

Il video seguente illustra i passaggi necessari per creare variabili di tipo String e XML e utilizzarle nel flusso di lavoro.

La variabile XML può essere utilizzata per precompilare il modulo adattivo o per memorizzare i dati di invio del modulo adattivo nel flusso di lavoro.

La variabile stringa può essere compilata da Xpathing nella variabile XML. Questa variabile stringa viene quindi generalmente utilizzata per popolare i segnaposto del modello e-mail nel componente Invia e-mail

>[!NOTE]
>
>Se il modulo adattivo non è associato a XSD, l’XPath per ottenere il valore di un elemento sarà simile a
>
>**/afData/afUnboundData/data/submitterName**

I dati del modulo adattivo vengono memorizzati sotto l’elemento dati come mostrato sopra. **_In XPath submitterName è il nome del campo di testo nel modulo adattivo._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - Quando si crea una variabile di tipo XML per acquisire i dati inviati nel modello di flusso di lavoro, non associare l&#39;XSD alla variabile. Questo perché quando invii un modulo adattivo basato su XSD, i dati inviati non sono conformi a XSD. I dati del reclamo XSD sono racchiusi nell&#39;elemento /afData/afBoundData/.
>
>**AEM Forms 6.5.1** - Se associ XSD alla variabile XML, puoi sfogliare gli elementi dello schema per eseguire il mapping delle variabili. Non potrai accedere ai dati del modulo non associati agli elementi dello schema. Se il caso d’uso prevede l’accesso a dati associati a elementi dello schema e a dati non associati, non associare lo schema alla variabile XML nel flusso di lavoro.Sarà necessario utilizzare l’espressione XPath appropriata per accedere ai dati necessari

## Creazione di variabili XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### Utilizzo di uno schema con una variabile XML

**Mappatura di una variabile XML con lo schema. Utilizza questa funzionalità con AEM Forms 6.5.1 e versioni successive**

>[!VIDEO](https://video.tv.adobe.com/v/328816?quality=12&learn=on&captions=ita)

#### Utilizzo della variabile nell’e-mail di invio

>[!VIDEO](https://video.tv.adobe.com/v/328817?quality=12&learn=on&captions=ita)

Per far sì che le risorse funzionino sul sistema, segui i passaggi seguenti:

* [Scaricare e importare le risorse in AEM tramite Gestione pacchetti](assets/xmlandstringvariable.zip)
* [Esplora il modello di flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) per comprendere le variabili utilizzate nel flusso di lavoro
* [Configura il servizio e-mail](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia il modulo.
