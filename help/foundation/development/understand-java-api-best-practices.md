---
title: Java&trade; Best practice API in AEM
description: AEM è basato su un ricco stack di software open-source che espone molte API Java&trade da utilizzare durante lo sviluppo. Questo articolo esplora le principali API e quando e perché dovrebbero essere utilizzate.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 416
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 0%

---

# Best practice per le API Java™

Adobe Experience Manager (AEM) è basato su uno stack di software open-source che espone molte API Java™ da utilizzare durante lo sviluppo. Questo articolo esplora le principali API e quando e perché dovrebbero essere utilizzate.

L’AEM è basato su quattro set API Java™ primari.

* **Adobe Experience Manager (AEM)**

   * Astrazioni di prodotto come pagine, risorse, flussi di lavoro, ecc.

* **Framework web Apache Sling**

   * Astrazioni REST e basate su risorse come risorse, mappe del valore e richieste HTTP.

* **JCR (Apache Jackrabbit Oak)**

   * Astrazioni di dati e contenuti come nodo, proprietà e sessioni.

* **OSGi (Apache Felix)**

   * Astrazioni dei contenitori di applicazioni OSGi come servizi e componenti (OSGi).

## Preferenza API Java™ &quot;rule of thumb&quot;

La regola generale consiste nel preferire le API/astrazioni nel seguente ordine:

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

Se un’API è fornita dall’AEM, preferiscila rispetto a [!DNL Sling], JCR e OSGi. Se l’AEM non fornisce un’API, preferisci [!DNL Sling] su JCR e OSGi.

Questo ordine è una regola generale, il che significa che esistono eccezioni. Motivi accettabili per derogare a questa regola sono:

* Eccezioni ben note, come descritto di seguito.
* La funzionalità richiesta non è disponibile in un’API di livello superiore.
* Operare nel contesto di un codice esistente (codice prodotto personalizzato o AEM) che utilizza un’API meno preferita e il costo per passare alla nuova API è ingiustificabile.

   * È meglio utilizzare in modo coerente l’API di livello inferiore piuttosto che creare un mix.

## API AEM

