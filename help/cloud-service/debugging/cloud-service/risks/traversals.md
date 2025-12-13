---
title: Avvisi di attraversamento in AEM as a Cloud Service
description: Scopri come attenuare gli avvisi di attraversamento in AEM as a Cloud Service.
feature: Migration
role: Developer
level: Beginner
jira: KT-10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
duration: 155
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 5%

---

# Avvisi di attraversamento

>[!TIP]
>Aggiungi un segnalibro a questa pagina per riferimenti futuri.

_Cosa sono gli avvisi di attraversamento?_

Gli avvisi trasversali sono __aemerror__ istruzioni di registro che indicano che le query non vengono eseguite correttamente nel servizio di pubblicazione AEM. Gli avvisi di attraversamento si manifestano in genere in AEM in due modi:

1. __Query lente__ che non utilizzano indici, con conseguente lentezza di risposta.
1. __Query non riuscite__, che generano `RuntimeNodeTraversalException` e causano un&#39;esperienza non funzionante.

La deselezione degli avvisi di attraversamento rallenta le prestazioni di AEM e può causare un’esperienza non funzionante per gli utenti.

## Come risolvere gli avvisi di attraversamento

Gli avvisi di attraversamento attenuati possono essere affrontati utilizzando tre semplici passaggi: analizzare, regolare e verificare. Prima di identificare le regolazioni ottimali, è necessario eseguire diverse iterazioni di rettifica e verifica.

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
               <p class="is-size-6">Identifica e comprende quali query attraversano.</p>
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
               <p class="is-size-6">Aggiorna le query e gli indici per evitare attraversamenti delle query.</p>
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
               <a href="#verify" title="Verificare" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/3-verify.png" alt="Verificare">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Verificare le regolazioni eseguite</p>                       
               <p class="is-size-6">Verificare le modifiche apportate alle query e agli indici per rimuovere gli attraversamenti.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Verifica</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## &#x200B;1. Analizzare{#analyze}

Innanzitutto, identifica i servizi di pubblicazione di AEM che presentano avvisi trasversali. A tale scopo, da Cloud Manager [scarica i registri `aemerror` dei servizi di pubblicazione](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager){target="_blank"} da tutti gli ambienti (sviluppo, staging e produzione) per gli ultimi __tre giorni__.

![Scarica registri di AEM as a Cloud Service](./assets/traversals/download-logs.jpg)

Aprire i file di log e cercare la classe Java™ `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. Il registro contenente gli avvisi di attraversamento contiene una serie di istruzioni simili alle seguenti:

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

A seconda del contesto dell’esecuzione della query, le istruzioni di registro possono contenere informazioni utili sull’iniziatore della query:

+ URL della richiesta HTTP associato all’esecuzione della query

   + Esempio: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Sintassi delle query Oak

   + Esempio: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ Query XPath

   + Esempio: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ Codice che esegue la query

   + Esempio: `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__Le query non riuscite__ sono seguite da un&#39;istruzione `RuntimeNodeTraversalException` simile a:

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## &#x200B;2. Adeguare{#adjust}

Una volta individuate le query che generano l’infrazione e il relativo codice di richiamo, è necessario apportare le modifiche necessarie. È possibile effettuare due tipi di regolazioni per attenuare gli avvisi di attraversamento:

### Regolare la query

__Modificare la query__ per aggiungere nuove restrizioni alle query che risolvono le restrizioni degli indici esistenti. Se possibile, preferisci modificare la query anziché gli indici.

+ [Scopri come ottimizzare le prestazioni delle query](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target="_blank"}

### Regolare l’indice

__Modificare (o creare) un indice AEM__ in modo che le restrizioni delle query esistenti possano essere risolte in base agli aggiornamenti dell&#39;indice.

+ [Scopri come ottimizzare gli indici esistenti](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target="_blank"}
+ [Scopri come creare gli indici](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target="_blank"}

## &#x200B;3. Verifica{#verify}

È necessario verificare le regolazioni apportate alle query, agli indici o a entrambi per assicurarsi che attenuino gli avvisi trasversali.

![Spiega query](./assets/traversals/verify.gif)

Se vengono apportate solo [modifiche alla query](#adjust-the-query), la query può essere testata direttamente in AEM as a Cloud Service tramite la [query esplicativa](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=it#queries){target="_blank"} di Developer Console. Explain Query viene eseguito per il servizio AEM Author. Tuttavia, poiché le definizioni degli indici sono le stesse nei servizi Author e Publish, è sufficiente convalidare le query per il servizio AEM Author.

Se vengono apportate [modifiche all&#39;indice](#adjust-the-index), è necessario distribuire l&#39;indice in AEM as a Cloud Service. Con le regolazioni dell&#39;indice distribuite, è possibile utilizzare la [Explain Query](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=it#queries){target="_blank"} di Developer Console per eseguire e ottimizzare ulteriormente la query.

In ultima analisi, tutte le modifiche (query e codice) vengono salvate in Git e distribuite in AEM as a Cloud Service utilizzando Cloud Manager. Dopo la distribuzione, verificare i percorsi del codice associati agli avvisi di attraversamento originali e verificare che gli avvisi non vengano più visualizzati nel registro `aemerror`.

## Altre risorse

Consulta queste altre risorse utili per comprendere gli indici, la ricerca e gli avvisi di attraversamento di AEM.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5: ricerca e indicizzazione" tabindex="-1"><img class="is-bordered-r-small" src="../../../expert-resources/cloud-5/imgs/009-thumb.png" alt="Cloud 5: ricerca e indicizzazione"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5: ricerca e indicizzazione">Cloud 5: ricerca e indicizzazione</a></p>
               <p class="is-size-6">Il team di Cloud 5 mostra come esplorare i pro e i contro della ricerca e dell’indicizzazione su AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
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
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=it" title="Ricerca e indicizzazione dei contenuti" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="Ricerca e indicizzazione dei contenuti">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=it" title="Ricerca e indicizzazione dei contenuti">Documentazione su ricerca e indicizzazione dei contenuti</a></p>
               <p class="is-size-6">Scopri come creare e gestire gli indici in AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=it" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
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
               <p class="is-size-6">Scopri come convertire le definizioni degli indici AEM 6 Oak affinché siano compatibili con AEM as a Cloud Service e come mantenere gli indici in futuro.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
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
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentazione sulla definizione dell’indice" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="Documentazione sulla definizione dell’indice">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentazione sulla definizione dell’indice">Documentazione sull’indice Lucene</a></p>
               <p class="has-ellipsis is-size-6">Riferimento dell’indice Apache Oak Jackrabbit Lucene che documenta tutte le configurazioni dell’indice Lucene supportate.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>
