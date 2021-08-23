---
title: Comprendere le best practice per le API Java in AEM
description: AEM è basato su uno stack software open-source ricco che espone molte API Java da utilizzare durante lo sviluppo. Questo articolo esplora le API principali e spiega quando e perché dovrebbero essere utilizzate.
version: 6.2, 6.3, 6.4, 6.5
sub-product: base, risorse, siti
feature: API
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 3%

---


# Comprendere le best practice per le API Java

Adobe Experience Manager (AEM) è basato su uno stack software open-source ricco che espone molte API Java da utilizzare durante lo sviluppo. Questo articolo esplora le API principali e spiega quando e perché dovrebbero essere utilizzate.

AEM è basato su 4 set API Java principali.

* **Adobe Experience Manager (AEM)**

   * astrazioni di prodotto come pagine, risorse, flussi di lavoro, ecc.

* **[!DNL Apache Sling]Framework Web**

   * astrazioni REST e basate su risorse come risorse, mappe dei valori e richieste HTTP.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * astrazioni di dati e contenuti quali nodo, proprietà e sessioni.

* **[!DNL OSGi (Apache Felix)]**

   * astrazioni del contenitore dell&#39;applicazione OSGi quali servizi e componenti (OSGi).

## Preferenza API Java &quot;regola del pollice&quot;

La regola generale è di preferire le API/astrazioni nel seguente ordine:

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

Se un’API è fornita da AEM, preferiscila rispetto a [!DNL Sling], JCR e OSGi. Se AEM non fornisce un&#39;API, preferisci [!DNL Sling] rispetto a JCR e OSGi.

Questo ordine è una regola generale, il che significa che esistono delle eccezioni. Motivi accettabili per uscire da questa regola sono i seguenti:

* Eccezioni ben note, come descritto di seguito.
* La funzionalità richiesta non è disponibile in un’API di livello superiore.
* Operando nel contesto del codice esistente (codice di prodotto personalizzato o AEM) che utilizza un’API meno preferita e il costo per il passaggio alla nuova API non è giustificabile.

   * È meglio utilizzare in modo coerente l’API di livello inferiore rispetto a creare un mix.

## API AEM

