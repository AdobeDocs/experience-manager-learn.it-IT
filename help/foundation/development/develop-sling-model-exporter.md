---
title: Sviluppa esportatori di modelli Sling in AEM
description: Questa guida tecnica passa attraverso la configurazione di AEM da utilizzare con Sling Model Exporter, la valorizzazione di un modello Sling esistente utilizzando il framework Exporter per il rendering come JSON, e su come utilizzare le opzioni Esportatore e le annotazioni Jackson per personalizzare ulteriormente l'output.
version: 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# Sviluppa esportatori di modelli Sling

Questa guida tecnica passa attraverso la configurazione di AEM da utilizzare con Sling Model Exporter, la valorizzazione di un modello Sling esistente utilizzando il framework Exporter per il rendering come JSON, e su come utilizzare le opzioni Esportatore e le annotazioni Jackson per personalizzare ulteriormente l&#39;output.

Sling Model Exporter è stato introdotto in Sling Models v1.3.0. Questa nuova funzione consente di aggiungere nuove annotazioni ai modelli Sling che definiscono come il modello può essere esportato come un diverso oggetto Java, o più comunemente, serializzato in un formato diverso come JSON.

Apache Sling fornisce un esportatore Jackson JSON per coprire il caso più comune di esportare modelli Sling come oggetti JSON da utilizzare da parte di consumatori web programmatici, come altri servizi web e applicazioni JavaScript.

## Configurazione di AEM per Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] è una funzione del [!DNL Apache Sling] e non direttamente associati al ciclo di rilascio del prodotto AEM. [!DNL Sling Model Exporter] è compatibile con AEM 6.3 e versioni successive.

## Il caso d’uso per [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] è ideale per sfruttare modelli Sling che contengono già logica di business che supportano le rappresentazioni HTML tramite HTL (o in precedenza JSP) ed espongono la stessa rappresentazione aziendale di JSON per l’utilizzo da parte di servizi Web programmatici o applicazioni JavaScript.

## Creazione di un&#39;esportazione di modelli Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Abilitazione [!DNL Exporter] supporto su [!DNL Sling Model] è facile come aggiungere il `@Exporter` annotazione alla classe Java .

## Applicazione delle opzioni di esportazione del modello Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] supporta il passaggio di opzioni di esportazione per modello all’implementazione di esportazione per determinare in che modo [!DNL Sling Model] viene infine esportato. Queste opzioni generalmente si applicano &quot;globalmente&quot; al modo in cui [!DNL Sling Model] viene esportato, rispetto a per punto dati che può essere fatto tramite annotazioni in linea descritte di seguito.

[!DNL Jackson Exporter] le opzioni includono:

* [Opzioni delle funzioni di mappatura](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opzioni delle funzioni di serializzazione](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Applicazione [!DNL Jackson] annotazioni

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Le implementazioni degli esportatori possono anche supportare annotazioni che possono essere applicate in linea sulla [!DNL Sling Model] , che può fornire un livello di controllo più preciso sulla modalità di esportazione dei dati.

* [[!DNL Jackson Exporter] annotazioni](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Visualizza il codice {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiali di supporto {#supporting-materials}

* [[!DNL Jackson Mapper] Funzionalità Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Funzionalità Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documenti](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
