---
title: Informazioni sulle procedure ottimali per le API Java in AEM
description: AEM è basato su uno stack software open-source ricco che espone molte API Java da utilizzare durante lo sviluppo. Questo articolo esplora le principali API e spiega quando e perché dovrebbero essere utilizzate.
version: 6.2, 6.3, 6.4, 6.5
sub-product: fondazione, risorse, siti
feature: null
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
translation-type: tm+mt
source-git-commit: fcb47ee3878f6a789b2151e283431c4806e12564
workflow-type: tm+mt
source-wordcount: '2023'
ht-degree: 1%

---


# Informazioni sulle procedure ottimali per le API Java

Adobe Experience Manager (AEM) è basato su uno stack software open-source avanzato che espone molte API Java da utilizzare durante lo sviluppo. Questo articolo esplora le principali API e spiega quando e perché dovrebbero essere utilizzate.

AEM è basato su 4 set di API Java principali.

* **Adobe Experience Manager (AEM)**

   * astrazioni di prodotto come pagine, risorse, flussi di lavoro ecc.

* **[!DNL Apache Sling]Framework Web**

   * astrazioni REST e basate su risorse come risorse, mappe dei valori e richieste HTTP.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * astrazioni di dati e contenuti quali nodi, proprietà e sessioni.

* **[!DNL OSGi (Apache Felix)]**

   * astrazioni dei contenitori di applicazioni OSGi quali servizi e componenti (OSGi).

## Preferenza API Java &quot;regola del pollice&quot;

La regola generale consiste nel preferire le API/astrazioni nel seguente ordine:

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

Se un&#39;API è fornita da AEM, preferitela rispetto a [!DNL Sling], JCR e OSGi. Se AEM non fornisce un&#39;API, preferisci [!DNL Sling] su JCR e OSGi.

Questo ordine è una regola generale, il che significa che esistono delle eccezioni. Motivi accettabili per uscire da questa regola sono:

* Eccezioni ben note, come descritto di seguito.
* La funzionalità richiesta non è disponibile in un&#39;API di livello superiore.
* Operando nel contesto del codice esistente (codice prodotto personalizzato o AEM) che utilizza un&#39;API meno preferita, il costo per passare alla nuova API è ingiustificabile.

   * È meglio utilizzare in modo coerente l&#39;API di livello inferiore piuttosto che creare un mix.

## API AEM

