---
title: Utilizza oak-run.jar per gestire gli indici
description: Il comando dell'indice oak-run.jar consolida una serie di funzioni per gestire gli indici Oak in AEM, dalla raccolta delle statistiche dell'indice, l'esecuzione dei controlli di coerenza dell'indice e la reindicizzazione degli indici stessi.
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Utilizza oak-run.jar per gestire gli indici

[!DNL oak-run.jar]Il comando di indice consolida una serie di funzioni da gestire [!DNL Oak]200 indici in AEM, dalla raccolta delle statistiche sugli indici, dall&#39;esecuzione dei controlli di coerenza degli indici e dalla reindicizzazione degli stessi indici.

>[!NOTE]
>
>All&#39;interno di questo articolo e video i termini indicizzazione e reindicizzazione vengono utilizzati in modo intercambiabile e considerate la stessa operazione.

## [!DNL oak-run.jar] Nozioni di base sui comandi degli indici

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* Versione di [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilizzato deve corrispondere alla versione di Oak utilizzata sull&#39;istanza AEM.
* Gestione degli indici tramite [!DNL oak-run.jar] sfrutta **[!DNL index]** comando con vari flag per supportare operazioni diverse.

   * `java -jar oak-run*.jar index ...`

## Statistiche indice

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` scarica tutte le definizioni dell&#39;indice, gli stati importanti dell&#39;indice e il contenuto dell&#39;indice per l&#39;analisi offline.
* La raccolta delle statistiche dell&#39;indice è sicura da eseguire su istanze AEM in uso.

## Controllo della coerenza dell’indice

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se gli indici lucene Oak sono corrotti.
* Il controllo di coerenza è sicuro da eseguire su AEM istanza in uso per i livelli di controllo di coerenza 1 e 2.

## Inindicizzazione online di TarMK con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Inindicizzazione online di [!DNL TarMK] utilizzo [!DNL oak-run.jar] è più veloce dell&#39;impostazione `reindex=true` sulla `oak:queryIndexDefinition` nodo. Nonostante questo aumento delle prestazioni, indicizzazione online utilizzando [!DNL oak-run.jar] richiede ancora una finestra di manutenzione per eseguire l&#39;indicizzazione.

* Inindicizzazione online di [!DNL TarMK] utilizzo [!DNL oak-run.jar] dovrebbe **not** vengono eseguite su istanze AEM al di fuori della finestra di manutenzione delle istanze AEM.

## Indicizzazione offline TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* indicizzazione offline di [!DNL TarMK] utilizzo [!DNL oak-run.jar] è il più semplice [!DNL oak-run.jar] approccio basato sull&#39;indicizzazione per [!DNL TarMK] poiché richiede un singolo [!DNL oak-run.jar] tuttavia richiede lo spegnimento dell&#39;istanza AEM.

## indicizzazione fuori banda di TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* Indicizzazione fuori banda su [!DNL TarMK] utilizzo [!DNL oak-run.jar] riduce al minimo l’impatto dell’indicizzazione sulle istanze di AEM in uso.
* L’indicizzazione fuori banda è l’approccio di indicizzazione consigliato per le installazioni AEM in cui il tempo di re/indicizzazione supera le finestre di manutenzione disponibili.

## Inindicizzazione online MongoMK con oak-run.jar

* Indice online con [!DNL oak-run.jar] su [!DNL MongoMK] e [!DNL RDBMK] è il metodo consigliato per la re/indicizzazione [!DNL MongoMK] e [!DNL RDBMK]) AEM installazioni. **Nessun altro metodo deve essere utilizzato per [!DNL MongoMK] o [!DNL RDBMK].**
* Questa indicizzazione deve essere eseguita solo su una singola istanza AEM nel cluster.
* Inindicizzazione online di [!DNL MongoMK] è sicuro da eseguire su un cluster AEM in esecuzione, in quanto l&#39;attraversamento del repository si verifica solo su un singolo [!DNL MongoDB] , che consente agli altri di continuare a servire le richieste senza un impatto significativo sulle prestazioni.

La [!DNL oak-run.jar] comando index per eseguire un&#39;indicizzazione online di [!DNL MongoMK] è [come [!DNL TarMK] Inindicizzazione online con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la differenza che il parametro dell’archivio segmenti punta al [!DNL MongoDB] istanza che contiene l&#39;archivio Node.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiali di supporto

* [Download [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Assicurati che la versione scaricata corrisponda alla versione di Oak installata su AEM come descritto sopra*
* [Documentazione del comando dell&#39;indice oak-run.jar di Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
