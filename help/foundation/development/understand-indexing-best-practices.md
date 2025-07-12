---
title: Best practice per l’indicizzazione in AEM
description: Scopri le best practice di indicizzazione in AEM.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
sub-product: Experience Manager, Experience Manager Sites
feature: Search
doc-type: Article
topic: Development
role: Developer, Architect
level: Beginner
duration: 373
last-substantial-update: 2024-01-04T00:00:00Z
jira: KT-14745
thumbnail: KT-14745.jpeg
exl-id: 3fd4c404-18e9-44e5-958f-15235a3091d5
source-git-commit: 7ada3c2e7deb414b924077a5d2988db16f28712c
workflow-type: tm+mt
source-wordcount: '1693'
ht-degree: 1%

---

# Best practice per l’indicizzazione in AEM

Scopri le best practice di indicizzazione in Adobe Experience Manager (AEM). Apache [Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/query/query.html) attiva la ricerca dei contenuti in AEM e i punti chiave sono i seguenti:

- Per supportare le funzionalità di ricerca e query, AEM fornisce diversi indici predefiniti, ad esempio `damAssetLucene`, `cqPageLucene` e altro ancora.
- Tutte le definizioni degli indici sono archiviate nell&#39;archivio nel nodo `/oak:index`.
- AEM as a Cloud Service supporta solo gli indici Oak Lucene.
- La configurazione dell’indice deve essere gestita nella base di codice del progetto AEM e distribuita utilizzando le pipeline CI/CD di Cloud Manager.
- Se per una determinata query sono disponibili più indici, viene utilizzato l&#39;**indice con il costo stimato più basso**.
- Se non è disponibile alcun indice per una determinata query, la struttura del contenuto viene attraversata per trovare il contenuto corrispondente. Tuttavia, il limite predefinito tramite `org.apache.jackrabbit.oak.query.QueryEngineSettingsService` è quello di attraversare solo 100.000 nodi.
- I risultati di una query sono **filtrati almeno** per garantire che l&#39;utente corrente disponga dell&#39;accesso in lettura. Ciò significa che i risultati della query potrebbero essere inferiori al numero di nodi indicizzati.
- La reindicizzazione dell’archivio dopo le modifiche della definizione dell’indice richiede tempo e dipende dalle dimensioni dell’archivio.

Per disporre di una funzionalità di ricerca efficiente e corretta che non influisca sulle prestazioni dell’istanza di AEM, è importante comprendere le best practice per l’indicizzazione.

## Indice personalizzato e OOTB

A volte, è necessario creare indici personalizzati per supportare i requisiti di ricerca. Tuttavia, segui le linee guida riportate di seguito prima di creare indici personalizzati:

- Comprendi i requisiti di ricerca e verifica se gli indici OOTB possono supportare i requisiti di ricerca. Utilizza **Strumento Prestazioni query**, disponibile in [SDK locale](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) e AEMCS tramite Developer Console o `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell`.

