---
title: Avvisi di passaggio in AEM as a Cloud Service
description: Scopri come attenuare gli avvisi di attraversamento in AEM as a Cloud Service.
topics: Migration
feature: Migration
role: Architect, Developer
level: Beginner
kt: 10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
source-git-commit: d30641b3a9565dd3b7121dfdf6b6969a2b401e92
workflow-type: tm+mt
source-wordcount: '834'
ht-degree: 3%

---

# Avvisi al passaggio

>[!TIP]
>Aggiungi ai segnalibri questa pagina per riferimenti futuri.

_Cosa sono gli avvisi traversali?_

Le avvertenze di passaggio sono __aemerror__ sul servizio AEM Publish vengono eseguite istruzioni di registro che indicano l’esecuzione di query con prestazioni scadenti. Le avvertenze di passaggio si manifestano in genere in AEM in due modi:

1. __Query lente__ che non utilizzano indici e causano tempi di risposta lenti.
1. __Query non riuscite__, che lanciano un `RuntimeNodeTraversalException`, con conseguente interruzione dell’esperienza.

Consentire agli avvisi traversal di non essere controllati rallenta AEM prestazioni e può causare interruzioni delle esperienze per gli utenti.

## Come risolvere gli avvisi trasversali

La riduzione degli avvisi traversali può essere affrontata in tre semplici passaggi: analizzare, regolare e verificare. Ci si aspettano diverse iterazioni di regolazione e verifica prima di identificare le regolazioni ottimali.

<div class="columns is-multiline">

<!-- Analyze -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Analyze" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#analyze" title="Analizza" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/1-analyze.png" alt="Analizza">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Analizzare il problema</p>
               <p class="is-size-6">Identifica e capisce quali query stanno attraversando.</p>
               <a href="#analyze" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Analizza</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Adjust -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adjust" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#adjust" title="Regola" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/2-adjust.png" alt="Regola">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Regolare il codice o la configurazione</p>
               <p class="is-size-6">Aggiorna query e indici per evitare attraversamenti di query.</p>
               <a href="#adjust" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Regola</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Verify -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Verify" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#verify" title="Verifica" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/3-verify.png" alt="Verifica">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Verificare le regolazioni eseguite</p>                       
               <p class="is-size-6">Verifica le modifiche apportate alle query e agli indici rimuovendo i traversaggi.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Verifica</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1. Analizzare{#analyze}

In primo luogo, identifica quali servizi AEM Publish presentano avvisi trasversali. A questo scopo, da Cloud Manager, [scarica servizi di pubblicazione&quot; `aemerror` logs](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager){target=&quot;_blank&quot;} da tutti gli ambienti (sviluppo, stage e produzione) per il passato __tre giorni__.

![Scaricare AEM registri as a Cloud Service](./assets/traversals/download-logs.jpg)

Apri i file di registro e cerca la classe Java™ `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. Il registro contenente gli avvisi traversal contiene una serie di istruzioni simili a:

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

A seconda del contesto dell’esecuzione della query, le istruzioni di registro possono contenere informazioni utili sull’autore della query:

+ URL della richiesta HTTP associato all’esecuzione della query

   + Esempio: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Sintassi della query Oak

   + Esempio: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ Query XPath

   + Esempio: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ Codice che esegue la query

   + Esempio:  `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__Query non riuscite__ sono seguite da un `RuntimeNodeTraversalException` simile a:

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2. Regolare{#adjust}

Una volta scoperte le query offensive e il relativo codice di chiamata, è necessario apportare delle modifiche. Per attenuare gli avvisi di attraversamento è possibile effettuare due tipi di regolazioni:

### Regolare la query

__Modificare la query__ per aggiungere nuove restrizioni alla query che risolvono le restrizioni esistenti all&#39;indice. Se possibile, preferisci modificare la query alla modifica degli indici.

+ [Scopri come ottimizzare le prestazioni delle query](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;}

### Regolare l&#39;indice

__Modificare (o creare) un indice AEM__ in modo che le restrizioni esistenti alla query siano risolvibili negli aggiornamenti dell&#39;indice.

+ [Scopri come ottimizzare gli indici esistenti](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;}
+ [Scopri come creare indici](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target=&quot;_blank&quot;}

## 3. Verifica{#verify}

È necessario verificare le modifiche apportate alle query, agli indici o a entrambi per assicurarsi che attenuino gli avvisi di attraversamento.

![Spiega query](./assets/traversals/verify.gif)

Solo se [adeguamenti della query](#adjust-the-query) La query può essere testata direttamente su AEM as a Cloud Service tramite Developer Console [Spiega query](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;}. La funzione Query di spiegazione viene eseguita sul servizio Author di AEM, tuttavia, poiché le definizioni degli indici sono le stesse nei servizi Author e Publish, è sufficiente convalidare le query rispetto al servizio Author di AEM.

Se [aggiustamenti dell&#39;indice](#adjust-the-index) vengono effettuati, l&#39;indice deve essere distribuito AEM as a Cloud Service. Con le regolazioni dell&#39;indice implementate, la Console per sviluppatori [Spiega query](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries)È possibile utilizzare {target=&quot;_blank&quot;} per eseguire e ottimizzare ulteriormente la query.

In ultima analisi, tutte le modifiche (query e codice) sono salvate in Git e distribuite in AEM as a Cloud Service utilizzando Cloud Manager. Una volta distribuiti, verifica i percorsi del codice associati agli avvisi traversal originali e verifica che gli avvisi di attraversamento non siano più visualizzati nella pagina `aemerror` registro.

## Altre risorse

Consulta queste altre risorse utili per comprendere gli indici AEM, gli avvisi di ricerca e attraversamento.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - Ricerca e indicizzazione" tabindex="-1"><img class="is-bordered-r-small" src="../../../cloud-5/imgs/009-thumb.png" alt="Cloud 5 - Ricerca e indicizzazione"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - Ricerca e indicizzazione">Cloud 5 - Ricerca e indicizzazione</a></p>
               <p class="is-size-6">Il team di Cloud 5 mostra come esplora i dettagli e le uscite della ricerca e dell’indicizzazione su AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Content Search and Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Content Search and Indexing
" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="Ricerca e indicizzazione dei contenuti" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="Ricerca e indicizzazione dei contenuti">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="Ricerca e indicizzazione dei contenuti">Documentazione sulla ricerca e l’indicizzazione dei contenuti</a></p>
               <p class="is-size-6">Scopri come creare e gestire gli indici in AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Modernizing your Oak indexes -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modernizing your Oak indexes" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Modernizzazione degli indici Oak" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="Modernizzazione degli indici Oak">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Modernizzazione degli indici Oak">Modernizzazione degli indici Oak</a></p>
               <p class="is-size-6">Scopri come convertire le definizioni dell'indice Oak AEM 6 per renderle AEM compatibili con l'as a Cloud Service e come mantenere gli indici in corso.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Index definition documentation -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Index definition documentation" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentazione sulla definizione dell'indice" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="Documentazione sulla definizione dell'indice">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentazione sulla definizione dell'indice">Documentazione dell'indice Lucene</a></p>
               <p class="has-ellipsis is-size-6">L'indice Apache Oak Jackrabbit Lucene fa riferimento che documenta tutte le configurazioni dell'indice Lucene supportate.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>