* [**JavaDocs API AEM**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API forniscono astrazioni e funzionalità specifiche per casi di utilizzo prodotti.

Ad esempio, AEM le API [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) e [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) forniscono le astrazioni per i nodi `cq:Page` in AEM che rappresentano le pagine Web.

Anche se questi nodi sono disponibili tramite [!DNL Sling] API come risorse, e API JCR come nodi, AEM API forniscono le astrazioni per i casi d&#39;uso comuni. L&#39;utilizzo delle API AEM assicura un comportamento coerente tra AEM prodotto e personalizzazioni ed estensioni di AEM.

### com.adobe.* vs com.day.* API

AEM API hanno una preferenza intra-package, identificata dai seguenti pacchetti Java, in ordine di preferenza:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` supporta i casi di utilizzo dei prodotti, mentre  `com.adobe.granite` supporta i casi di utilizzo di piattaforme tra più prodotti, come i flussi di lavoro o le attività (utilizzate tra i prodotti:  AEM Assets, Siti ecc.).

`com.day.cq` contiene le API &quot;originali&quot;. Queste API riguardano le astrazioni e le funzionalità di base che esistevano prima e/o intorno  Adobe  acquisizione di [!DNL Day CQ]. Queste API sono supportate e non devono essere evitate, a meno che `com.adobe.cq` o `com.adobe.granite` non forniscano un&#39;alternativa (più recente).

Le nuove astrazioni come [!DNL Content Fragments] e [!DNL Experience Fragments] sono integrate nello spazio `com.adobe.cq` anziché in `com.day.cq` come descritto di seguito.

### API di query

AEM supporta più lingue di query. Le tre lingue principali sono [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath e [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

La preoccupazione più importante è mantenere un linguaggio di query coerente nella base di codice, per ridurre la complessità e i costi da comprendere.

Tutti i linguaggi di query presentano effettivamente gli stessi profili di prestazioni, in quanto [!DNL Apache Oak] li trasferisce a JCR-SQL2 per l&#39;esecuzione finale della query, e il tempo di conversione a JCR-SQL2 è trascurabile rispetto al tempo stesso della query.

L&#39;API preferita è [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), che è l&#39;astrazione di livello più alto e fornisce un&#39;API affidabile per la creazione, l&#39;esecuzione e il recupero dei risultati per le query, e fornisce quanto segue:

* Struttura semplice e con parametri delle query (param di query modellati come una mappa)
* API Java e API HTTP native [](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [Debugger query OOTB](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [Previsioni OOTB ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) che supportano requisiti query comuni

* API estensibile, che consente lo sviluppo di predicati [query ](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html) personalizzati
* JCR-SQL2 e XPath possono essere eseguiti direttamente tramite le API [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) e [JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html), restituendo risultati come [[!DNL Sling] Risorse](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) o [Nodi JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html) rispettivamente.

>[!CAUTION]
>
>AEM API QueryBuilder perde un oggetto ResourceResolver. Per attenuare la perdita, seguire questo [esempio di codice](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).


## [!DNL Sling] API

* [**JavaDocs  [!DNL Sling] API Apache**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apacheè la struttura web RESTful che sostiene AEM. [!DNL Sling] fornisce il routing delle richieste HTTP, modella i nodi JCR come risorse, fornisce il contesto di protezione e molto altro.

[!DNL Sling] Le API hanno il vantaggio aggiunto di essere create per l&#39;estensione, il che significa che spesso è più facile e sicuro migliorare il comportamento delle applicazioni create utilizzando  [!DNL Sling] le API rispetto alle API JCR meno estensibili.

### Utilizzi comuni delle API [!DNL Sling]

* Accesso ai nodi JCR come [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) e accesso ai relativi dati tramite [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Contesto di protezione tramite [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Creazione e rimozione di risorse tramite i [metodi di creazione/spostamento/copia/eliminazione di ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Aggiornamento delle proprietà tramite [ModificableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Creazione di blocchi predefiniti di elaborazione richieste

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtri servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Blocchi di generazione dell&#39;elaborazione del lavoro asincrona

   * [Gestione eventi e processi](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Scheduler](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Utenti del servizio](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## API JCR

* **[JavaDocs JCR 2.0](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Le API [JCR (Java Content Repository) 2.0 APIs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) fanno parte di una specifica per le implementazioni JCR (nel caso di AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Tutte le implementazioni JCR devono essere conformi a queste API e implementarle. Si tratta quindi dell&#39;API di livello più basso per l&#39;interazione con AEM contenuto.

Il JCR stesso è un datastore NoSQL gerarchico basato su albero AEM utilizza come archivio dei contenuti. Il JCR dispone di una vasta gamma di API supportate, che vanno dal CRUD del contenuto al contenuto in query. Nonostante questa potente API, è raro che siano preferite rispetto alle AEM di livello superiore e alle [!DNL Sling] astrazioni.

Preferisci sempre le API JCR rispetto alle API Apache Jackrabbit Oak. Le API JCR sono per ***interagire*** con un repository JCR, mentre le API Oak sono per ***implementare*** un repository JCR.

### Concetti erronei comuni sulle API JCR

Sebbene JCR sia AEM archivio dei contenuti, le sue API NON sono il metodo preferito per interagire con il contenuto. Preferiscono invece le API AEM (Pagina, Risorse, Tag, ecc.) o Sling Resource APIs, in quanto forniscono astrazioni migliori.

>[!CAUTION]
>
>L&#39;ampio utilizzo delle interfacce di sessione e nodo delle API JCR in un&#39;applicazione AEM è un odore di codice. Assicurati che le [!DNL Sling] API non siano utilizzate al posto.

### Utilizzi comuni delle API JCR

* [Gestione del controllo degli accessi](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [Gestione autorizzabile (utenti/gruppi)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* Osservazione JCR (ascolto di eventi JCR)
* Creazione di strutture nodo profonde

   * Mentre le API Sling supportano la creazione di risorse, le API JCR dispongono di metodi di convenienza in [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) e [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) che velocizzano la creazione di strutture profonde.

## API OSGi

* [**JavaDocs OSGi R6**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs di OSGi Dichiarative Services 1.2](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs OSGi Dichiarative Services 1.2 - Annotazioni dei tipi di metriche](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Le API OSGi e le API di livello superiore (AEM, [!DNL Sling] e JCR) sono poco sovrapposte e la necessità di utilizzare le API OSGi è rara e richiede un livello elevato di esperienza AEM sviluppo.

### API OSGi e Apache Felix

OSGi definisce una specifica che tutti i contenitori OSGi devono implementare e rispettare. AEM implementazione OSGi, Apache Felix, fornisce anche diverse proprie API.

* Preferisci le API OSGi (`org.osgi`) alle API Apache Felix (`org.apache.felix`).

### Utilizzi comuni delle API OSGi

* Note OSGi per la dichiarazione di servizi e componenti OSGi.

   * Preferisci [OSGi Dichiarative Services (DS) 1.2 Annotazioni](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) su [Felix SCR Annotazioni](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) per dichiarare servizi e componenti OSGi

* API OSGi per l&#39;annullamento/registrazione dinamica dei servizi/componenti OSGi [ in codice.](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

   * Preferisci l’utilizzo di annotazioni OSGi DS 1.2 quando non è necessaria la gestione condizionale di OSGi Service/Component (come spesso accade).

## Eccezioni alla regola

Di seguito sono riportate alcune eccezioni comuni alle regole definite in precedenza.

### AEM API delle risorse

* Preferisci [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) su [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Le API `com.day.cq` Risorse forniscono strumenti più gratuiti per AEM casi d’uso di gestione delle risorse.
   * Le API di Granite Assets supportano i casi d’uso di gestione delle risorse di basso livello (versione, relazioni).

### API di query

* AEM QueryBuilder non supporta alcune funzioni di query quali [suggerimenti](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), controllo ortografia e suggerimenti indice tra le altre funzioni meno comuni. Per eseguire query con queste funzioni è preferibile JCR-SQL2.

### [!DNL Sling] Registrazione servlet  {#sling-servlet-registration}

* [!DNL Sling] registrazione servlet, preferisci le annotazioni  [OSGi DS 1.2 con @](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover  `@SlingServlet`

### [!DNL Sling] Registrazione filtro  {#sling-filter-registration}

* [!DNL Sling] registrazione del filtro, preferisci le annotazioni  [OSGi DS 1.2 con @](https://sling.apache.org/documentation/the-sling-engine/filters.html) SlingServletFilterover  `@SlingFilter`

## Snippet di codice utili

Di seguito sono riportati utili snippet di codice Java che illustrano le best practice per i casi d’uso comuni utilizzando le API discusse. Questi snippet illustrano inoltre come passare dalle API meno preferite alle API più preferite.

### Sessione JCR in [!DNL Sling] ResourceResolver

#### Chiusura automatica di Sling ResourceResolver

A partire da AEM 6.2, il [!DNL Sling] ResourceResolver è `AutoClosable` in un&#39;istruzione [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). Utilizzando questa sintassi, non è necessaria una chiamata esplicita a `resourceResolver .close()`.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### Sling ResourceResolver chiuso manualmente

ResourceResolver può essere chiuso manualmente in un blocco `finally`, se non è possibile utilizzare la tecnica di chiusura automatica indicata sopra.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### Percorso JCR di [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Nodo JCR su [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] per AEM risorsa

#### Approccio consigliato

`DamUtil.resolveToAsset(..)`risolve tutte le risorse sotto l&#39;oggetto Asset  `dam:Asset` portandosi verso l&#39;alto la struttura ad albero come necessario.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Approccio alternativo

Per adattare una risorsa a una risorsa, è necessario che la risorsa stessa sia il nodo `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Risorsa AEM pagina

#### Approccio consigliato

`pageManager.getContainingPage(..)` risolve tutte le risorse sotto  `cq:Page` l&#39;oggetto Page, risalendo la struttura ad albero come necessario.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Approccio alternativo {#alternative-approach-1}

Per adattare una risorsa a una pagina, è necessario che la risorsa stessa sia il nodo `cq:Page`.

```java
Page page = resource.adaptTo(Page.class);
```

### Proprietà AEM pagina

Utilizzare i getter dell&#39;oggetto Page per ottenere proprietà note (`getTitle()`, `getDescription()`, ecc.) e `page.getProperties()` per ottenere il valore `[cq:Page]/jcr:content` ValueMap per il recupero di altre proprietà.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lettura AEM proprietà dei metadati delle risorse

L&#39;API Asset fornisce metodi pratici per la lettura delle proprietà dal nodo `[dam:Asset]/jcr:content/metadata`. Si noti che non si tratta di una ValueMap, il secondo parametro (valore predefinito e inserimento automatico) non è supportato.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Proprietà [!DNL Sling] [!DNL Resource] {#read-sling-resource-properties}

Quando le proprietà sono memorizzate in posizioni (proprietà o risorse relative) a cui le API AEM (Pagina, Risorsa) non possono accedere direttamente, per ottenere i dati è possibile utilizzare le [!DNL Sling] Risorse e ValueMaps.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In questo caso, l&#39;oggetto AEM può essere convertito in una [!DNL Sling] [!DNL Resource] per individuare in modo efficiente la proprietà o la sottorisorsa desiderata.

#### AEM pagina a [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM risorsa a [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Scrivere le proprietà utilizzando la proprietà ModificableValueMap di [!DNL Sling]

Utilizzare [!DNL Sling] [ModificableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) per scrivere le proprietà sui nodi. Questo può essere scritto solo nel nodo immediato (i percorsi delle proprietà relative non sono supportati).

Nota: la chiamata a `.adaptTo(ModifiableValueMap.class)` richiede autorizzazioni di scrittura per la risorsa, altrimenti restituisce null.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Creare una pagina AEM

Utilizzare sempre PageManager per creare le pagine necessarie per definire e inizializzare correttamente le pagine in AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Creare una risorsa [!DNL Sling]

ResourceResolver supporta operazioni di base per la creazione di risorse. Durante la creazione di astrazioni di livello superiore (AEM pagine, risorse, tag ecc.) utilizzare i metodi forniti dai rispettivi manager.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Eliminare una risorsa [!DNL Sling]

ResourceResolver supporta la rimozione di una risorsa. Durante la creazione di astrazioni di livello superiore (AEM pagine, risorse, tag ecc.) utilizzare i metodi forniti dai rispettivi manager.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
