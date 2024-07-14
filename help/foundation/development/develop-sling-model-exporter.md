---
title: Sviluppare esportatori di modelli Sling in AEM
description: Questa guida tecnica illustra come configurare AEM per l’utilizzo con Sling Model Exporter, migliorare un modello Sling esistente utilizzando il framework Exporter per il rendering come JSON e come utilizzare le opzioni Exporter e le annotazioni Jackson per personalizzare ulteriormente l’output.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
duration: 932
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Sviluppa esportatori modello Sling

Questa guida tecnica illustra come configurare AEM per l’utilizzo con Sling Model Exporter, migliorare un modello Sling esistente utilizzando il framework Exporter per il rendering come JSON e come utilizzare le opzioni Exporter e le annotazioni Jackson per personalizzare ulteriormente l’output.

Sling Model Exporter è stato introdotto in Sling Models v1.3.0. Questa nuova funzione consente di aggiungere nuove annotazioni ai modelli Sling che definiscono come il modello può essere esportato come un oggetto Java diverso o, più comunemente, serializzato in un formato diverso, ad esempio JSON.

Apache Sling fornisce una funzione di esportazione JSON di Jackson per coprire il caso più comune di esportazione di modelli Sling come oggetti JSON per l’utilizzo da parte di consumatori web programmatici come altri servizi web e applicazioni JavaScript.

## Configurazione di AEM per Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] è una funzionalità del progetto [!DNL Apache Sling] e non è associato direttamente al ciclo di rilascio del prodotto AEM. [!DNL Sling Model Exporter] è compatibile con AEM 6.3 e versioni successive.

## Caso d&#39;uso per [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] è perfetto per sfruttare modelli Sling che contengono già regole business che supportano le rappresentazioni HTML tramite HTL (o in precedenza JSP) ed esporre la stessa rappresentazione business di JSON per l&#39;utilizzo da parte di servizi Web programmatici o applicazioni JavaScript.

## Creazione di un’esportazione di modelli Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

Abilitare il supporto per [!DNL Exporter] in un [!DNL Sling Model] è facile come aggiungere l&#39;annotazione `@Exporter` alla classe Java.

## Applicazione delle opzioni Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] supporta il passaggio delle opzioni di esportazione per modello all&#39;implementazione di Exporter per determinare il modo in cui [!DNL Sling Model] viene infine esportato. Queste opzioni si applicano in genere &quot;globalmente&quot; alla modalità di esportazione di [!DNL Sling Model], rispetto al punto dati che può essere eseguito tramite annotazioni in linea descritte di seguito.

Le opzioni [!DNL Jackson Exporter] includono:

* [Opzioni funzionalità mappatore](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opzioni della funzione di serializzazione](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Applicazione di [!DNL Jackson] annotazioni

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Le implementazioni degli esportatori possono inoltre supportare annotazioni che possono essere applicate in linea alla classe [!DNL Sling Model], in modo da fornire un livello di controllo più preciso sulle modalità di esportazione dei dati.

* [[!DNL Jackson Exporter] annotazioni](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Visualizza il codice {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiali di supporto {#supporting-materials}

* [[!DNL Jackson Mapper] Funzionalità Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Funzionalità Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documenti](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