* [**JavaDocs API per AEM**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

Le API dell’AEM forniscono astrazioni e funzionalità specifiche per i casi d’uso prodotti.

Ad esempio, AEM [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) e [Pagina](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) Le API forniscono astrazioni per `cq:Page` nodi in AEM che rappresentano pagine web.

Mentre questi nodi sono disponibili tramite [!DNL Sling] API come Risorse e API JCR come Nodi, le API AEM forniscono astrazioni per casi d’uso comuni. L’utilizzo delle API dell’AEM garantisce un comportamento coerente tra l’AEM del prodotto e le personalizzazioni ed estensioni dell’AEM.

### com.adobe.&#42; rispetto a com.day.&#42; API

Le API AEM hanno una preferenza intra-pacchetto, identificata dai seguenti pacchetti Java™, in ordine di preferenza:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

Il `com.adobe.cq` supporta casi di utilizzo di prodotti, mentre `com.adobe.granite` supporta casi di utilizzo per più piattaforme, ad esempio flusso di lavoro o attività (utilizzate tra prodotti diversi: AEM Assets, Sites e così via).

Il `com.day.cq` Il pacchetto contiene API &quot;originali&quot;. Queste API rispondono alle astrazioni e alle funzionalità di base che esistevano prima e/o intorno all’acquisizione di da parte di Adobe di [!DNL Day CQ]. Queste API sono supportate e devono essere evitate, a meno che `com.adobe.cq` o `com.adobe.granite` I pacchetti NON forniscono un’alternativa (più recente).

Nuove astrazioni come [!DNL Content Fragments] e [!DNL Experience Fragments] sono integrate in `com.adobe.cq` spazio anziché `com.day.cq` descritte di seguito.

### API di query

L’AEM supporta più linguaggi di query. Le tre lingue principali sono [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath e [Generatore di query AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

La preoccupazione più importante è mantenere un linguaggio di query coerente in tutta la base di codice, per ridurre la complessità e i costi di comprensione.

Tutti i linguaggi di query hanno effettivamente gli stessi profili di prestazioni, come [!DNL Apache Oak] li trasferisce a JCR-SQL2 per l’esecuzione finale della query e il tempo di conversione a JCR-SQL2 è trascurabile rispetto al tempo di query stesso.

L’API preferita è [Generatore di query AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html), che è l’astrazione di livello più alto e fornisce una solida API per la costruzione, l’esecuzione e il recupero dei risultati delle query, oltre a:

* Costruzione di query con parametri semplice (parametri di query modellati come una mappa)
* Nativa [API Java™ e HTTP](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it)
* [Debugger query AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [Predicati AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html) supporto di requisiti di query comuni

* API estensibile, che consente lo sviluppo di [predicati query](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it)
* JCR-SQL2 e XPath possono essere eseguiti direttamente tramite [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) e [API JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), restituendo risultati a [[!DNL Sling] Risorse](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) o [Nodi JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), rispettivamente.

>[!CAUTION]
>
>L’API QueryBuilder di AEM genera perdite in un oggetto ResourceResolver. Per limitare questa perdita, segui questa procedura [esempio di codice](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## [!DNL Sling] API

* [**Apache [!DNL Sling] JavaDocs API**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) è il framework web RESTful alla base dell’AEM. [!DNL Sling] fornisce il routing delle richieste HTTP, modella i nodi JCR come risorse, fornisce il contesto di sicurezza e molto altro.

[!DNL Sling] Le API presentano il vantaggio aggiunto di essere create per l’estensione, il che significa che spesso è più semplice e sicuro incrementare il comportamento delle applicazioni create utilizzando [!DNL Sling] rispetto alle API JCR meno estensibili.

### Utilizzi comuni di [!DNL Sling] API

* Accesso ai nodi JCR come [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) e l&#39;accesso ai dati tramite [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fornire contesto di sicurezza tramite [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Creazione e rimozione di risorse tramite ResourceResolver [metodi create/move/copy/delete (crea/sposta/copia/elimina)](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Aggiornamento delle proprietà tramite [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Elaborazione di blocchi predefiniti di richiesta

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtri servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Elaborazione asincrona dei blocchi predefiniti

   * [Gestori di eventi e processi](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Scheduler](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Utenti del servizio](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## API JCR

* **[JavaDocs JCR 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Il [API JCR (Java™ Content Repository) 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) fa parte di una specifica per le implementazioni JCR (nel caso dell’AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). Tutta l’implementazione JCR deve essere conforme a e implementare queste API, e rappresenta quindi il livello più basso di API per interagire con i contenuti dell’AEM.

Il JCR stesso è un datastore NoSQL gerarchico/basato su struttura utilizzato dall’AEM come archivio dei contenuti. JCR dispone di una vasta gamma di API supportate, che vanno dal contenuto CRUD all’esecuzione di query sui contenuti. Nonostante questa robusta API, è raro che siano preferite rispetto al livello superiore AEM e [!DNL Sling] astrazioni.

Preferisci sempre le API JCR rispetto alle API Apache Jackrabbit Oak. Le API JCR sono per ***interazione*** con un archivio JCR, mentre le API Oak sono per ***implementazione*** un archivio JCR.

### Concetti errati comuni sulle API JCR

Anche se JCR è un archivio di contenuti AEM, le sue API NON sono il metodo preferito per interagire con il contenuto. Preferisci invece le API AEM (Pagina, Risorse, Tag e così via) o le API Sling Resource in quanto forniscono astrazioni migliori.

>[!CAUTION]
>
>L’ampio utilizzo delle interfacce Sessione e Nodo delle API JCR in un’applicazione AEM è di tipo code-smell. Assicurare [!DNL Sling] Al suo posto, utilizza le API.

### Usi comuni delle API JCR

* [Gestione del controllo degli accessi](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Gestione autorizzabile (utenti/gruppi)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* Osservazione JCR (ascolto degli eventi JCR)
* Creazione di strutture di nodi profondi

   * Sebbene le API Sling supportino la creazione di risorse, le API JCR dispongono di metodi di convenienza in [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) e [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) che accelerano la creazione di strutture profonde.

## API OSGi

* [**JavaDocs OSGi R6**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs per annotazioni componenti OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs per annotazioni metatipo OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs per framework OSGi**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Vi è poca sovrapposizione tra le API OSGi e le API di livello superiore (AEM, [!DNL Sling]e JCR) e la necessità di utilizzare le API OSGi è rara e richiede un elevato livello di esperienza nello sviluppo dell’AEM.

### Confronto tra API OSGi e Apache Felix

OSGi definisce una specifica che tutti i contenitori OSGi devono implementare e rispettare. L’implementazione OSGi dell’AEM, Apache Felix, fornisce anche diverse API proprie.

* Preferisci API OSGi (`org.osgi`) sulle API Apache Felix (`org.apache.felix`).

### Utilizzi comuni delle API OSGi

* Annotazioni OSGi per dichiarare servizi e componenti OSGi.

   * Preferisci [Annotazioni di OSGi Declarative Services (DS) 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) oltre [Annotazioni Felix SCR](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) per dichiarare servizi e componenti OSGi

* API OSGi per l’inserimento dinamico nel codice [annullamento/registrazione di servizi/componenti OSGi](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Preferisci l’utilizzo delle annotazioni di OSGi DS 1.2 quando non è necessaria la gestione condizionale del servizio/componente OSGi (il che avviene il più delle volte).

## Eccezioni alla regola

Di seguito sono riportate le eccezioni comuni alle regole definite in precedenza.

### API OSGi

Quando si tratta di astrazioni OSGi di basso livello, come la definizione o la lettura delle proprietà dei componenti OSGi, le astrazioni più recenti fornite da `org.osgi` sono preferite rispetto alle astrazioni Sling di livello superiore. Le astrazioni Sling concorrenti non sono state contrassegnate come `@Deprecated` e suggerisci `org.osgi` alternativa.

Inoltre, tieni presente che la definizione del nodo di configurazione OSGi preferisce `cfg.json` oltre il `sling:OsgiConfig` formato.

### API di AEM Asset

* Preferisci [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) oltre [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Mentre il `com.day.cq` Le API Assets forniscono strumenti complementari per i casi d’uso della gestione delle risorse AEM.
   * Le API di Granite Assets supportano casi d’uso di basso livello per la gestione delle risorse (versione, relazioni).

### API di query

* AEM QueryBuilder non supporta alcune funzioni di query come [suggerimenti](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), controllo ortografico e suggerimenti per l&#39;indicizzazione, oltre ad altre funzioni meno comuni. Per eseguire query con queste funzioni è preferibile utilizzare JCR-SQL2.

### [!DNL Sling] Registrazione servlet {#sling-servlet-registration}

* [!DNL Sling] registrazione servlet, preferenza [Annotazioni OSGi DS 1.2 con @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) oltre `@SlingServlet`

### [!DNL Sling] Registrazione filtro {#sling-filter-registration}

* [!DNL Sling] filtra la registrazione, preferisci [Annotazioni OSGi DS 1.2 con @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) oltre `@SlingFilter`

## Frammenti di codice utili

Di seguito sono riportati snippet di codice Java™ utili che illustrano le best practice per i casi d’uso comuni utilizzando le API discusse. Questi snippet illustrano anche come passare dalle API meno preferite a quelle più preferite.

### Sessione JCR a [!DNL Sling] ResourceResolver

#### ResourceResolver Sling con chiusura automatica

A partire dall&#39;AEM 6.2, la [!DNL Sling] ResourceResolver è `AutoClosable` in un [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) dichiarazione. Utilizzando questa sintassi, una chiamata esplicita a `resourceResolver .close()` non è necessario.

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

#### ResourceResolver Sling chiuso manualmente

ResourceResolvers può essere chiuso manualmente in un `finally` se non è possibile utilizzare la tecnica di chiusura automatica illustrata sopra.

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

### Percorso JCR per [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Nodo JCR a [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] a AEM Asset

#### Approccio consigliato

Il `DamUtil.resolveToAsset(..)` funzione risolve qualsiasi risorsa sotto il `dam:Asset` all’oggetto Asset spostandosi verso l’alto nella struttura, se necessario.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Approccio alternativo

L’adattamento di una risorsa a una risorsa richiede che la risorsa stessa sia la `dam:Asset` nodo.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Pagina Risorsa per AEM

#### Approccio consigliato

`pageManager.getContainingPage(..)` risolve le risorse sotto `cq:Page` all&#39;oggetto Page spostandosi verso l&#39;alto nella struttura, se necessario.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Approccio alternativo {#alternative-approach-1}

L’adattamento di una risorsa a una pagina richiede che la risorsa stessa sia la `cq:Page` nodo.

```java
Page page = resource.adaptTo(Page.class);
```

### Proprietà pagina Leggi AEM

Utilizzare i getter dell&#39;oggetto Page per ottenere proprietà note (`getTitle()`, `getDescription()`e così via) e `page.getProperties()` per ottenere `[cq:Page]/jcr:content` ValueMap per recuperare altre proprietà.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Proprietà metadati risorse AEM lette

L’API Asset fornisce metodi pratici per leggere le proprietà da `[dam:Asset]/jcr:content/metadata` nodo. Questo non è un ValueMap, il secondo parametro (valore predefinito e cast di tipo automatico) non è supportato.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Letto [!DNL Sling] [!DNL Resource] proprietà {#read-sling-resource-properties}

Quando le proprietà sono memorizzate in posizioni (proprietà o risorse relative) in cui le API AEM (Pagina, Risorsa) non possono accedere direttamente, il [!DNL Sling] È possibile utilizzare le risorse e ValueMaps per ottenere i dati.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In questo caso, l’oggetto AEM potrebbe dover essere convertito in un [!DNL Sling] [!DNL Resource] per individuare in modo efficiente la proprietà o la risorsa secondaria desiderata.

#### Pagina AEM a [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Risorsa AEM a [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Scrivi proprietà tramite [!DNL Sling]ModifiableValueMap

Utilizzare [!DNL Sling]di [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) per scrivere le proprietà nei nodi. Questo può scrivere solo nel nodo immediato (i percorsi di proprietà relativi non sono supportati).

Nota la chiamata a `.adaptTo(ModifiableValueMap.class)` richiede autorizzazioni di scrittura per la risorsa, altrimenti restituisce null.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Creare una pagina AEM

Utilizza sempre PageManager per creare pagine man mano che accetta un modello di pagina, necessario per definire e inizializzare correttamente le pagine in AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Creare un [!DNL Sling] Risorsa

ResourceResolver supporta operazioni di base per la creazione di risorse. Quando crei astrazioni di livello superiore (pagine AEM, risorse, tag e così via), utilizza i metodi forniti dai rispettivi manager.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Eliminare un [!DNL Sling] Risorsa

ResourceResolver supporta la rimozione di una risorsa. Quando crei astrazioni di livello superiore (pagine AEM, risorse, tag e così via), utilizza i metodi forniti dai rispettivi manager.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
