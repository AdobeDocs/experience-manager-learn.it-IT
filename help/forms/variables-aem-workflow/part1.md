---
title: Variabili nel flusso di lavoro AEM[Parte1]
seo-title: Variabili nel flusso di lavoro AEM[Parte1]
description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro AEM
seo-description: Utilizzo di variabili di tipo xml,json,arraylist,document nel flusso di lavoro AEM
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Variabili XML nel flusso di lavoro AEM

Le variabili di tipo XML vengono in genere utilizzate quando si dispone di un modulo adattivo basato su XSD e si desidera estrarre i valori dall&#39;invio del modulo adattivo nel flusso di lavoro.

Il video seguente illustra i passaggi necessari per creare variabili di tipo String e XML e utilizzarle nel flusso di lavoro.

La variabile XML può essere utilizzata per precompilare il modulo adattivo o per memorizzare i dati di invio del modulo adattivo nel flusso di lavoro.

La variabile di stringa può essere compilata da Xpathing nella variabile XML. Questa variabile stringa viene in genere utilizzata per compilare i segnaposto dei modelli di e-mail nel componente Invia e-mail

>[!NOTE]
>
>Se il modulo adattivo non è associato a XSD, l&#39;XPath per ottenere il valore di un elemento sarà simile a
>
>**/afData/afUnboundData/data/submitterName**

I dati del modulo adattivo vengono memorizzati sotto l’elemento dati come mostrato sopra. **_In XPath submitterName sopra è il nome del campo di testo nel modulo adattivo._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - Quando si crea una variabile di tipo XML per acquisire i dati inviati nel modello di workflow, non associare XSD alla variabile. Questo perché quando si invia un modulo adattivo basato su XSD, i dati inviati non sono conformi a XSD. I dati del reclamo XSD sono racchiusi nell&#39;elemento /afData/afBoundData/.
>
>**AEM Forms 6.5.1** - Se si associa XSD alla variabile XML, è possibile esplorare gli elementi dello schema per eseguire la mappatura della variabile. Non sarà possibile accedere ai dati del modulo non associati agli elementi dello schema. Se il caso d&#39;uso è quello di accedere ai dati associati a elementi dello schema e ai dati non associati, non eseguire il binding dello schema con la variabile XML nel flusso di lavoro. Sarà necessario utilizzare l&#39;espressione XPath appropriata per ottenere i dati necessari

## Creazione di variabili XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Uso dello schema con la variabile XML

**Mappatura di una variabile XML con schema. Utilizzate questa funzionalità a partire da  AEM Forms 6.5.1**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Utilizzo della variabile in Invia e-mail

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Per utilizzare le risorse sul sistema, effettuate le seguenti operazioni:

* [Scaricate e importate le risorse in AEM utilizzando il gestore pacchetti](assets/xmlandstringvariable.zip)
* [Esplora il modello](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) di workflow per comprendere le variabili utilizzate nel flusso di lavoro
* [Configurare il servizio e-mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Aprire il modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Compila i dettagli e invia il modulo.

