---
title: Sviluppa esportatori di modelli Sling in AEM
description: Questa guida tecnica passa attraverso la configurazione di AEM da utilizzare con Sling Model Exporter, la valorizzazione di un modello Sling esistente utilizzando il framework Exporter per il rendering come JSON, e su come utilizzare le opzioni Esportatore e le annotazioni Jackson per personalizzare ulteriormente l'output.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: API
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Sviluppo
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---


# Sviluppa esportatori di modelli Sling

Questa guida tecnica passa attraverso la configurazione di AEM da utilizzare con Sling Model Exporter, la valorizzazione di un modello Sling esistente utilizzando il framework Exporter per il rendering come JSON, e su come utilizzare le opzioni Esportatore e le annotazioni Jackson per personalizzare ulteriormente l&#39;output.

Sling Model Exporter è stato introdotto in Sling Models v1.3.0. Questa nuova funzione consente di aggiungere nuove annotazioni ai modelli Sling che definiscono come il modello può essere esportato come un diverso oggetto Java, o più comunemente, serializzato in un formato diverso come JSON.

Apache Sling fornisce un esportatore Jackson JSON per coprire il caso più comune di esportare modelli Sling come oggetti JSON da utilizzare da parte di consumatori web programmatici, come altri servizi web e applicazioni JavaScript.

## Configurazione di AEM per Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] è una funzione del  [!DNL Apache Sling] progetto e non è direttamente legato al ciclo di rilascio del prodotto AEM. [!DNL Sling Model Exporter] è compatibile con AEM 6.3 e versioni successive.

## Il caso d’uso per [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] è ideale per sfruttare modelli Sling che contengono già logica di business che supportano le rappresentazioni HTML tramite HTL (o precedentemente JSP) ed espongono la stessa rappresentazione aziendale di JSON per l’utilizzo da parte di servizi Web programmatici o applicazioni JavaScript.

## Creazione di un&#39;esportazione di modelli Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Abilitare il supporto [!DNL Exporter] su un [!DNL Sling Model] è semplice quanto aggiungere l&#39;annotazione `@Exporter` alla classe Java.

## Applicazione delle opzioni di esportazione del modello Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] supporta il passaggio di opzioni di esportazione per modello all’implementazione di esportazione per determinare in che modo l’esportazione  [!DNL Sling Model] viene finalmente effettuata. Queste opzioni generalmente si applicano &quot;a livello globale&quot; alla modalità di esportazione di [!DNL Sling Model] rispetto a per punto di dati che può essere eseguito tramite annotazioni in linea descritte di seguito.

[!DNL Jackson Exporter] le opzioni includono:

* [Opzioni delle funzioni di mappatura](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opzioni delle funzioni di serializzazione](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Applicazione delle annotazioni [!DNL Jackson]

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Le implementazioni degli esportatori possono inoltre supportare annotazioni che possono essere applicate in linea sulla classe [!DNL Sling Model] , che possono fornire un livello di controllo più preciso sulle modalità di esportazione dei dati.

* [[!DNL Jackson Exporter] annotazioni](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Visualizza il codice {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Materiali di supporto {#supporting-materials}

* [[!DNL Jackson Mapper] Funzionalità Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Funzionalità Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documenti](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
