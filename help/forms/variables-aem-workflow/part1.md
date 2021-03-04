---
title: Variabili nel flusso di lavoro AEM[Parte1]
seo-title: Variabili nel flusso di lavoro AEM[Parte1]
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
source-wordcount: '441'
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
>**AEM Forms 6.5.0**  - Quando crei una variabile di tipo XML per acquisire i dati inviati nel modello di flusso di lavoro, non associare XSD alla variabile. Questo perché quando si invia un modulo adattivo basato su XSD i dati inviati non sono conformi a XSD. I dati del reclamo XSD sono racchiusi nell&#39;elemento /afData/afBoundData/ .
>
>**AEM Forms 6.5.1**  - Se si associa XSD alla variabile XML, è possibile sfogliare gli elementi dello schema per eseguire la mappatura delle variabili. Non sarà possibile accedere ai dati del modulo non associati a elementi dello schema. Se il caso d&#39;uso è quello di accedere a dati associati a elementi dello schema e a dati non associati, non eseguire il binding dello schema con la variabile XML nel flusso di lavoro.Sarà necessario utilizzare l&#39;espressione XPath appropriata per accedere ai dati necessari

## Creazione di variabili XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Utilizzo di uno schema con una variabile XML

**Mappatura di una variabile XML con lo schema. Utilizza questa funzionalità con AEM Forms 6.5.1 a partire da**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Utilizzo della variabile nell’e-mail di invio

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Per far funzionare le risorse sul sistema, effettua le seguenti operazioni:

* [Scarica e importa le risorse in AEM utilizzando il gestore di pacchetti](assets/xmlandstringvariable.zip)
* [Esplora il ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) modello di flusso di lavoro per comprendere le variabili utilizzate nel flusso di lavoro
* [Configurare il servizio e-mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Apri il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia il modulo.

