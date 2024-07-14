---
title: Utilizzare oak-run.jar per gestire gli indici
description: Il comando index di oak-run.jar consolida una serie di funzioni per la gestione degli indici Oak nell’AEM, dalla raccolta delle statistiche sugli indici all’esecuzione di controlli di coerenza degli indici fino alla reindicizzazione degli stessi indici.
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Utilizzare oak-run.jar per gestire gli indici

Il comando index di [!DNL oak-run.jar] consolida una serie di funzionalità per la gestione degli indici [!DNL Oak]200 in AEM, dalla raccolta delle statistiche sugli indici, all&#39;esecuzione di verifiche di coerenza degli indici e alla reindicizzazione degli stessi indici.

>[!NOTE]
>
>In questo articolo e nei video i termini indicizzazione e reindicizzazione sono utilizzati in modo intercambiabile e vengono considerati come la stessa operazione.

## Nozioni di base sui comandi dell&#39;indice [!DNL oak-run.jar]

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* La versione di [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) utilizzata deve corrispondere alla versione di Oak utilizzata nell&#39;istanza AEM.
* La gestione degli indici tramite [!DNL oak-run.jar] sfrutta il comando **[!DNL index]** con vari flag per supportare diverse operazioni.

   * `java -jar oak-run*.jar index ...`

## Statistiche indice

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` esegue il dump di tutte le definizioni di indice, delle statistiche di indice importanti e del contenuto di indice per l&#39;analisi offline.
* La raccolta delle statistiche dell’indice è sicura per l’esecuzione sulle istanze AEM in uso.

## Verifica coerenza indice

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se gli indici Oak Lucene sono danneggiati.
* La verifica di coerenza può essere eseguita in modo sicuro sull’istanza AEM in uso per i livelli di verifica di coerenza 1 e 2.

## Indicizzazione TarMK Online con [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* L&#39;indicizzazione online di [!DNL TarMK] tramite [!DNL oak-run.jar] è più veloce dell&#39;impostazione di `reindex=true` nel nodo `oak:queryIndexDefinition`. Nonostante questo aumento delle prestazioni, l&#39;indicizzazione online con [!DNL oak-run.jar] richiede ancora una finestra di manutenzione per eseguire l&#39;indicizzazione.

* L&#39;indicizzazione online di [!DNL TarMK] utilizzando [!DNL oak-run.jar] deve **non** essere eseguita su istanze AEM al di fuori della finestra di manutenzione delle istanze AEM.

## Indicizzazione offline di TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* L&#39;indicizzazione offline di [!DNL TarMK] tramite [!DNL oak-run.jar] è l&#39;approccio di indicizzazione basato su [!DNL oak-run.jar] più semplice per [!DNL TarMK] in quanto richiede un singolo comando [!DNL oak-run.jar], ma richiede la chiusura dell&#39;istanza AEM.

## Indicizzazione fuori banda di TarMK con oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* L&#39;indicizzazione fuori banda in [!DNL TarMK] con [!DNL oak-run.jar] riduce al minimo l&#39;impatto dell&#39;indicizzazione sulle istanze AEM in uso.
* L’indicizzazione fuori banda è l’approccio consigliato per le installazioni AEM in cui il tempo di reindicizzazione supera le finestre di manutenzione disponibili.

## Indicizzazione online di MongoMK con oak-run.jar

* L&#39;indice online con [!DNL oak-run.jar] in [!DNL MongoMK] e [!DNL RDBMK] è il metodo consigliato per la reindicizzazione di [!DNL MongoMK] (e [!DNL RDBMK]) installazioni AEM. **Nessun altro metodo deve essere utilizzato per [!DNL MongoMK] o [!DNL RDBMK].**
* Questa indicizzazione deve essere eseguita solo su una singola istanza AEM nel cluster.
* L&#39;indicizzazione online di [!DNL MongoMK] è sicura per un cluster AEM in esecuzione, poiché l&#39;attraversamento dell&#39;archivio si verifica solo su un singolo nodo [!DNL MongoDB], consentendo agli altri di continuare a servire le richieste senza un impatto significativo sulle prestazioni.

Il comando [!DNL oak-run.jar] per l&#39;indicizzazione in linea di [!DNL MongoMK] è [uguale all&#39;indicizzazione in linea di  [!DNL TarMK] con [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar), con la differenza che il parametro dell&#39;archivio segmenti punta all&#39;istanza di [!DNL MongoDB] che contiene l&#39;archivio nodi.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiali di supporto

* [Scarica [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Verificare che la versione scaricata corrisponda alla versione di Oak installata nell&#39;AEM come descritto in precedenza*
* [Documentazione comando indice Apache Jackrabbit Oak oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