- Definisci una query ottimale, utilizza il [diagramma di flusso dell&#39;ottimizzazione delle query](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices) e [Scheda di riferimento rapido per le query JCR](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=it) per riferimento.

- Se gli indici OOTB non supportano i requisiti di ricerca, sono disponibili due opzioni. Tuttavia, consulta i [suggerimenti per la creazione di indici efficienti](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing)
   - Personalizza l’indice OOTB: opzione preferita in quanto è facile da mantenere e aggiornare.
   - Indice completamente personalizzato: solo se l’opzione precedente non funziona.

### Personalizzare l’indice OOTB

- In **AEMCS**, per personalizzare l&#39;indice OOTB, utilizzare la convenzione di denominazione **\&lt;NomeIndiceOOTBI>-\&lt;VersioneProdotto>-custom-\&lt;VersionePersonalizzata>**. Ad esempio, `cqPageLucene-custom-1` o `damAssetLucene-8-custom-1`. Questo consente di unire la definizione dell’indice personalizzato ogni volta che l’indice OOTB viene aggiornato. Per ulteriori dettagli, vedi [Modifiche agli indici predefiniti](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/operations/indexing).

- In **AEM 6.X**, la denominazione _indicata sopra non funziona_, ma è sufficiente aggiornare l&#39;indice OOTB con le proprietà necessarie nel nodo `indexRules`.

- Copia sempre la definizione più recente dell’indice OOTB dall’istanza di AEM utilizzando Gestione pacchetti di CRX DE (/crx/packmgr/), rinominala e aggiungi personalizzazioni all’interno del file XML.

- Archivia la definizione dell&#39;indice nel progetto AEM in `ui.apps/src/main/content/jcr_root/_oak_index` e distribuiscila con le pipeline CI/CD di Cloud Manager. Per ulteriori dettagli, vedere [Distribuzione delle definizioni di indice personalizzato](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/operations/indexing).

### Indice completamente personalizzato

La creazione di un indice completamente personalizzato deve essere l’ultima opzione disponibile e solo se quella precedente non funziona.

- Per creare un indice completamente personalizzato, utilizzare **\&lt;prefisso>.Convenzione di denominazione \&lt;customIndexName>-\&lt;version>-custom-\&lt;customVersion>**. Ad esempio, `wknd.adventures-1-custom-1`. Questo consente di evitare conflitti di denominazione. `wknd` è il prefisso e `adventures` è il nome di indice personalizzato. Questa convenzione è applicabile sia per AEM 6.X che per AEMCS e aiuta a preparare la futura migrazione ad AEMCS.

- AEMCS supporta solo gli indici Lucene; pertanto, per prepararsi alla futura migrazione ad AEMCS, utilizza sempre gli indici Lucene. Per ulteriori dettagli, vedi [Indici Lucene vs Indici proprietà](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing).

- Evita di creare un indice personalizzato sullo stesso tipo di nodo dell’indice OOTB. Personalizzare invece l&#39;indice OOTB con le proprietà necessarie nel nodo `indexRules`. Ad esempio, non creare un indice personalizzato sul tipo di nodo `dam:Asset`, ma personalizzare l&#39;indice `damAssetLucene` OOTB. _È stata una causa comune di problemi funzionali e di prestazioni_.

- Evitare inoltre di aggiungere più tipi di nodo, ad esempio `cq:Page` e `cq:Tag`, nel nodo delle regole di indicizzazione (`indexRules`). È invece possibile creare indici separati per ogni tipo di nodo.

- Come indicato nella sezione precedente, archivia la definizione dell’indice nel progetto AEM in `ui.apps/src/main/content/jcr_root/_oak_index` e distribuiscila con le pipeline CI/CD di Cloud Manager. Per ulteriori dettagli, vedere [Distribuzione delle definizioni di indice personalizzato](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/operations/indexing).

- Le linee guida per la definizione dell’indice sono:
   - Il tipo di nodo (`jcr:primaryType`) deve essere `oak:QueryIndexDefinition`
   - Il tipo di indice (`type`) deve essere `lucene`
   - La proprietà asincrona (`async`) deve essere `async,nrt`
   - Usa `includedPaths` ed evita la proprietà `excludedPaths`. Imposta sempre il valore `queryPaths` sullo stesso valore del valore `includedPaths`.
   - Per applicare la restrizione del percorso, utilizzare la proprietà `evaluatePathRestrictions` e impostarla su `true`.
   - Utilizzare la proprietà `tags` per assegnare tag all&#39;indice e durante la query specificare il valore dei tag per utilizzare l&#39;indice. La sintassi generale della query è `<query> option(index tag <tagName>)`.

  ```xml
  /oak:index/wknd.adventures-1-custom-1
      - jcr:primaryType = "oak:QueryIndexDefinition"
      - type = "lucene"
      - compatVersion = 2
      - async = ["async", "nrt"]
      - includedPaths = ["/content/wknd"]
      - queryPaths = ["/content/wknd"]
      - evaluatePathRestrictions = true
      - tags = ["customAdvSearch"]
  ...
  ```

### Esempi

Per comprendere le best practice, esaminiamo alcuni esempi.

#### Utilizzo non corretto della proprietà tags

Nell&#39;immagine seguente viene illustrata la definizione dell&#39;indice personalizzato e OOTB, evidenziando la proprietà `tags`. Entrambi gli indici utilizzano lo stesso valore `visualSimilaritySearch`.

![Utilizzo non corretto della proprietà tag](./assets/understand-indexing-best-practices/incorrect-tags-property.png)

##### Analisi

Utilizzo non corretto della proprietà `tags` nell&#39;indice personalizzato. Il motore di query di Oak seleziona l’indice personalizzato rispetto alla causa dell’indice OOTB che rappresenta il costo stimato più basso.

Il modo corretto è personalizzare l&#39;indice OOTB e aggiungere le proprietà necessarie nel nodo `indexRules`. Per ulteriori dettagli, vedere [Personalizzazione dell&#39;indice OOTB](#customize-the-ootb-index).

#### Indice sul tipo di nodo `dam:Asset`

Nell&#39;immagine seguente viene illustrato l&#39;indice personalizzato per il tipo di nodo `dam:Asset` con la proprietà `includedPaths` impostata su un percorso specifico.

![Indice nel tipo di nodo dam:Asset](./assets/understand-indexing-best-practices/index-for-damAsset-type.png)

##### Analisi

Se esegui omnisearch su Assets, restituisce risultati non corretti in quanto l’indice personalizzato presenta un costo stimato inferiore.

Non creare un indice personalizzato sul tipo di nodo `dam:Asset`, ma personalizzare l&#39;indice `damAssetLucene` OOTB con le proprietà necessarie nel nodo `indexRules`.

#### Più tipi di nodo nelle regole di indicizzazione

L&#39;immagine seguente mostra l&#39;indice personalizzato con più tipi di nodo sotto il nodo `indexRules`.

![Più tipi di nodo nelle regole di indicizzazione](./assets/understand-indexing-best-practices/multiple-nodetypes-in-index.png)

##### Analisi

Non è consigliabile aggiungere più tipi di nodo in un singolo indice. Tuttavia, è possibile indicizzare tipi di nodo nello stesso indice se i tipi di nodo sono strettamente correlati, ad esempio `cq:Page` e `cq:PageContent`.

Una soluzione valida consiste nel personalizzare l&#39;indice OOTB `cqPageLucene` e `damAssetLucene` e aggiungere le proprietà necessarie nel nodo `indexRules` esistente.

#### Assenza della proprietà `queryPaths`

L&#39;immagine seguente mostra l&#39;indice personalizzato (non conforme anche alla convenzione di denominazione) senza la proprietà `queryPaths`.

![Assenza della proprietà queryPaths](./assets/understand-indexing-best-practices/absense-of-queryPaths-prop.png)

##### Analisi

Imposta sempre il valore `queryPaths` sullo stesso valore del valore `includedPaths`. Inoltre, per applicare la restrizione del percorso, impostare la proprietà `evaluatePathRestrictions` su `true`.

#### Query con tag di indice

Nell&#39;immagine seguente viene illustrato l&#39;indice personalizzato con la proprietà `tags` e come utilizzarlo durante l&#39;esecuzione di una query.

![Query con tag indice](./assets/understand-indexing-best-practices/tags-prop-usage.png)

```
/jcr:root/content/dam//element(*,dam:Asset)[(jcr:content/@contentFragment = 'true' and jcr:contains(., '/content/sitebuilder/test/mysite/live/ja-jp/mypage'))]order by @jcr:created descending option (index tag assetPrefixNodeNameSearch)
```

##### Analisi

Viene illustrato come impostare il valore della proprietà `tags` non in conflitto e corretto nell&#39;indice e utilizzarlo durante l&#39;esecuzione di query. La sintassi generale della query è `<query> option(index tag <tagName>)`. Vedi anche [Tag indice opzione query](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#query-option-index-tag)

#### Indice personalizzato

L&#39;immagine seguente mostra l&#39;indice personalizzato con il nodo `suggestion` per ottenere la funzionalità di ricerca avanzata.

![Indice personalizzato](./assets/understand-indexing-best-practices/custom-index-with-suggestion-node.png)

##### Analisi

È un caso d&#39;uso valido creare un indice personalizzato per la funzionalità di [ricerca avanzata](https://jackrabbit.apache.org/oak/docs/query/lucene.html#advanced-search-features). Tuttavia, il nome dell&#39;indice deve seguire **\&lt;prefisso>.Convenzione di denominazione \&lt;customIndexName>-\&lt;version>-custom-\&lt;customVersion>**.

## Ottimizzazione dell’indice disabilitando Apache Tika

AEM utilizza [Apache Tika](https://tika.apache.org/) per _estrarre metadati e contenuto di testo da file_ di tipo PDF, Word, Excel e altro. Il contenuto estratto viene memorizzato nell’archivio e indicizzato dall’indice Oak Lucene.

A volte gli utenti non hanno bisogno della possibilità di cercare all’interno del contenuto di un file o di una risorsa; in questi casi, puoi migliorare le prestazioni di indicizzazione disabilitando Apache Tika. I vantaggi sono i seguenti:

- Indicizzazione più rapida
- Riduzione delle dimensioni dell’indice
- Minore utilizzo dell&#39;hardware

>[!CAUTION]
>
>Prima di disabilitare Apache Tika, accertati che i requisiti di ricerca non richiedano la possibilità di eseguire ricerche all’interno del contenuto di una risorsa.


### Disattiva per tipo MIME

Per disabilitare Apache Tika per tipo mime, procedi come segue:

- Aggiungere il nodo `tika` di tipo `nt:unstructured` nella definizione dell&#39;indice OOBT o personalizzato. Nell&#39;esempio seguente, il tipo MIME PDF è disabilitato per l&#39;indice OOTB `damAssetLucene`.

```xml
/oak:index/damAssetLucene
    - jcr:primaryType = "oak:QueryIndexDefinition"
    - type = "lucene"
    ...
    <tika jcr:primaryType="nt:unstructured">
        <config.xml/>
    </tika>
```

- Aggiungi `config.xml` con i dettagli seguenti sotto il nodo `tika`.

```xml
<properties>
  <parsers>
    <parser class="org.apache.tika.parser.EmptyParser">
      <mime>application/pdf</mime>
      <!-- Add more mime types to disable -->
  </parsers>
</properties>
```

- Per aggiornare l&#39;indice archiviato, impostare la proprietà `refresh` su `true` nel nodo di definizione dell&#39;indice. Per ulteriori dettagli, vedere [Proprietà definizione indice](https://jackrabbit.apache.org/oak/docs/query/lucene.html#index-definition:~:text=Defaults%20to%2010000-,refresh,-Optional%20boolean%20property).

L&#39;immagine seguente mostra l&#39;indice OOTB `damAssetLucene` con il nodo `tika` e il file `config.xml` che disabilita PDF e altri tipi MIME.

![Indice damAssetLucene OOTB con nodo tika](./assets/understand-indexing-best-practices/ootb-index-with-tika-node.png)

### Disattiva completamente

Per disabilitare completamente Apache Tika, procedi come segue:

- Aggiungere la proprietà `includePropertyTypes` in `/oak:index/<INDEX-NAME>/indexRules/<NODE-TYPE>` e impostare il valore su `String`. Nell&#39;immagine seguente, ad esempio, viene aggiunta la proprietà `includePropertyTypes` per il tipo di nodo `dam:Asset` dell&#39;indice OOBT `damAssetLucene`.

![Proprietà IncludePropertyTypes](./assets/understand-indexing-best-practices/includePropertyTypes-prop.png)

- Aggiungi `data` con le proprietà di seguito sotto il nodo `properties`, assicurati che sia il primo nodo sopra la definizione della proprietà. Per esempio, vedi l’immagine seguente:

```xml
/oak:index/<INDEX-NAME>/indexRules/<NODE-TYPE>/properties/data
    - jcr:primaryType = "nt:unstructured"
    - type = "String"
    - name = "jcr:data"
    - nodeScopeIndex = false
    - propertyIndex = false
    - analyze = false
```

![Proprietà dati](./assets/understand-indexing-best-practices/data-prop.png)

- Reindicizzare la definizione dell&#39;indice aggiornato impostando la proprietà `reindex` su `true` nel nodo di definizione dell&#39;indice.

## Strumenti utili

Esaminiamo alcuni strumenti che possono aiutarti a definire, analizzare e ottimizzare gli indici.

### Strumento di creazione dell’indice

Lo strumento [Oak Index Definition Generator](https://oakutils.appspot.com/generate/index) consente a **di generare la definizione dell&#39;indice** in base alle query di input. È un buon punto di partenza per creare un indice personalizzato.

### Strumento Analizza indice

Lo strumento [Index Definition Analyzer](https://oakutils.appspot.com/analyze/index) consente a **di analizzare la definizione dell&#39;indice** e fornisce suggerimenti per migliorare la definizione dell&#39;indice.

### Strumento Prestazioni query

Lo _strumento Prestazioni query_ OOTB disponibile in [SDK locale](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) e AEMCS tramite Developer Console o `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell` consente a **di analizzare le prestazioni delle query** e [Scheda di riferimento rapido per le query JCR](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=it) di definire la query ottimale.

### Strumenti e suggerimenti per la risoluzione dei problemi

La maggior parte delle seguenti opzioni è applicabile per AEM 6.X e per la risoluzione dei problemi locali.

- Gestione indici disponibile in `http://host:port/libs/granite/operations/content/diagnosistools/indexManager.html` per ottenere informazioni sull&#39;indice come tipo, ultimo aggiornamento, dimensione.

- Registrazione dettagliata della query Oak e dei pacchetti Java™ correlati all&#39;indicizzazione come `org.apache.jackrabbit.oak.plugins.index`, `org.apache.jackrabbit.oak.query` e `com.day.cq.search` tramite `http://host:port/system/console/slinglog` per la risoluzione dei problemi.

- MBean JMX di tipo _IndexStats_ disponibile in `http://host:port/system/console/jmx` per ottenere informazioni sull&#39;indice come stato, avanzamento o statistiche relative all&#39;indicizzazione asincrona. Fornisce anche _FailingIndexStats_. Se non sono presenti risultati, significa che nessun indice è danneggiato. AsyncIndexerService contrassegna qualsiasi indice che non viene aggiornato per 30 minuti (configurabile) come danneggiato e interrompe l’indicizzazione. Se una query non fornisce i risultati previsti, è utile che gli sviluppatori la verifichino prima di procedere con la reindicizzazione, in quanto è computazionalmente costosa e richiede tempo.

- MBean JMX di tipo _LuceneIndex_ disponibile alle `http://host:port/system/console/jmx` per le statistiche dell&#39;indice Lucene come dimensione, numero di documenti per definizione dell&#39;indice.

- MBean JMX di tipo _QueryStat_ disponibile in `http://host:port/system/console/jmx` per le statistiche delle query di Oak, incluse query lente e popolari con dettagli quali query e tempo di esecuzione.

## Risorse aggiuntive

Per ulteriori informazioni, consulta la seguente documentazione:

- [Query e indicizzazione Oak](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/deploying/deploying/queries-and-indexing)
- [Best practice per query e indicizzazione](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices)
- [Procedure consigliate per query e indicizzazione](https://experienceleague.adobe.com/it/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing)

