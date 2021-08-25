---
title: Guida all’implementazione per semplici ricerche
description: L'implementazione Semplice di ricerca sono i materiali del laboratorio del Summit 2017 AEM Ricerca Demystified. Questa pagina contiene i materiali di questo laboratorio. Per una visita guidata del laboratorio, vedere la cartella di lavoro Lab nella sezione Presentazione di questa pagina.
version: 6.3, 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 2%

---


# Guida all’implementazione per semplici ricerche{#simple-search-implementation-guide}

L&#39;implementazione della ricerca semplice è costituita dai materiali del **laboratorio di Adobe Summit AEM Search Demystified**. Questa pagina contiene i materiali di questo laboratorio. Per una visita guidata del laboratorio, vedere la cartella di lavoro Lab nella sezione Presentazione di questa pagina.

![Panoramica dell’architettura di ricerca](assets/l4080/simple-search-application.png)

## Materiali di presentazione {#bookmarks}

* [Cartella di lavoro Lab](assets/l4080/l4080-lab-workbook.pdf)
* [Presentazione](assets/l4080/l4080-presentation.pdf)

## Segnalibri {#bookmarks-1}

### Strumenti {#tools}

* [Gestione indice](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Spiega query](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [Gestione pacchetti CRX](http://localhost:4502/crx/packmgr/index.jsp)
* [Debugger di QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Generatore di definizione dell&#39;indice Oak](https://oakutils.appspot.com/generate/index)

### Capitoli {#chapters}

*I collegamenti riportati di seguito presuppongono l’installazione dei  [pacchetti ](#initialpackages) iniziali su AEM Author all’indirizzo`http://localhost:4502`*

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

### Colli per capitolo {#chapter-packages}

* [Soluzione del capitolo 1](assets/l4080/l4080-chapter1.zip)
* [Soluzione del capitolo 2](assets/l4080/l4080-chapter2.zip)
* [Capitolo 3: soluzione](assets/l4080/l4080-chapter3.zip)
* [Capitolo 4 soluzione](assets/l4080/l4080-chapter4.zip)
* [Capitolo 5](assets/l4080/l4080-chapter5-setup.zip)
* [Capitolo 5 soluzione](assets/l4080/l4080-chapter5-solution.zip)
* [Capitolo 6 soluzione](assets/l4080/l4080-chapter6.zip)
* [Capitolo 9 soluzione](assets/l4080/l4080-chapter9.zip)

## Materiali di riferimento {#reference-materials}

* [Archivio Github](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Esportatore di modelli Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API di QueryBuilder](https://experienceleague.adobe.com/docs/)
* [Plug-in AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode)  (pagina [Documentazione](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Correzioni e follow-up {#corrections-and-follow-up}

Correzioni e chiarimenti dalle discussioni di laboratorio e risposte alle domande successive dei partecipanti.

1. **Come interrompere la reindicizzazione?**

   La reindicizzazione può essere arrestata tramite la MBean IndexStats disponibile tramite [AEM console Web > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Esegui `abortAndPause()` per interrompere la reindicizzazione. Questo blocca l&#39;indice per un&#39;ulteriore reindicizzazione fino a quando `resume()` non viene richiamato.
      * L&#39;esecuzione di `resume()` riavvierà il processo di indicizzazione.
   * Documentazione: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Come possono gli indici oak supportare più tenant?**

   Oak supporta il posizionamento di indici attraverso la struttura del contenuto, e questi indici si indicizzeranno solo all’interno di tale sottoalbero. Ad esempio, **`/content/site-a/oak:index/cqPageLucene`** può essere creato per indicizzare il contenuto solo sotto **`/content/site-a`.**

   Un approccio equivalente consiste nell&#39;utilizzare le proprietà **`includePaths`** e **`queryPaths`** su un indice in **`/oak:index`**. Esempio:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Le considerazioni di questo approccio sono le seguenti:

   * Le query DEVONO specificare una restrizione del percorso che sia uguale all&#39;ambito del percorso di query dell&#39;indice, o essere un discendente di.
   * Gli indici con ambito più ampio (ad esempio `/oak:index/cqPageLucene`) indicizzeranno anche i dati, con conseguente inserimento di duplicati e costo di utilizzo del disco.
   * Può essere necessaria la gestione della configurazione duplicativa (ad esempio, aggiungi le stesse regole di indice su più indici tenant se devono soddisfare gli stessi set di query)
   * Questo approccio è più adatto al livello di pubblicazione di AEM per la ricerca di siti personalizzati, come in AEM Author, è comune che le query vengano eseguite ad alto livello nella struttura dei contenuti per tenant diversi (ad esempio, tramite OmniSearch) - diverse definizioni di indice possono causare comportamenti diversi in base solo alla restrizione del percorso.


3. **Dove si trova un elenco di tutti gli analizzatori disponibili?**

   Oak espone un set di elementi di configurazione dell&#39;analizzatore lucene-fornisce da utilizzare in AEM.

   * [Documentazione di Apache Oak Analyzers](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Token](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtri](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Come cercare pagine e risorse nella stessa query?**

   La novità di AEM 6.3 è la capacità di eseguire query per più tipi di nodo nella stessa query fornita. La seguente query QueryBuilder. Tieni presente che ogni &quot;sottoquery&quot; può risolvere il proprio indice, pertanto in questo esempio la sottoquery `cq:Page` viene risolta in `/oak:index/cqPageLucene` e la sottoquery `dam:Asset` viene risolta in `/oak:index/damAssetLucene`.

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

5. **Come eseguire la ricerca su più percorsi nella stessa query?**

   La novità di AEM 6.3 è la capacità di eseguire query su più percorsi nella stessa query fornita. La seguente query QueryBuilder. Tieni presente che ogni &quot;sottoquery&quot; può risolvere al proprio indice.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   restituisce il seguente piano di query

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Esplora la query e i risultati tramite [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) e [AEM Chrome Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
