---
title: Guida all’implementazione della ricerca semplice
description: L’implementazione di Ricerca semplice è costituita dai materiali del laboratorio AEM Search Demystified del Summit 2017. Questa pagina contiene i materiali di questo laboratorio. Per una visita guidata del laboratorio, visualizzare la cartella di lavoro del laboratorio nella sezione Presentazione di questa pagina.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 138
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 2%

---

# Guida all’implementazione della ricerca semplice{#simple-search-implementation-guide}

L&#39;implementazione della ricerca semplice è costituita dai materiali di **Adobe Summit lab AEM Search Demystified**. Questa pagina contiene i materiali di questo laboratorio. Per una visita guidata del laboratorio, visualizzare la cartella di lavoro del laboratorio nella sezione Presentazione di questa pagina.

![Panoramica dell&#39;architettura di ricerca](assets/l4080/simple-search-application.png)

## Materiale della presentazione {#bookmarks}

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

*I collegamenti ai capitoli seguenti presuppongono che i [pacchetti iniziali](#initialpackages) siano installati in AEM Author all&#39;indirizzo`http://localhost:4502`*

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
* [Pacchetto di applicazioni per la ricerca semplice](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Pacchetti capitolo {#chapter-packages}

* [Soluzione capitolo 1](assets/l4080/l4080-chapter1.zip)
* [Capitolo 2 soluzione](assets/l4080/l4080-chapter2.zip)
* [Capitolo 3 soluzione](assets/l4080/l4080-chapter3.zip)
* [Capitolo 4 soluzione](assets/l4080/l4080-chapter4.zip)
* [Impostazione capitolo 5](assets/l4080/l4080-chapter5-setup.zip)
* [Soluzione capitolo 5](assets/l4080/l4080-chapter5-solution.zip)
* [Capitolo 6 soluzione](assets/l4080/l4080-chapter6.zip)
* [Capitolo 9 soluzione](assets/l4080/l4080-chapter9.zip)

## Materiali di riferimento {#reference-materials}

* [Archivio Github](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Esportatore modello Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API QueryBuilder](https://experienceleague.adobe.com/docs/?lang=it)
* [Plug-in di AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([Pagina documentazione](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Correzioni e follow-up {#corrections-and-follow-up}

Correzioni e chiarimenti delle discussioni di laboratorio e risposte alle domande di follow-up dei partecipanti.

1. **Come interrompere la reindicizzazione?**

   La reindicizzazione può essere interrotta tramite la voce MBean IndexStats disponibile tramite [Console web AEM > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Eseguire `abortAndPause()` per interrompere la reindicizzazione. L&#39;indice verrà bloccato per essere reindicizzato finché non verrà richiamato `resume()`.
      * L&#39;esecuzione di `resume()` riavvierà il processo di indicizzazione.
   * Documentazione: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **In che modo gli indici OAK supportano più tenant?**

   Oak supporta il posizionamento di indici all’interno della struttura del contenuto e questi indici indicizzeranno solo all’interno di tale sottostruttura. Ad esempio, è possibile creare **`/content/site-a/oak:index/cqPageLucene`** per indicizzare il contenuto solo in **`/content/site-a`.**

   Un approccio equivalente consiste nell&#39;utilizzare le proprietà **`includePaths`** e **`queryPaths`** in un indice in **`/oak:index`**. Ad esempio:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Le considerazioni relative a questo approccio sono le seguenti:

   * Le query DEVONO specificare una restrizione di percorso uguale all&#39;ambito del percorso di query dell&#39;indice oppure essere discendenti di.
   * Anche indici con ambito più ampio (ad esempio `/oak:index/cqPageLucene`) indicizzeranno i dati, con conseguente inserimento di duplicati e costo di utilizzo del disco.
   * Può essere necessaria una gestione duplicativa della configurazione (ad es. l’aggiunta degli stessi indexRules a più indici tenant se questi devono soddisfare gli stessi set di query)
   * Questo approccio è consigliato sul livello di pubblicazione di AEM per la ricerca personalizzata del sito, come in AEM Author, in genere le query vengono eseguite in cima alla struttura del contenuto per tenant diversi (ad esempio, tramite OmniSearch); definizioni di indice diverse possono causare comportamenti diversi solo in base alla restrizione del percorso.

3. **Dove si trova un elenco di tutti gli analizzatori disponibili?**

   Oak espone un set di elementi di configurazione dell’analizzatore forniti da lucene da utilizzare in AEM.

   * [Documentazione di Apache Oak Analyzers](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizer](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtri](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Come cercare pagine e Assets nella stessa query?**

   Una novità di AEM 6.3 è la possibilità di eseguire query per più tipi di nodo nella stessa query fornita. La seguente query QueryBuilder. Si noti che ogni &quot;sottoquery&quot; può risolvere nel proprio indice, quindi in questo esempio la sottoquery `cq:Page` viene risolta in `/oak:index/cqPageLucene` e la sottoquery `dam:Asset` in `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   restituisce il piano di query e query seguente:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Esplora query e risultati tramite [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) e [AEM Chrome Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **Come eseguire ricerche in più percorsi nella stessa query?**

   Una novità di AEM 6.3 è la possibilità di eseguire query su più percorsi nella stessa query fornita. La seguente query QueryBuilder. Tieni presente che ogni &quot;sottoquery&quot; può risolvere nel proprio indice.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   restituisce il piano di query e query seguente

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Esplora query e risultati tramite [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) e [AEM Chrome Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
