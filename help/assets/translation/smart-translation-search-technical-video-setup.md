---
title: Configurare la ricerca di traduzione avanzata con AEM Assets
description: La funzione Ricerca di traduzione intelligente consente di utilizzare termini di ricerca non inglesi per risolvere i contenuti in inglese. Per impostare AEM per Smart Translation Search, è necessario installare e configurare il bundle Apache Oak Search Machine Translation OSGi, nonché i pacchetti linguistici Apache Joshua gratuiti e open source che contengono le regole di traduzione.
version: 6.3, 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---


# Configurare la ricerca per la traduzione avanzata con AEM Assets{#set-up-smart-translation-search-with-aem-assets}

La funzione Ricerca di traduzione intelligente consente di utilizzare termini di ricerca non inglesi per risolvere i contenuti in inglese. Per impostare AEM per Smart Translation Search, è necessario installare e configurare il bundle Apache Oak Search Machine Translation OSGi, nonché i pacchetti linguistici Apache Joshua gratuiti e open source che contengono le regole di traduzione.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>La ricerca per la traduzione avanzata deve essere impostata su ogni istanza di AEM che la richiede.

1. Scarica e installa il bundle OSGi di Oak Search Machine Translation
   * [Scarica il ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) bundle OSGi di Oak Search Machine Translation che corrisponde alla versione Oak di AEM.
   * Installa il bundle OSGi scaricato Oak Search Machine Translation in AEM tramite [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Scarica e aggiorna i Language Pack di Apache Joshua
   * Scarica e decomprimi i [Language Pack Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) desiderati.
   * Modifica il file `joshua.config` e commenta le 2 righe che iniziano con:

      ```
      feature-function = LanguageModel ...
      ```

   * Determina e registra le dimensioni della cartella del modello del Language Pack, in quanto questo influenza lo spazio heap aggiuntivo richiesto da AEM.
   * Sposta la cartella del Language Pack Apache Joshua decompressa (con le `joshua.config` modifiche) in

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Esempio:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. Riavvia AEM con allocazione aggiornata della memoria heap
   * Interrompi AEM
   * Determinare la nuova dimensione heap richiesta per AEM

      * Dimensione heap pre-language-lack di AEM + la dimensione della directory del modello arrotondata al più vicino 2 GB
      * Ad esempio: Se l&#39;installazione di AEM richiede 8 GB di heap per essere eseguita e la cartella del modello del Language Pack non è compressa da 3,8 GB, la nuova dimensione dell&#39;heap è:

         L&#39;originale `8GB` + ( `3.75GB` arrotondato all&#39;originale `2GB`, che è `4GB`) per un totale di `12GB`
   * Verificare che il computer disponga di questa quantità di memoria disponibile aggiuntiva.
   * Aggiorna gli script di avvio di AEM per regolare la nuova dimensione dell&#39;heap

      * Esempio. `java -Xmx12g -jar cq-author-p4502.jar`
   * Riavvia AEM con la dimensione dell&#39;heap aumentata.

   >[!NOTE]
   >
   >Lo spazio heap necessario per i Language Pack può aumentare, specialmente quando vengono utilizzati più Language Pack.
   >
   >
   >Assicurati sempre che **l&#39;istanza abbia memoria sufficiente** per adattarsi agli aumenti dello spazio heap allocato.
   >
   >
   >L&#39; **heap di base deve sempre essere calcolato per supportare prestazioni accettabili senza l&#39;installazione di Language Pack**.

4. Registrare i Language Pack tramite Apache Jackrabbit Oak Machine Translation Configurazioni Full-text Query Termini Provider OSGi

   * Per ogni Language Pack, [crea una nuova configurazione Apache Jackrabbit Oak Machine Translation Full-text Query Termini Provider OSGi ](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) tramite il Configuration Manager di AEM Web Console.

      * `Joshua Config Path` è il percorso assoluto del file joshua.config. Il processo AEM deve essere in grado di leggere tutti i file nella cartella del Language Pack.
      * `Node types` sono i tipi di nodo candidati la cui ricerca full-text coinvolgerà questo Language Pack per la traduzione.
      * `Minimum score` è il punteggio minimo di affidabilità per un termine tradotto da utilizzare.

         * Per esempio, hombre (spagnolo per &quot;uomo&quot;) può tradurre alla parola inglese &quot;uomo&quot; con un punteggio di affidabilità di `0.9` e anche tradurre alla parola inglese &quot;umano&quot; con un punteggio di affidabilità `0.2`. Regolare il punteggio minimo su `0.3`, manterrebbe la traduzione &quot;hombre&quot; su &quot;uomo&quot;, ma scarta la traduzione &quot;hombre&quot; su &quot;umano&quot;, in quanto questo punteggio di traduzione di `0.2` è inferiore al punteggio minimo di `0.3`.

5. Eseguire una ricerca full-text sulle risorse
   * Poiché dam:Asset è il tipo di nodo che questo Language Pack viene registrato nuovamente, per convalidarlo è necessario cercare AEM Assets utilizzando la ricerca full-text.
   * Passa a AEM > Risorse e apri Omnisearch. Cercare un termine nella lingua di cui è stato installato il Language Pack.
   * Se necessario, regola il punteggio minimo nelle configurazioni OSGi per garantire la precisione dei risultati.

6. Aggiornamento dei Language Pack
   * I pacchetti linguistici Apache Joshua sono interamente mantenuti dal progetto Apache Joshua, e il loro aggiornamento o correzione è come la discrezione del progetto Apache Joshua.
   * Se un Language Pack viene aggiornato, per installare gli aggiornamenti in AEM, è necessario seguire i passaggi da 2 a 4 di cui sopra, regolando le dimensioni dell’heap in alto o in basso secondo necessità.

      * Tieni presente che quando scomprimi il Language Pack decompresso nella cartella crx-quickstart/opt , sposta qualsiasi cartella esistente del Language Pack prima di copiarlo nel nuovo.
   * Se AEM non richiede un riavvio, le configurazioni OSGi pertinenti Apache Jackrabbit Oak Machien Translation Fulltext Query Provider che riguardano i Language Pack aggiornati devono essere nuovamente salvate in modo che AEM elabori i file aggiornati.


## Aggiornamento dell&#39;indice damAssetLucene {#updating-damassetlucene-index}

Affinché [AEM Smart Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) sia interessato dalla traduzione avanzata di AEM, l’indice `/oak   :index  /damAssetLucene` di AEM deve essere aggiornato per contrassegnare i predictedTags (il nome di sistema per &quot;Smart Tags&quot;) come parte dell’indice Lucene aggregato della risorsa.

Alla voce `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`, assicurati che la configurazione sia la seguente:

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

* [Bundle OSGi di Apache Oak Search Machine Translation](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua Language Pack](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Tag avanzati AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Best practice per query e indicizzazione](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)