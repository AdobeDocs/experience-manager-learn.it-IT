---
title: Variabili nel flusso di lavoro AEM[Parte1]
description: Utilizzo di variabili di tipo XML, JSON, ArrayList, Document in un flusso di lavoro AEM
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 0%

---

# Variabili XML nel flusso di lavoro AEM

Le variabili di tipo XML vengono generalmente utilizzate quando si dispone di un modulo adattivo basato su XSD e si desidera estrarre i valori dall’invio del modulo adattivo nel flusso di lavoro.

Il video seguente illustra i passaggi necessari per creare variabili di tipo String e XML e utilizzarle nel flusso di lavoro.

La variabile XML può essere utilizzata per precompilare il modulo adattivo o archiviare i dati di invio del modulo adattivo nel flusso di lavoro.

La variabile stringa può essere compilata da Xpathing nella variabile XML. Questa variabile stringa viene quindi generalmente utilizzata per compilare i segnaposto del modello di posta elettronica nel componente Invia e-mail

>[!NOTE]
>
>Se il modulo adattivo non è associato a XSD, l&#39;XPath per ottenere il valore di un elemento sarà simile a
>
>**/afData/afUnboundData/data/submitterName**

I dati del modulo adattivo vengono memorizzati nell’elemento dati come mostrato sopra. **_In XPath submitterName è il nome del campo di testo nel modulo adattivo._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - Quando si crea una variabile di tipo XML per acquisire i dati inviati nel modello di flusso di lavoro, non associare XSD alla variabile. Questo perché quando si invia un modulo adattivo basato su XSD i dati inviati non sono conformi a XSD. I dati del reclamo XSD sono racchiusi nell&#39;elemento /afData/afBoundData/ .
>
>**AEM Forms 6.5.1** - Se si associa XSD alla variabile XML, è possibile sfogliare gli elementi dello schema per eseguire la mappatura della variabile. Non sarà possibile accedere ai dati del modulo non associati a elementi dello schema. Se il caso d&#39;uso è quello di accedere a dati associati a elementi dello schema e a dati non associati, non eseguire il binding dello schema con la variabile XML nel flusso di lavoro.Sarà necessario utilizzare l&#39;espressione XPath appropriata per accedere ai dati necessari

## Creazione di variabili XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### Utilizzo di uno schema con una variabile XML

**Mappatura di una variabile XML con lo schema. Utilizza questa funzionalità con AEM Forms 6.5.1 a partire da**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### Utilizzo della variabile nell’e-mail di invio

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Per far funzionare le risorse sul sistema, effettua le seguenti operazioni:

* [Scarica e importa le risorse in AEM utilizzando il gestore di pacchetti](assets/xmlandstringvariable.zip)
* [Esplorare il modello di flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) per comprendere le variabili utilizzate nel flusso di lavoro
* [Configurare il servizio e-mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia il modulo.
