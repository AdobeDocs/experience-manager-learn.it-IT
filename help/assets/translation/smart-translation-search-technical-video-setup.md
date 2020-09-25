---
title: Configurare la ricerca per la traduzione intelligente con  AEM Assets
seo-title: Configurare la ricerca per la traduzione intelligente con  AEM Assets
description: Smart Translation Search consente l'utilizzo di termini di ricerca non inglesi per la risoluzione al contenuto inglese. Per impostare AEM per Smart Translation Search, il bundle Apache Oak Search Machine Translation OSGi deve essere installato e configurato, così come i relativi pacchetti linguistici Apache Joshua gratuiti e open source che contengono le regole di traduzione.
seo-description: Smart Translation Search consente l'utilizzo di termini di ricerca non inglesi per la risoluzione al contenuto inglese. Per impostare AEM per Smart Translation Search, il bundle Apache Oak Search Machine Translation OSGi deve essere installato e configurato, così come i relativi pacchetti linguistici Apache Joshua gratuiti e open source che contengono le regole di traduzione.
uuid: b0e8dab2-6bc4-4158-91a1-4b9811359798
discoiquuid: 4db1b4db-74f4-4646-b5de-cb891612cc90
topics: authoring, search, metadata, localization
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '929'
ht-degree: 0%

---


# Configurare la ricerca per la traduzione intelligente con  AEM Assets{#set-up-smart-translation-search-with-aem-assets}

Smart Translation Search consente l&#39;utilizzo di termini di ricerca non inglesi per la risoluzione al contenuto inglese. Per impostare AEM per Smart Translation Search, il bundle Apache Oak Search Machine Translation OSGi deve essere installato e configurato, così come i relativi pacchetti linguistici Apache Joshua gratuiti e open source che contengono le regole di traduzione.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>È necessario impostare la ricerca per le traduzioni intelligenti su ogni istanza AEM che lo richiede.

1. Scaricare e installare il bundle Oak Search Machine Translation OSGi
   * [Scaricate il bundle](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) Oak Search Machine Translation OSGi che corrisponde AEM versione Oak.
   * Installate il bundle Oak Search Machine Translation OSGi scaricato in AEM tramite [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Download e aggiornamento dei pacchetti linguistici Apache Joshua
   * Scaricate e decomprimete i Language Pack [Apache Joshua desiderati](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Modificare il `joshua.config` file e commentare le 2 righe che iniziano con:

      ```
      feature-function = LanguageModel ...
      ```

   * Determinare e registrare la dimensione della cartella del modello del Language Pack, in quanto questo influenza la quantità di spazio heap supplementare AEM richiederà.
   * Spostate la cartella del Language Pack Apache Joshua decompressa (con le `joshua.config` modifiche) in

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

      * AEM dimensione dell&#39;heap pre-lingua-mancanza + la dimensione della directory del modello arrotondata al più vicino 2 GB
      * Ad esempio: Se l&#39;installazione dei pacchetti pre-lingua richiede 8 GB di heap per essere eseguita e la cartella del modello del Language Pack non è compressa da 3,8 GB, la nuova dimensione dell&#39;heap è:

         L&#39;originale `8GB` + ( `3.75GB` arrotondato per eccesso al più vicino `2GB`, che è `4GB`) per un totale di `12GB`
   * Verificare che la macchina disponga di questa quantità di memoria supplementare disponibile.
   * Aggiornare AEM script di avvio per regolare la nuova dimensione dell&#39;heap

      * Esempio. `java -Xmx12g -jar cq-author-p4502.jar`
   * Riavviate AEM con la dimensione heap aumentata.

   >[!NOTE]
   >
   >Lo spazio richiesto per i Language Pack può aumentare notevolmente, soprattutto se vengono utilizzati più Language Pack.
   >
   >
   >Assicurarsi sempre che **l&#39;istanza disponga di memoria** sufficiente per contenere gli incrementi nello spazio heap allocato.
   >
   >
   >L&#39;heap di **base deve essere sempre calcolato per supportare prestazioni accettabili senza l&#39;installazione di Language Pack** .

4. Registrare i Language Pack tramite Apache Jackrabbit Oak Machine Translation Completo di testo Query Termini Fornitori OSGi configurazioni

   * Per ciascun Language Pack, [create una nuova configurazione](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi tramite il gestore di configurazione della console Web AEM.

      * `Joshua Config Path` è il percorso assoluto del file joshua.config. Il processo di AEM deve essere in grado di leggere tutti i file presenti nella cartella del Language Pack.
      * `Node types` sono i tipi di nodo candidati per i quali la ricerca full-text coinvolgerà questo Language Pack per la traduzione.
      * `Minimum score` è il punteggio minimo di confidenza per un termine convertito da utilizzare.

         * Per esempio, hombre (spagnolo per &quot;uomo&quot;) può tradurre alla parola inglese &quot;uomo&quot; con un punteggio di confidenza di `0.9` e anche tradurre alla parola inglese &quot;umano&quot; con un punteggio di confidenza `0.2`. Sintonizzare il punteggio minimo a `0.3`, manterrebbe la traduzione &quot;hombre&quot; a &quot;uomo&quot;, ma scartare la traduzione &quot;hombre&quot; a &quot;umano&quot; in quanto questo punteggio di traduzione di `0.2` è inferiore al punteggio minimo di `0.3`.

5. Eseguire una ricerca full-text rispetto alle risorse
   * Poiché dam:Asset è il tipo di nodo che questo Language Pack è registrato di nuovo, è necessario cercare  AEM Assets utilizzando la ricerca full-text per convalidarlo.
   * Andate a AEM > Risorse e aprite Omnisearch. Cercare un termine nella lingua di cui è stato installato il Language Pack.
   * Se necessario, regolate il punteggio minimo nelle configurazioni OSGi per garantire la precisione dei risultati.

6. Aggiornamento dei Language Pack
   * I pacchetti linguistici Apache Joshua sono completamente mantenuti dal progetto Apache Joshua, e il loro aggiornamento o la loro correzione è come la discrezione del progetto Apache Joshua.
   * Se un Language Pack viene aggiornato, per installare gli aggiornamenti in AEM, devono essere seguiti i passaggi da 2 a 4 indicati sopra, regolando la dimensione dell&#39;heap in alto o in basso secondo necessità.

      * Quando si sposta il Language Pack decompresso nella cartella crx-quickstart/opt, spostare qualsiasi cartella esistente del Language Pack prima di copiarlo nel nuovo file.
   * Se AEM non richiede un riavvio, le configurazioni Apache Jackrabbit Oak Machien Translation FullText Query Termini provider OSGi relative ai Language Pack aggiornati devono essere salvate nuovamente in modo AEM elaborare i file aggiornati.


## Aggiornamento dell&#39;indice damAssetLucene {#updating-damassetlucene-index}

Affinché [AEM Smart Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) possa essere influenzato da AEM Smart Translation, AEM `/oak   :index  /damAssetLucene` indice deve essere aggiornato per contrassegnare i predettiTags (il nome di sistema per &quot;Smart Tags&quot;) come parte dell&#39;indice Lucene aggregato della risorsa.

Nella sezione `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`verificare che la configurazione sia la seguente:

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

* [Apache Oak Search Machine Translation OSGi bundle](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Pacchetti linguistici Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM smart tag](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Best practice per query e indicizzazione](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)