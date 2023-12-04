---
title: Configurare la ricerca di traduzione intelligente con AEM Assets
description: Smart Translation Search consente di utilizzare termini di ricerca non inglesi per risolvere contenuti in lingua inglese. Per configurare AEM per Smart Translation Search, è necessario installare e configurare il bundle OSGi Apache Oak Search Machine Translation, nonché i Language Pack Apache Joshua gratuiti e open source pertinenti che contengono le regole di traduzione.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 664
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# Configurare la ricerca di traduzione intelligente con AEM Assets{#set-up-smart-translation-search-with-aem-assets}

Smart Translation Search consente di utilizzare termini di ricerca non inglesi per risolvere contenuti in lingua inglese. Per configurare AEM per Smart Translation Search, è necessario installare e configurare il bundle OSGi Apache Oak Search Machine Translation, nonché i Language Pack Apache Joshua gratuiti e open source pertinenti che contengono le regole di traduzione.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>La funzione Ricerca avanzata traduzione deve essere impostata su ogni istanza AEM che la richiede.

1. Scarica e installa il bundle OSGi Oak Search Machine Translation
   * [Scarica il bundle OSGi Oak Search Machine Translation](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) che corrisponde alla versione AEM Oak.
   * Installa il bundle OSGi Oak Search Machine Translation scaricato in AEM tramite [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Scarica e aggiorna i Language Pack di Apache Joshua
   * Scarica e decomprimi il file desiderato [Language Pack Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Modifica il `joshua.config` filtra e commenta le 2 righe che iniziano con:

     ```
     feature-function = LanguageModel ...
     ```

   * Determina e registra le dimensioni della cartella del modello del Language Pack, in quanto influiscono sulla quantità di spazio heap aggiuntivo che l’AEM richiederà.
   * Sposta la cartella del Language Pack Apache Joshua decompresso (con `joshua.config` modifiche) per

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     Ad esempio:

     ```
      .../crx-quickstart/opt/es-en
     ```

3. Riavvia AEM con allocazione della memoria heap aggiornata
   * Interrompere l’AEM
   * Determinare la nuova dimensione heap richiesta per l’AEM

      * Memoria heap priva di pre-lingua AEM + dimensione della directory dei modelli arrotondata per eccesso ai 2 GB più vicini
      * Ad esempio: se i pacchetti pre-lingua per l’installazione dell’AEM richiedono 8 GB di heap e la cartella dei modelli del Language Pack è 3,8 GB non compressa, la nuova dimensione heap è:

        L&#39;originale `8GB` + ( `3.75GB` arrotondato per eccesso al più vicino `2GB`, che è `4GB`) per un totale di `12GB`

   * Verificare che il computer disponga di questa quantità di memoria aggiuntiva.
   * Aggiornare gli script di avvio dell’AEM per adattarli alla nuova dimensione heap

      * Esempio: `java -Xmx12g -jar cq-author-p4502.jar`

   * Riavvia AEM con la dimensione heap aumentata.

   >[!NOTE]
   >
   >Lo spazio heap richiesto per i Language Pack può aumentare, soprattutto quando vengono utilizzati più Language Pack.
   >
   >
   >Assicurati sempre di **l’istanza dispone di memoria sufficiente** per adattarsi agli aumenti dello spazio heap allocato.
   >
   >
   >Il **l’heap di base deve sempre essere calcolato per supportare prestazioni accettabili senza Language Pack** installato.

4. Registra i Language Pack tramite le configurazioni OSGi del provider di termini di query full-text di Apache Jackrabbit Oak Machine Translation

   * Per ogni Language Pack, [crea una nuova configurazione OSGi provider di termini di query full-text Apache Jackrabbit Oak Machine Translation](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) tramite Gestione configurazione della console web AEM.

      * `Joshua Config Path` percorso assoluto del file joshua.config. Il processo AEM deve essere in grado di leggere tutti i file nella cartella del Language Pack.
      * `Node types` sono i tipi di nodo candidati la cui ricerca full-text coinvolgerà questo Language Pack per la traduzione.
      * `Minimum score` è il punteggio di affidabilità minimo per un termine tradotto da utilizzare.

         * Ad esempio, hombre (in spagnolo significa &quot;uomo&quot;) può tradurre la parola inglese &quot;uomo&quot; con un punteggio di affidabilità di `0.9` e tradurre anche in inglese la parola &quot;human&quot; con un punteggio di affidabilità `0.2`. Sintonizzazione del punteggio minimo su `0.3`, manterrebbe la traduzione &quot;hombre&quot; in &quot;man&quot;, ma scarterebbe la traduzione &quot;hombre&quot; in &quot;human&quot; come punteggio di traduzione di `0.2` è minore del punteggio minimo di `0.3`.

5. Eseguire una ricerca full-text delle risorse
   * Poiché dam:Asset è il tipo di nodo che questo Language Pack è nuovamente registrato, per convalidarlo è necessario eseguire una ricerca full-text in AEM Assets.
   * Passa a AEM > Risorse e apri Omnisearch. Cercare un termine nella lingua in cui è stato installato il Language Pack.
   * Se necessario, ottimizza il Punteggio minimo nelle configurazioni OSGi per garantire la precisione dei risultati.

6. Aggiornamento dei Language Pack
   * I Language Pack Apache Joshua sono interamente gestiti dal progetto Apache Joshua e il loro aggiornamento o correzione è a discrezione del progetto Apache Joshua.
   * Se un Language Pack viene aggiornato, per installare gli aggiornamenti in AEM è necessario seguire i passaggi da 2 a 4 precedenti, aumentando o riducendo la dimensione dell’heap in base alle esigenze.

      * Tieni presente che quando sposti il Language Pack decompresso nella cartella crx-quickstart/opt, sposta eventuali cartelle di Language Pack esistenti prima di copiarle nella nuova.

   * Se l’AEM non richiede un riavvio, le configurazioni OSGi del provider di termini di query full-text Apache Jackrabbit Oak Machien Translation relative ai Language Pack aggiornati devono essere salvate nuovamente in modo che l’AEM elabori i file aggiornati.

## Aggiornamento dell’indice damAssetLucene {#updating-damassetlucene-index}

Per ottenere [Tag avanzati AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) essere influenzato da AEM Smart Translation, AEM `/oak   :index  /damAssetLucene` L’indice deve essere aggiornato per contrassegnare i predictedTags (il nome di sistema per &quot;Tag avanzati&quot;) come parte dell’indice Lucene aggregato della risorsa.

Sotto `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`, verifica che la configurazione sia la seguente:

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## Risorse aggiuntive{#additional-resources}

* [Pacchetto OSGi per traduzione automatica Apache Oak Search](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Language Pack di Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Tag avanzati AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Best practice per query e indicizzazione](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
