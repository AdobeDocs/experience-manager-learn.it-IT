---
title: Guida all’implementazione della ricerca semplice
description: L'implementazione semplice della ricerca sono i materiali del laboratorio Summit 2017 AEM Search Demystified. Questa pagina contiene i materiali di questo laboratorio. Per una visita guidata del laboratorio, vedere la cartella di lavoro Lab nella sezione Presentazione di questa pagina.
topics: development, search
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 1%

---


# Guida all&#39;implementazione della ricerca semplice{#simple-search-implementation-guide}

L&#39;implementazione della ricerca semplice è costituita dai materiali del **Adobe Summit lab AEM Search Demystified**. Questa pagina contiene i materiali di questo laboratorio. Per una visita guidata del laboratorio, vedere la cartella di lavoro Lab nella sezione Presentazione di questa pagina.

![Panoramica sull&#39;architettura di ricerca](assets/l4080/simple-search-application.png)

## Materiali di presentazione {#bookmarks}

* [Cartella di lavoro Lab](assets/l4080/l4080-lab-workbook.pdf)
* [Presentazione](assets/l4080/l4080-presentation.pdf)

## Segnalibri {#bookmarks-1}

### Strumenti {#tools}

* [Gestione indice](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Spiega query](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [Gestione pacchetti CRX](http://localhost:4502/crx/packmgr/index.jsp)
* [Debugger QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Generatore definizione indice Oak](https://oakutils.appspot.com/generate/index)

### Capitoli {#chapters}

*I collegamenti ai capitoli riportati di seguito presuppongono l&#39;installazione dei  [pacchetti ](#initialpackages) iniziali in AEM Author all&#39;indirizzo`http://localhost:4502`*

* [Capitolo 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [Capitolo 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [Capitolo 3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [Capitolo 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [Capitolo 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [Capitolo 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [Capitolo 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [Capitolo 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [Capitolo 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## Pacchetti {#packages}

### Pacchetti iniziali {#initial-packages}

* [Tag](assets/l4080/summit-tags.zip)
* [Pacchetto applicazione di ricerca semplice](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Pacchetti di capitoli {#chapter-packages}

* [Soluzione del capitolo 1](assets/l4080/l4080-chapter1.zip)
* [Soluzione del capitolo 2](assets/l4080/l4080-chapter2.zip)
* [Soluzione del capitolo 3](assets/l4080/l4080-chapter3.zip)
* [Capitolo 4: soluzione](assets/l4080/l4080-chapter4.zip)
* [Impostazione del capitolo 5](assets/l4080/l4080-chapter5-setup.zip)
* [Capitolo 5: soluzione](assets/l4080/l4080-chapter5-solution.zip)
* [Soluzione del capitolo 6](assets/l4080/l4080-chapter6.zip)
* [Capitolo 9: soluzione](assets/l4080/l4080-chapter9.zip)

## Materiali di riferimento {#reference-materials}

* [Repository di Github](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Esportatore modello Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API QueryBuilder](https://docs.adobe.com/docs/en/aem/6-2/develop/search/querybuilder-api.html)
* [AEM Chrome Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) (pagina[ ](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/)Documentazione)

## Correzioni e follow-up {#corrections-and-follow-up}

Correzioni e chiarimenti dalle discussioni di laboratorio e risposte alle domande di follow-up dei partecipanti.

1. **Come interrompere la reindicizzazione?**

   È possibile arrestare la reindicizzazione tramite l&#39;opzione IndexStats MBean disponibile tramite [AEM console Web > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Eseguire `abortAndPause()` per interrompere la reindicizzazione. In questo modo l&#39;indice verrà bloccato per un&#39;ulteriore reindicizzazione fino a quando non verrà richiamato `resume()`.
      * L&#39;esecuzione di `resume()` riavvia il processo di indicizzazione.
   * Documentazione: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Come possono gli indici di quercia supportare più locatari?**

   Oak supporta l&#39;inserimento di indici nella struttura del contenuto, che verranno indicizzati solo all&#39;interno della sottostruttura. Ad esempio, **`/content/site-a/oak:index/cqPageLucene`** potrebbe essere creato per indicizzare il contenuto solo sotto **`/content/site-a`.**

   Un approccio equivalente consiste nell&#39;utilizzare le proprietà **`includePaths`** e **`queryPaths`** su un indice in **`/oak:index`**. Esempio:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Le considerazioni di questo approccio sono:

   * Query DEVONO specificare una restrizione del percorso uguale all&#39;ambito del percorso di query dell&#39;indice, oppure essere un discendente.
   * Gli indici con ambito più ampio (ad esempio `/oak:index/cqPageLucene`) indicizzeranno anche i dati, con conseguente caricamento di duplicati e costo di utilizzo del disco.
   * Può essere necessaria una gestione di configurazione duplicata (ad esempio aggiunta delle stesse regole indexRules tra più indici tenant, se devono soddisfare gli stessi set di query)
   * Questo approccio è indicato nel livello AEM Publish per la ricerca di siti personalizzata. In AEM Author, è comune che le query vengano eseguite a un livello superiore rispetto alla struttura del contenuto per tenant diversi (ad esempio tramite OmniSearch). Diverse definizioni di indice possono dare luogo a comportamenti diversi in base solo alla limitazione del percorso.


3. **Dov&#39;è un elenco di tutti gli analizzatori disponibili?**

   Oak espone una serie di elementi di configurazione dell&#39;analizzatore di tipo lucene da usare in AEM.

   * [Documentazione di Apache Oak Analyzers](http://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Token](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtri](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Come cercare pagine e risorse nella stessa query?**

   La novità di AEM 6.3 è la capacità di eseguire query per più tipi di nodi nella stessa query fornita. La seguente query QueryBuilder. Ogni &quot;sub-query&quot; può essere risolta in base al proprio indice, pertanto in questo esempio la sottoquery `cq:Page` viene risolta in `/oak:index/cqPageLucene` e la sottoquery `dam:Asset` in `/oak:index/damAssetLucene` viene risolta in &lt;a3/>.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   restituisce il seguente piano di query e query:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Esplora la query e i risultati tramite [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) e [AEM Chrome Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **Come eseguire ricerche su più percorsi nella stessa query?**

   La novità di AEM 6.3 è la capacità di eseguire query su più percorsi nella stessa query fornita. La seguente query QueryBuilder. Si noti che ogni &quot;sub-query&quot; può raggiungere il proprio indice.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   restituisce il seguente piano di query e query

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Esplora la query e i risultati tramite [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) e [AEM Chrome Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
