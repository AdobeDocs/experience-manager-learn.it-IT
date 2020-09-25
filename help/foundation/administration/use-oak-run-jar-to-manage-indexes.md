---
title: Utilizzare oak-run.jar per gestire gli indici
description: il comando indice di oak-run.jar consolida una serie di funzioni per gestire gli indici Oak in AEM, dalla raccolta delle statistiche di indice, dall'esecuzione dei controlli di coerenza dell'indice e dalla reindicizzazione degli indici stessi.
version: 6.4, 6.5
feature: oak
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# Utilizzare oak-run.jar per gestire gli indici

[!DNL oak-run.jar]Il comando index (indice) consolida una serie di funzioni per gestire [!DNL Oak]200 indici in AEM, dalla raccolta di statistiche sugli indici, dall&#39;esecuzione di controlli di coerenza sugli indici e dalla reindicizzazione degli indici stessi.

>[!NOTE]
>
>All&#39;interno di questo articolo e dei video, l&#39;indicizzazione e la reindicizzazione dei termini vengono utilizzate in modo intercambiabile e considerate la stessa operazione.

## [!DNL oak-run.jar] index Command Basics

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* La versione di [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilizzata deve corrispondere alla versione di Oak utilizzata nell&#39;istanza AEM.
* La gestione degli indici mediante [!DNL oak-run.jar] il **[!DNL index]** comando utilizza vari flag per supportare operazioni diverse.

   * `java -jar oak-run*.jar index ...`

## Statistiche indice

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` scarica tutte le definizioni di indice, gli stati di indice importanti e il contenuto dell&#39;indice per l&#39;analisi offline.
* La raccolta delle statistiche di indice è sicura per essere eseguita su istanze AEM in uso.

## Controllo coerenza indice

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se gli indici Oak lucene sono danneggiati.
* La verifica di coerenza è sicura per essere eseguita sull&#39;istanza AEM in uso per i livelli di controllo di coerenza 1 e 2.

## Indice TarMK Online con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* L&#39;indicizzazione online dell&#39; [!DNL TarMK] utilizzo [!DNL oak-run.jar] è più rapida rispetto all&#39;impostazione `reindex=true` sul `oak:queryIndexDefinition` nodo. Nonostante questo aumento delle prestazioni, l&#39;indicizzazione online che utilizza [!DNL oak-run.jar] continua a richiedere una finestra di manutenzione per eseguire l&#39;indicizzazione.

* L&#39;indicizzazione online dell&#39; [!DNL TarMK] utilizzo [!DNL oak-run.jar] non **** deve essere eseguita su istanze AEM all&#39;esterno della finestra di manutenzione delle istanze AEM.

## Indicizzazione offline TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* L&#39;indicizzazione offline dell&#39; [!DNL TarMK] utilizzo [!DNL oak-run.jar] è l&#39;approccio di indicizzazione [!DNL oak-run.jar] basato più semplice per [!DNL TarMK] quanto richiede un singolo [!DNL oak-run.jar] comando, ma richiede che l&#39;istanza AEM venga chiusa.

## Indicizzazione fuori banda TarMK con track-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* L&#39;indicizzazione out-of-band sull&#39; [!DNL TarMK] utilizzo [!DNL oak-run.jar] riduce al minimo l&#39;impatto dell&#39;indicizzazione sulle istanze AEM in uso.
* L&#39;indicizzazione fuori banda è l&#39;approccio di indicizzazione consigliato per AEM installazioni in cui il tempo di re/index supera le finestre di manutenzione disponibili.

## Indice MongoMK Online con Oak-run.jar

* Indice online con [!DNL oak-run.jar] on [!DNL MongoMK] e [!DNL RDBMK] è il metodo consigliato per reindicizzare [!DNL MongoMK] (e [!DNL RDBMK]) AEM installazioni. **Nessun altro metodo deve essere utilizzato per[!DNL MongoMK]o[!DNL RDBMK].**
* L&#39;indicizzazione deve essere eseguita solo su una singola istanza AEM nel cluster.
* L&#39;indicizzazione online di [!DNL MongoMK] è sicura per essere eseguita su un cluster AEM in esecuzione, poiché l&#39;attraversamento del repository avviene su un solo [!DNL MongoDB] nodo, consentendo agli altri di continuare a distribuire le richieste senza un impatto significativo sulle prestazioni.

Il comando [!DNL oak-run.jar] index di cui eseguire un indicizzazione online [!DNL MongoMK] è [uguale all&#39;indicizzazione online con [!DNL TarMK] la differenza che il parametro store segmento punta all&#39; [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) [!DNL MongoDB] istanza che contiene lo store Node.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiali di supporto

* [Scarica [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Verificate che la versione scaricata corrisponda alla versione di Oak installata in AEM come descritto sopra*
* [Apache Jackrabbit Oak roak-run.jar Index Documentazione di comando](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
