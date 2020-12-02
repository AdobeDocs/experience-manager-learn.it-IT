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
source-wordcount: '449'
ht-degree: 0%

---


# Utilizzare oak-run.jar per gestire gli indici

[!DNL oak-run.jar]Il comando index (indice) consolida una serie di funzioni per gestire  [!DNL Oak]200 indici in AEM, dalla raccolta di statistiche sugli indici, dall&#39;esecuzione di controlli di coerenza sugli indici e dalla reindicizzazione degli indici stessi.

>[!NOTE]
>
>All&#39;interno di questo articolo e dei video, l&#39;indicizzazione e la reindicizzazione dei termini vengono utilizzate in modo intercambiabile e considerate la stessa operazione.

## [!DNL oak-run.jar] index Command Basics

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* La versione di [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilizzata deve corrispondere alla versione di Oak utilizzata nell&#39;istanza AEM.
* La gestione degli indici con [!DNL oak-run.jar] sfrutta il comando **[!DNL index]** con diversi flag per supportare operazioni diverse.

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

* L&#39;indicizzazione online di [!DNL TarMK] utilizzando [!DNL oak-run.jar] è più rapida rispetto all&#39;impostazione `reindex=true` sul nodo `oak:queryIndexDefinition`. Nonostante questo aumento delle prestazioni, l&#39;indicizzazione online con [!DNL oak-run.jar] richiede ancora una finestra di manutenzione per eseguire l&#39;indicizzazione.

* L&#39;indicizzazione online di [!DNL TarMK] utilizzando [!DNL oak-run.jar] deve essere eseguita su istanze AEM esterne alla finestra di manutenzione delle istanze AEM.****

## Indicizzazione offline TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* L&#39;indicizzazione offline di [!DNL TarMK] utilizzando [!DNL oak-run.jar] è l&#39;approccio di indicizzazione basato su [!DNL oak-run.jar] più semplice per [!DNL TarMK], in quanto richiede un singolo comando [!DNL oak-run.jar], ma richiede che l&#39;istanza AEM sia chiusa.

## Indicizzazione fuori banda TarMK con track-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* L&#39;indicizzazione fuori banda su [!DNL TarMK] con [!DNL oak-run.jar] riduce al minimo l&#39;impatto dell&#39;indicizzazione sulle istanze AEM in uso.
* L&#39;indicizzazione fuori banda è l&#39;approccio di indicizzazione consigliato per AEM installazioni in cui il tempo di re/index supera le finestre di manutenzione disponibili.

## Indice MongoMK Online con Oak-run.jar

* Indice online con [!DNL oak-run.jar] su [!DNL MongoMK] e [!DNL RDBMK] è il metodo consigliato per reindicizzare le installazioni AEM [!DNL MongoMK] (e [!DNL RDBMK]). **Nessun altro metodo deve essere utilizzato per  [!DNL MongoMK] o  [!DNL RDBMK].**
* L&#39;indicizzazione deve essere eseguita solo su una singola istanza AEM nel cluster.
* L&#39;indicizzazione online di [!DNL MongoMK] è sicura da eseguire su un cluster AEM in esecuzione, in quanto l&#39;attraversamento del repository avviene su un solo nodo [!DNL MongoDB], consentendo agli altri di continuare a distribuire le richieste senza un impatto significativo sulle prestazioni.

Il comando di indice [!DNL oak-run.jar] per eseguire un&#39;indicizzazione online di [!DNL MongoMK] è lo [stesso dell&#39;indicizzazione online di  [!DNL TarMK] con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la differenza che il parametro dell&#39;archivio segmenti punta all&#39;istanza [!DNL MongoDB] che contiene l&#39;archivio nodi.

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