* [**JavaDocs API AEM**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

Le API AEM forniscono astrazioni e funzionalità specifiche per i casi d’uso prodotti.

Ad esempio, AEM le API [PageManager](https://helpx.adobe.com/it/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) e [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) forniscono le astrazioni per i nodi `cq:Page` AEM che rappresentano le pagine web.

Anche se questi nodi sono disponibili tramite [!DNL Sling] API come risorse e API JCR come nodi, le API AEM forniscono astrazioni per i casi d’uso comuni. L’utilizzo delle API AEM garantisce un comportamento coerente tra AEM prodotto e personalizzazioni ed estensioni a AEM.

### com.adobe.* vs com.day.* API

AEM API hanno una preferenza intra-pacchetto, identificata dai seguenti pacchetti Java, in ordine di preferenza:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` supporta i casi di utilizzo dei prodotti, mentre  `com.adobe.granite` supporta i casi di utilizzo di piattaforme cross-product, come i flussi di lavoro o le attività (utilizzati tra i prodotti: AEM Assets, Sites, ecc.).

`com.day.cq` contiene API &quot;originali&quot;. Queste API gestiscono le astrazioni e le funzionalità principali esistenti prima e/o intorno all&#39;acquisizione di [!DNL Day CQ] da parte di Adobe. Queste API sono supportate e non devono essere evitate, a meno che `com.adobe.cq` o `com.adobe.granite` non forniscano un&#39;alternativa (più recente).

Le nuove astrazioni come [!DNL Content Fragments] e [!DNL Experience Fragments] sono integrate nello spazio `com.adobe.cq` anziché `com.day.cq` come descritto di seguito.

### API di query

AEM supporta più linguaggi di query. Le tre lingue principali sono [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath e [AEM Query Builder](https://helpx.adobe.com/it/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

La preoccupazione più importante è mantenere un linguaggio di query coerente all&#39;interno della base di codice, per ridurre la complessità e i costi da comprendere.

Tutti i linguaggi di query hanno effettivamente gli stessi profili di prestazioni, in quanto [!DNL Apache Oak] li trasferisce a JCR-SQL2 per l&#39;esecuzione finale della query, e il tempo di conversione a JCR-SQL2 è trascurabile rispetto al tempo di query stesso.

L’API preferita è [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), che è l’astrazione di livello più alto e fornisce un’API affidabile per la costruzione, l’esecuzione e il recupero dei risultati per le query, e fornisce quanto segue:

* Struttura semplice e parametrizzata delle query (parametri di query modellati come mappa)
* API Java e HTTP native [](https://helpx.adobe.com/it/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [Debugger query OOTB](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [Previsioni OOTB ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) che supportano requisiti di query comuni

* API estensibile, che consente lo sviluppo di predicati [query personalizzati](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 e XPath possono essere eseguiti direttamente tramite [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) e [API JCR](https://docs.adobe.com/content/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html), restituendo risultati rispettivamente a [[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) o [Nodi JCR](https://docs.adobe.com/content/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html).

>[!CAUTION]
>
>AEM API QueryBuilder perde un oggetto ResourceResolver. Per attenuare questa perdita, segui questo [esempio di codice](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).


## [!DNL Sling] API

* [**JavaDocs  [!DNL Sling] API di Apache**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apacheè il framework web RESTful che AEM alla base. [!DNL Sling] fornisce routing delle richieste HTTP, modelli di nodi JCR come risorse, fornisce contesto di sicurezza e molto altro.

[!DNL Sling] Le API hanno il vantaggio aggiuntivo di essere create per l’estensione, il che significa che spesso è più facile e sicuro migliorare il comportamento delle applicazioni create utilizzando  [!DNL Sling] le API rispetto alle API JCR meno estensibili.

### Utilizzo comune delle API [!DNL Sling]

* Accesso ai nodi JCR come [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) e accesso ai loro dati tramite [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fornire un contesto di sicurezza tramite [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Creazione e rimozione di risorse tramite i metodi [create/move/copy/delete di ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Aggiornamento delle proprietà tramite [ModificableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Blocchi predefiniti per l’elaborazione delle richieste di generazione

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtri servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Blocchi predefiniti per l’elaborazione del lavoro asincrono

   * [Gestori di eventi e processi](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Scheduler](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Utenti del servizio](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## API JCR

* **[JavaDocs JCR 2.0](https://docs.adobe.com/content/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Le API [JCR (Java Content Repository) 2.0](https://docs.adobe.com/content/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) fanno parte di una specifica per le implementazioni JCR (nel caso di AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Tutte le implementazioni JCR devono essere conformi e implementate a queste API, ed è quindi l’API di livello più basso per interagire con i contenuti AEM.

Il JCR stesso è un datastore NoSQL gerarchico/basato su albero AEM utilizza come archivio dei contenuti. Il JCR dispone di una vasta gamma di API supportate, che vanno dal CRUD del contenuto alla query del contenuto. Nonostante questa solida API, è raro che siano preferite rispetto alle AEM di livello più alto e alle astrazioni [!DNL Sling] .

Preferisci sempre le API JCR rispetto alle API Apache Jackrabbit Oak. Le API JCR sono per ***interagire*** con un archivio JCR, mentre le API Oak sono per ***implementare*** un archivio JCR.

### Concetti sbagliati comuni sulle API JCR

Anche se JCR è AEM archivio di contenuti, le sue API NON sono il metodo preferito per interagire con il contenuto. Preferiscono invece le API AEM (Pagina, Risorse, Tag, ecc.) o API di risorse Sling in quanto forniscono migliori astrazioni.

>[!CAUTION]
>
>L&#39;ampio utilizzo delle interfacce Session e Node delle API JCR in un&#39;applicazione AEM è un odore di codice. Assicurati che le API [!DNL Sling] non siano utilizzate al loro posto.

### Utilizzo comune delle API JCR

* [Gestione del controllo degli accessi](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [Gestione autorizzabile (utenti/gruppi)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* Osservazione JCR (ascolto di eventi JCR)
* Creazione di strutture dei nodi profondi

   * Mentre le API Sling supportano la creazione di risorse, le API JCR hanno metodi di comodità in [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) e [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) che velocizzano la creazione di strutture profonde.

## API OSGi

* [**JavaDocs OSGi R6**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs di OSGi Declarative Services 1.2 Component Annotations](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs di OSGi Declarative Services 1.2 Mettype Annotations](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs Framework OSGi**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

C&#39;è poca sovrapposizione tra le API OSGi e le API di livello superiore (AEM, [!DNL Sling] e JCR), e la necessità di utilizzare le API OSGi è rara e richiede un alto livello di esperienza di sviluppo AEM.

### API OSGi e Apache Felix

OSGi definisce una specifica a cui tutti i contenitori OSGi devono implementare e conformarsi. AEM implementazione OSGi, Apache Felix, fornisce anche diverse proprie API.

* Preferisci le API OSGi (`org.osgi`) rispetto alle API Apache Felix (`org.apache.felix`).

### Utilizzo comune delle API OSGi

* Annotazioni OSGi per dichiarare i servizi e i componenti OSGi.

   * Preferisci [OSGi Declarative Services (DS) 1.2 Annotazioni](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) su [Felix SCR Annotations](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) per dichiarare i servizi e i componenti OSGi

* API OSGi per l&#39;annullamento/registrazione dinamica dei servizi/componenti OSGi](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html) in-code.[

   * Preferisci l’utilizzo di annotazioni OSGi DS 1.2 quando non è necessaria la gestione condizionale del servizio/componente OSGi (cosa che si verifica più spesso).

## Eccezioni alla regola

Di seguito sono riportate alcune eccezioni comuni alle regole definite in precedenza.

### API delle risorse AEM

* Preferisci [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) su [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Le API di `com.day.cq` Assets forniscono strumenti più gratuiti per AEM casi d’uso di gestione delle risorse.
   * Le API di Granite Assets supportano casi d’uso per la gestione delle risorse di basso livello (versione, relazioni).

### API di query

* AEM QueryBuilder non supporta alcune funzioni di query come [suggerimenti](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), controllo ortografia e suggerimenti di indice, tra le altre funzioni meno comuni. Per eseguire query con queste funzioni è preferibile JCR-SQL2.

### [!DNL Sling] Registrazione servlet {#sling-servlet-registration}

* [!DNL Sling] Registrazione servlet, preferisci le annotazioni  [OSGi DS 1.2 con @](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover  `@SlingServlet`

### [!DNL Sling] Registrazione del filtro {#sling-filter-registration}

* [!DNL Sling] registrazione del filtro, preferisci le annotazioni  [OSGi DS 1.2 con @](https://sling.apache.org/documentation/the-sling-engine/filters.html) SlingServletFilterover  `@SlingFilter`

## Frammenti di codice utili

Di seguito sono riportati utili snippet di codice Java che illustrano le best practice per i casi d’uso comuni che utilizzano API discusse. Questi snippet illustrano anche come passare da API meno preferite a API più preferite.

### Sessione JCR in [!DNL Sling] ResourceResolver

#### Chiusura automatica di Sling ResourceResolver

A partire da AEM 6.2, il ResourceResolver [!DNL Sling] è `AutoClosable` in un&#39;istruzione [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). Utilizzando questa sintassi, non è necessaria una chiamata esplicita a `resourceResolver .close()` .

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

È possibile chiudere manualmente i ResourceResolver in un blocco `finally` se non è possibile utilizzare la tecnica di chiusura automatica mostrata sopra.

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

### Percorso JCR a [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Nodo JCR su [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] AEM risorsa

#### Approccio consigliato

`DamUtil.resolveToAsset(..)`risolve tutte le risorse sotto  `dam:Asset` all’oggetto Risorsa spostandosi verso l’alto nella struttura, in base alle esigenze.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Approccio alternativo

Per adattare una risorsa a una risorsa è necessario che la stessa sia il nodo `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Risorsa AEM pagina

#### Approccio consigliato

`pageManager.getContainingPage(..)` risolve tutte le risorse sotto  `cq:Page` all’oggetto Page spostandosi verso l’alto nella struttura, in base alle esigenze.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Approccio alternativo {#alternative-approach-1}

Per adattare una risorsa a una pagina è necessario che la risorsa stessa sia il nodo `cq:Page` .

```java
Page page = resource.adaptTo(Page.class);
```

### Proprietà di lettura AEM pagina

Utilizzare i getters dell&#39;oggetto Page per ottenere proprietà ben note (`getTitle()`, `getDescription()`, ecc.) e `page.getProperties()` per ottenere il valore `[cq:Page]/jcr:content` ValueMap per il recupero di altre proprietà.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Leggere le proprietà dei metadati AEM risorsa

L’API delle risorse fornisce metodi pratici per leggere le proprietà dal nodo `[dam:Asset]/jcr:content/metadata` . Nota che non si tratta di un ValueMap, il secondo parametro (valore predefinito e casting automatico) non è supportato.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Proprietà [!DNL Sling] [!DNL Resource] {#read-sling-resource-properties}

Quando le proprietà sono memorizzate in posizioni (proprietà o risorse relative) a cui le API AEM (Pagina, Risorsa) non possono accedere direttamente, è possibile utilizzare le [!DNL Sling] Risorse e ValueMaps per ottenere i dati.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In questo caso, potrebbe essere necessario convertire l’oggetto AEM in un [!DNL Sling] [!DNL Resource] per individuare in modo efficiente la proprietà o la risorsa secondaria desiderata.

#### AEM pagina a [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM risorsa a [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Proprietà di scrittura utilizzando la proprietà ModifiableValueMap di [!DNL Sling]

Utilizza [!DNL Sling] di [ModificableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) per scrivere le proprietà sui nodi. Questo può solo scrivere sul nodo immediato (i percorsi delle proprietà relative non sono supportati).

Nota che la chiamata a `.adaptTo(ModifiableValueMap.class)` richiede autorizzazioni di scrittura per la risorsa, altrimenti restituisce null.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Creare una pagina AEM

Utilizzare sempre PageManager per creare pagine in quanto richiede un modello di pagina, è necessario per definire e inizializzare correttamente le pagine in AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Creare una risorsa [!DNL Sling]

ResourceResolver supporta operazioni di base per la creazione di risorse. Durante la creazione di astrazioni di livello superiore (pagine AEM, risorse, tag, ecc.) utilizzare i metodi forniti dai rispettivi responsabili.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Eliminare una risorsa [!DNL Sling]

ResourceResolver supporta la rimozione di una risorsa. Durante la creazione di astrazioni di livello superiore (pagine AEM, risorse, tag, ecc.) utilizzare i metodi forniti dai rispettivi responsabili.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
