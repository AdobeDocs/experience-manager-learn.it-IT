---
title: Sviluppare Esportatori di modelli Sling in AEM
description: Questa passeggiata tecnica passa attraverso la configurazione di AEM da utilizzare con Sling Model Exporter, migliorando un modello Sling esistente utilizzando il framework Exporter per la rappresentazione come JSON, e come utilizzare le opzioni Esportatore e le annotazioni Jackson per personalizzare ulteriormente l'output.
version: 6.3, 6.4, 6.5
sub-product: servizi di base, content-services
feature: sling-models, sling-model-exporter
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---


# Sviluppare Esportatori Di Modelli Sling

Questa passeggiata tecnica passa attraverso la configurazione di AEM da utilizzare con Sling Model Exporter, migliorando un modello Sling esistente utilizzando il framework Exporter per la rappresentazione come JSON, e come utilizzare le opzioni Esportatore e le annotazioni Jackson per personalizzare ulteriormente l&#39;output.

Sling Model Exporter è stato introdotto in Sling Models v1.3.0. Questa nuova funzione consente di aggiungere nuove annotazioni ai modelli Sling che definiscono il modo in cui il modello può essere esportato come un oggetto Java diverso, o più comunemente, serializzato in un formato diverso, come JSON.

Apache Sling fornisce un esportatore Jackson JSON per coprire il caso più comune di esportazione di modelli Sling come oggetti JSON da parte di consumatori Web programmatici, come altri servizi Web e applicazioni JavaScript.

## Configurazione di AEM per Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] è una funzione del [!DNL Apache Sling] progetto e non è direttamente legato al ciclo di rilascio del prodotto AEM. [!DNL Sling Model Exporter] è compatibile con AEM 6.3 e versioni successive.

## Il caso d’uso per [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] è perfetto per sfruttare i modelli Sling che già contengono logica di business che supportano le rappresentazioni HTML tramite HTL (o già JSP), ed esporre la stessa rappresentazione aziendale di JSON per l&#39;utilizzo da parte di servizi Web programmatici o applicazioni JavaScript.

## Creazione di un Esportatore di modelli Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Abilitare [!DNL Exporter] il supporto su un [!DNL Sling Model] oggetto è semplice come aggiungere l&#39; `@Exporter` annotazione alla classe Java.

## Applicazione delle opzioni Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] supporta il passaggio delle opzioni di esportazione per modello all&#39;implementazione di Exporter per determinare in che modo [!DNL Sling Model] viene infine esportato. Queste opzioni generalmente si applicano &quot;globalmente&quot; alla modalità di esportazione dell’ [!DNL Sling Model] oggetto, rispetto a quelle per punto di dati che possono essere effettuate tramite annotazioni in linea descritte di seguito.

[!DNL Jackson Exporter] le opzioni includono:

* [Opzioni delle funzioni di mappatura](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opzioni delle funzioni di serializzazione](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Applicazione di [!DNL Jackson] annotazioni

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Le implementazioni di Exporter possono inoltre supportare le annotazioni che possono essere applicate in linea sulla [!DNL Sling Model] classe, che possono fornire un livello di controllo più preciso sulle modalità di esportazione dei dati.

* [[!DNL Jackson Exporter] annotazioni](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Visualizzare il codice {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiali di supporto {#supporting-materials}

* [[!DNL Jackson Mapper] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documenti](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
