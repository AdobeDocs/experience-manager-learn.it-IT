---
title: Utilizza oak-run.jar per gestire gli indici
description: Il comando dell'indice oak-run.jar consolida una serie di funzioni per gestire gli indici Oak in AEM, dalla raccolta delle statistiche dell'indice, l'esecuzione dei controlli di coerenza dell'indice e la reindicizzazione degli indici stessi.
version: 6.4, 6.5
feature: Ricerca
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Spettacolo
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# Utilizza oak-run.jar per gestire gli indici

[!DNL oak-run.jar]Il comando di indice consolida una serie di funzionalità per gestire  [!DNL Oak]200 indici in AEM, dalla raccolta di statistiche sugli indici, l&#39;esecuzione di controlli di coerenza degli indici e la reindicizzazione degli indici stessi.

>[!NOTE]
>
>All&#39;interno di questo articolo e video i termini indicizzazione e reindicizzazione vengono utilizzati in modo intercambiabile e considerate la stessa operazione.

## [!DNL oak-run.jar] Nozioni di base sui comandi degli indici

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* La versione di [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilizzata deve corrispondere alla versione di Oak utilizzata sull&#39;istanza AEM.
* La gestione degli indici utilizzando [!DNL oak-run.jar] sfrutta il comando **[!DNL index]** con vari flag per supportare operazioni diverse.

   * `java -jar oak-run*.jar index ...`

## Statistiche indice

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` scarica tutte le definizioni dell&#39;indice, gli stati importanti dell&#39;indice e il contenuto dell&#39;indice per l&#39;analisi offline.
* La raccolta delle statistiche dell&#39;indice è sicura da eseguire su istanze AEM in uso.

## Controllo della coerenza dell’indice

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se gli indici lucene Oak sono corrotti.
* Il controllo di coerenza è sicuro da eseguire su AEM istanza in uso per i livelli di controllo di coerenza 1 e 2.

## Indicizzazione online TarMK con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* L&#39;indicizzazione online di [!DNL TarMK] utilizzando [!DNL oak-run.jar] è più veloce dell&#39;impostazione `reindex=true` sul nodo `oak:queryIndexDefinition`. Nonostante questo aumento delle prestazioni, l&#39;indicizzazione online utilizzando [!DNL oak-run.jar] richiede ancora una finestra di manutenzione per eseguire l&#39;indicizzazione.

* L&#39;indicizzazione online di [!DNL TarMK] utilizzando [!DNL oak-run.jar] deve essere eseguita su **non** AEM istanze al di fuori della finestra di manutenzione delle istanze AEM.

## Indicizzazione offline TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* L&#39;indicizzazione offline di [!DNL TarMK] utilizzando [!DNL oak-run.jar] è l&#39;approccio di indicizzazione basato su [!DNL oak-run.jar] più semplice per [!DNL TarMK] in quanto richiede un singolo comando [!DNL oak-run.jar], ma richiede lo spegnimento dell&#39;istanza AEM.

## indicizzazione fuori banda di TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* L’indicizzazione fuori banda su [!DNL TarMK] utilizzando [!DNL oak-run.jar] riduce al minimo l’impatto dell’indicizzazione sulle istanze di AEM in uso.
* L’indicizzazione fuori banda è l’approccio di indicizzazione consigliato per le installazioni AEM in cui il tempo di re/indicizzazione supera le finestre di manutenzione disponibili.

## Inindicizzazione online MongoMK con oak-run.jar

* L&#39;indice online con [!DNL oak-run.jar] su [!DNL MongoMK] e [!DNL RDBMK] è il metodo consigliato per le installazioni AEM di reindicizzazione [!DNL MongoMK] (e [!DNL RDBMK]). **Nessun altro metodo deve essere utilizzato per  [!DNL MongoMK] o  [!DNL RDBMK].**
* Questa indicizzazione deve essere eseguita solo su una singola istanza AEM nel cluster.
* L&#39;indicizzazione online di [!DNL MongoMK] è sicura da eseguire su un cluster AEM in esecuzione, in quanto l&#39;attraversamento dell&#39;archivio si verifica solo su un singolo nodo [!DNL MongoDB], consentendo agli altri di continuare a servire le richieste senza un impatto significativo sulle prestazioni.

Il comando di indice [!DNL oak-run.jar] per eseguire un&#39;indicizzazione online di [!DNL MongoMK] è lo [stesso dell&#39; [!DNL TarMK] indicizzazione online con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la differenza che il parametro dell&#39;archivio segmenti punta all&#39;istanza [!DNL MongoDB] che contiene l&#39;archivio dei nodi.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiali di supporto

* [Scarica [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Assicurati che la versione scaricata corrisponda alla versione di Oak installata su AEM come descritto sopra*
* [Documentazione del comando dell&#39;indice oak-run.jar di Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
