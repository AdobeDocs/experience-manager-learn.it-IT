---
title: Utilizzare oak-run.jar per gestire gli indici
description: Il comando index di oak-run.jar consolida una serie di funzioni per la gestione degli indici Oak in AEM, dalla raccolta delle statistiche sugli indici all'esecuzione di controlli di coerenza degli indici e alla reindicizzazione degli stessi indici.
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 759
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Utilizzare oak-run.jar per gestire gli indici

[!DNL oak-run.jar]Il comando index consolida una serie di funzionalità da gestire [!DNL Oak]200 indici in AEM, dalla raccolta delle statistiche sugli indici, all&#39;esecuzione di controlli di coerenza degli indici e alla reindicizzazione degli stessi indici.

>[!NOTE]
>
>In questo articolo e nei video i termini indicizzazione e reindicizzazione sono utilizzati in modo intercambiabile e vengono considerati come la stessa operazione.

## [!DNL oak-run.jar] informazioni di base sui comandi index

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* Versione di [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) La versione di Oak utilizzata deve corrispondere alla versione di Oak utilizzata nell’istanza AEM.
* Gestione degli indici tramite [!DNL oak-run.jar] sfrutta **[!DNL index]** con diversi flag per supportare diverse operazioni.

   * `java -jar oak-run*.jar index ...`

## Statistiche indice

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` esegue il dump di tutte le definizioni di indice, delle statistiche di indice importanti e del contenuto dell’indice per l’analisi offline.
* La raccolta delle statistiche dell’indice è sicura per l’esecuzione sulle istanze AEM in uso.

## Verifica coerenza indice

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se gli indici Oak Lucene sono danneggiati.
* La verifica di coerenza può essere eseguita in modo sicuro sull’istanza AEM in uso per i livelli di verifica di coerenza 1 e 2.

## Indicizzazione online di TarMK con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Indicizzazione online di [!DNL TarMK] utilizzo [!DNL oak-run.jar] è più veloce dell&#39;impostazione `reindex=true` il `oak:queryIndexDefinition` nodo. Nonostante questo aumento delle prestazioni, l’indicizzazione online tramite [!DNL oak-run.jar] richiede comunque una finestra di manutenzione per eseguire l’indicizzazione.

* Indicizzazione online di [!DNL TarMK] utilizzo [!DNL oak-run.jar] dovrebbe **non** essere eseguita in relazione alle istanze AEM al di fuori della finestra di manutenzione delle istanze AEM.

## Indicizzazione offline di TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Indicizzazione offline di [!DNL TarMK] utilizzo [!DNL oak-run.jar] è il più semplice [!DNL oak-run.jar] approccio basato sull’indicizzazione per [!DNL TarMK] in quanto richiede un singolo [!DNL oak-run.jar] tuttavia richiede la chiusura dell&#39;istanza AEM.

## Indicizzazione fuori banda di TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* Indicizzazione fuori banda su [!DNL TarMK] utilizzo [!DNL oak-run.jar] riduce al minimo l’impatto dell’indicizzazione sulle istanze AEM in uso.
* L’indicizzazione fuori banda è l’approccio consigliato per le installazioni AEM in cui il tempo di reindicizzazione supera le finestre di manutenzione disponibili.

## Indicizzazione online di MongoMK con oak-run.jar

* Indice online con [!DNL oak-run.jar] il [!DNL MongoMK] e [!DNL RDBMK] è il metodo consigliato per la reindicizzazione [!DNL MongoMK] (e [!DNL RDBMK]) installazioni AEM. **Nessun altro metodo deve essere utilizzato per [!DNL MongoMK] o [!DNL RDBMK].**
* Questa indicizzazione deve essere eseguita solo su una singola istanza AEM nel cluster.
* Indicizzazione online di [!DNL MongoMK] è sicuro da eseguire su un cluster AEM in esecuzione, in quanto l’attraversamento dell’archivio si verifica su un solo [!DNL MongoDB] consentendo agli altri utenti di continuare a soddisfare le richieste senza un impatto significativo sulle prestazioni.

Il [!DNL oak-run.jar] comando index per eseguire un&#39;indicizzazione online di [!DNL MongoMK] è il [uguale al [!DNL TarMK] Indicizzazione online con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) con la differenza che il parametro dell’archivio segmenti punta al [!DNL MongoDB] istanza che contiene l’archivio Nodi.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiali di supporto

* [Scarica [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Assicurati che la versione scaricata corrisponda alla versione di Oak installata su AEM come descritto in precedenza*
* [Documentazione del comando di indice Apache Jackrabbit Oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
