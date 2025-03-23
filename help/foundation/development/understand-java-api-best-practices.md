---
title: Java&trade; Best practice API in AEM
description: AEM è basato su uno stack di software open-source che espone molte API Java&trade; da utilizzare durante lo sviluppo. Questo articolo esplora le principali API e quando e perché dovrebbero essere utilizzate.
version: Experience Manager 6.4, Experience Manager 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 416
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 0%

---

# Best practice per le API Java™

Adobe Experience Manager (AEM) è basato su uno stack di software open-source che espone molte API Java™ da utilizzare durante lo sviluppo. Questo articolo esplora le principali API e quando e perché dovrebbero essere utilizzate.

AEM è basato su quattro set API Java™ primari.

* **Adobe Experience Manager (AEM)**

   * Astrazioni di prodotto come pagine, risorse, flussi di lavoro, ecc.

* **Framework Web Apache Sling**

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

Se un’API è fornita da AEM, preferiscila rispetto a [!DNL Sling], JCR e OSGi. Se AEM non fornisce un&#39;API, preferisci [!DNL Sling] rispetto a JCR e OSGi.

Questo ordine è una regola generale, il che significa che esistono eccezioni. Motivi accettabili per derogare a questa regola sono:

* Eccezioni ben note, come descritto di seguito.
* La funzionalità richiesta non è disponibile in un’API di livello superiore.
* Operare nel contesto di un codice esistente (codice prodotto personalizzato o AEM) che utilizza un’API meno preferita e il costo per passare alla nuova API è ingiustificabile.

   * È meglio utilizzare in modo coerente l’API di livello inferiore piuttosto che creare un mix.

## API AEM

* [**JavaDocs API AEM**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

Le API di AEM forniscono astrazioni e funzionalità specifiche per i casi d’uso prodotti.

Le API [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) e [Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) di AEM, ad esempio, forniscono astrazioni per `cq:Page` nodi in AEM che rappresentano pagine Web.

Anche se questi nodi sono disponibili tramite API [!DNL Sling] come risorse e API JCR come nodi, le API di AEM forniscono astrazioni per i casi d&#39;uso comuni. L’utilizzo delle API di AEM garantisce un comportamento coerente tra AEM e le personalizzazioni ed estensioni di AEM.

### com.adobe.&#42; rispetto a com.day.&#42; API

Le API di AEM hanno una preferenza intra-package, identificata dai seguenti pacchetti Java™, in ordine di preferenza:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

Il pacchetto `com.adobe.cq` supporta casi di utilizzo di prodotti, mentre `com.adobe.granite` supporta casi di utilizzo di piattaforme tra più prodotti, ad esempio flussi di lavoro o attività (utilizzate tra i prodotti: AEM Assets, Sites e così via).

Il pacchetto `com.day.cq` contiene API &quot;originali&quot;. Queste API rispondono alle astrazioni e alle funzionalità di base che esistevano prima e/o intorno all&#39;acquisizione di [!DNL Day CQ] da parte di Adobe. Queste API sono supportate e devono essere evitate, a meno che `com.adobe.cq` o `com.adobe.granite` pacchetti NON forniscano un&#39;alternativa (più recente).

Le nuove astrazioni, ad esempio [!DNL Content Fragments] e [!DNL Experience Fragments], vengono create nello spazio `com.adobe.cq` anziché `com.day.cq` come descritto di seguito.

### API di query

AEM supporta più linguaggi di query. Le tre lingue principali sono [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath e [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

La preoccupazione più importante è mantenere un linguaggio di query coerente in tutta la base di codice, per ridurre la complessità e i costi di comprensione.

Tutti i linguaggi di query hanno effettivamente gli stessi profili di prestazioni, in quanto [!DNL Apache Oak] li trasferisce a JCR-SQL2 per l&#39;esecuzione finale della query e il tempo di conversione a JCR-SQL2 è trascurabile rispetto al tempo di query stesso.

L&#39;API preferita è [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html), che rappresenta l&#39;astrazione di livello più alto e fornisce un&#39;API solida per la costruzione, l&#39;esecuzione e il recupero dei risultati per le query, nonché le seguenti informazioni:

* Costruzione di query con parametri semplice (parametri di query modellati come una mappa)
* API [Java™ e HTTP native](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it)
* [Debugger query AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [Predicati AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html) che supportano i requisiti di query comuni

* API estensibile, che consente lo sviluppo di [predicati di query](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it) personalizzati
* JCR-SQL2 e XPath possono essere eseguiti direttamente tramite [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) e [API JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), restituendo risultati rispettivamente come [[!DNL Sling] Risorse](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) o [Nodi JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html).

>[!CAUTION]
>
>L&#39;API QueryBuilder di AEM genera una perdita di un oggetto ResourceResolver. Per limitare questa perdita, segui questo [esempio di codice](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## [!DNL Sling] API

* [**JavaDocs API Apache [!DNL Sling]**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) è il framework web RESTful su cui si basa AEM. [!DNL Sling] fornisce il routing delle richieste HTTP, modella i nodi JCR come risorse, fornisce il contesto di sicurezza e molto altro.

[!DNL Sling] API hanno il vantaggio aggiunto di essere create per l&#39;estensione, il che significa che è spesso più facile e sicuro migliorare il comportamento delle applicazioni create utilizzando [!DNL Sling] API rispetto alle API JCR meno estensibili.

### Utilizzi comuni delle API [!DNL Sling]

* Accesso ai nodi JCR come [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) e ai relativi dati tramite [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Fornire contesto di sicurezza tramite [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Creazione e rimozione di risorse tramite i [metodi di creazione/spostamento/copia/eliminazione di ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Aggiornamento delle proprietà tramite [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Elaborazione di blocchi predefiniti di richiesta

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtri servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Elaborazione asincrona dei blocchi predefiniti

   * [Gestori eventi e processi](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Utilità di pianificazione](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Utenti del servizio](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## API JCR

* **[JavaDocs JCR 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Le API [JCR (Java™ Content Repository) 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) fanno parte di una specifica per le implementazioni JCR (nel caso di AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). Tutta l’implementazione JCR deve essere conforme a e implementare queste API, e pertanto rappresenta l’API di livello più basso per interagire con i contenuti di AEM.

Il JCR stesso è un archivio dati NoSQL gerarchico/ad albero utilizzato da AEM come archivio dei contenuti. JCR dispone di una vasta gamma di API supportate, che vanno dal contenuto CRUD all’esecuzione di query sui contenuti. Nonostante questa robusta API, è raro che siano preferite rispetto alle astrazioni di livello superiore di AEM e [!DNL Sling].

Preferisci sempre le API JCR rispetto alle API Apache Jackrabbit Oak. Le API JCR sono per ***interagire*** con un archivio JCR, mentre le API Oak sono per ***implementare*** un archivio JCR.

### Concetti errati comuni sulle API JCR

Anche se JCR è l’archivio dei contenuti di AEM, le sue API NON sono il metodo preferito per interagire con il contenuto. Preferisci invece le API di AEM (Pagina, Assets, Tag e così via) o le API di risorse Sling poiché forniscono astrazioni migliori.

>[!CAUTION]
>
>Un ampio utilizzo delle interfacce Sessione e Nodo delle API JCR in un’applicazione AEM è il code-smell. Assicurati di utilizzare al suo posto [!DNL Sling] API.

### Usi comuni delle API JCR

* [Gestione del controllo degli accessi](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Gestione autorizzabile (utenti/gruppi)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* Osservazione JCR (ascolto degli eventi JCR)
* Creazione di strutture di nodi profondi

   * Sebbene le API Sling supportino la creazione di risorse, le API JCR dispongono di metodi di convenienza in [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) e [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) che accelerano la creazione di strutture profonde.

## API OSGi

* [**JavaDocs OSGi R6**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs per annotazioni componenti OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs annotazioni metatipo OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs framework OSGi**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

La sovrapposizione tra le API OSGi e le API di livello superiore (AEM, [!DNL Sling] e JCR) è minima e la necessità di utilizzare le API OSGi è rara e richiede un elevato livello di esperienza nello sviluppo di AEM.

### Confronto tra API OSGi e Apache Felix

OSGi definisce una specifica che tutti i contenitori OSGi devono implementare e rispettare. Anche l’implementazione OSGi di AEM, Apache Felix, fornisce diverse API proprie.

* Preferisci le API OSGi (`org.osgi`) alle API Apache Felix (`org.apache.felix`).

### Utilizzi comuni delle API OSGi

* Annotazioni OSGi per dichiarare servizi e componenti OSGi.

   * Preferisci [Annotazioni di Servizi dichiarativi OSGi 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) rispetto a [Annotazioni di Felix SCR](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) per dichiarare servizi e componenti OSGi

* API OSGi per l&#39;annullamento/registrazione dinamica in codice [dei servizi/componenti OSGi](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Preferisci l’utilizzo delle annotazioni di OSGi DS 1.2 quando non è necessaria la gestione condizionale del servizio/componente OSGi (il che avviene il più delle volte).

## Eccezioni alla regola

Di seguito sono riportate le eccezioni comuni alle regole definite in precedenza.

### API OSGi

Quando si tratta di astrazioni OSGi di basso livello, ad esempio la definizione o la lettura nelle proprietà dei componenti OSGi, le astrazioni più recenti fornite da `org.osgi` sono preferite alle astrazioni Sling di livello superiore. Le astrazioni Sling concorrenti non sono state contrassegnate come `@Deprecated` e suggeriscono l&#39;alternativa `org.osgi`.

Inoltre, la definizione del nodo di configurazione OSGi preferisce `cfg.json` rispetto al formato `sling:OsgiConfig`.

### API di AEM Asset

* Preferisci [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) a [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Mentre le API di Assets `com.day.cq` forniscono strumenti complementari per i casi d&#39;uso di AEM relativi alla gestione delle risorse.
   * Le API di Granite Assets supportano casi d’uso di basso livello per la gestione delle risorse (versione, relazioni).

### API di query

* AEM QueryBuilder non supporta alcune funzioni di query, ad esempio [suggerimenti](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), controllo ortografico e suggerimenti indice, oltre ad altre funzioni meno comuni. Per eseguire query con queste funzioni è preferibile utilizzare JCR-SQL2.

### Registrazione servlet [!DNL Sling] {#sling-servlet-registration}

* [!DNL Sling] registrazione servlet, preferisci [annotazioni OSGi DS 1.2 con @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) rispetto a `@SlingServlet`

### Registrazione filtro [!DNL Sling] {#sling-filter-registration}

* [!DNL Sling] registrazione filtro, preferisci [annotazioni di OSGi DS 1.2 con @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) rispetto a `@SlingFilter`

## Frammenti di codice utili

Di seguito sono riportati snippet di codice Java™ utili che illustrano le best practice per i casi d’uso comuni utilizzando le API discusse. Questi snippet illustrano anche come passare dalle API meno preferite a quelle più preferite.

### Sessione JCR a ResourceResolver [!DNL Sling]

#### ResourceResolver Sling con chiusura automatica

A partire da AEM 6.2, ResourceResolver [!DNL Sling] è `AutoClosable` in un&#39;istruzione [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). Utilizzando questa sintassi, non è necessaria una chiamata esplicita a `resourceResolver .close()`.

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

ResourceResolvers può essere chiuso manualmente in un blocco `finally`, se non è possibile utilizzare la tecnica di chiusura automatica illustrata sopra.

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

### [!DNL Sling] [!DNL Resource] alla risorsa AEM

#### Approccio consigliato

La funzione `DamUtil.resolveToAsset(..)` risolve qualsiasi risorsa sotto `dam:Asset` nell&#39;oggetto Asset spostandosi verso l&#39;alto nella struttura come necessario.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Approccio alternativo

L&#39;adattamento di una risorsa a una risorsa richiede che la risorsa stessa sia il nodo `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### Pagina Risorsa [!DNL Sling] per AEM

#### Approccio consigliato

`pageManager.getContainingPage(..)` risolve qualsiasi risorsa sotto `cq:Page` nell&#39;oggetto Page spostandosi verso l&#39;alto nella struttura, se necessario.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Approccio alternativo {#alternative-approach-1}

L&#39;adattamento di una risorsa a una pagina richiede che la risorsa stessa sia il nodo `cq:Page`.

```java
Page page = resource.adaptTo(Page.class);
```

### Proprietà pagina Leggi AEM

Utilizzare i getter dell&#39;oggetto Page per ottenere proprietà note (`getTitle()`, `getDescription()` e così via) e `page.getProperties()` per ottenere il ValueMap `[cq:Page]/jcr:content` per recuperare altre proprietà.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Proprietà dei metadati delle risorse di AEM

L&#39;API Asset fornisce metodi pratici per la lettura delle proprietà dal nodo `[dam:Asset]/jcr:content/metadata`. Questo non è un ValueMap, il secondo parametro (valore predefinito e cast di tipo automatico) non è supportato.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lettura delle proprietà di [!DNL Sling] [!DNL Resource] {#read-sling-resource-properties}

Quando le proprietà sono memorizzate in percorsi (proprietà o risorse relative) in cui le API di AEM (Page, Asset) non possono accedere direttamente, è possibile utilizzare le risorse [!DNL Sling] e ValueMaps per ottenere i dati.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In questo caso, potrebbe essere necessario convertire l&#39;oggetto AEM in [!DNL Sling] [!DNL Resource] per individuare in modo efficiente la proprietà o la risorsa secondaria desiderata.

#### Pagina AEM su [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Risorsa AEM a [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Scrivere proprietà utilizzando ModifiableValueMap di [!DNL Sling]

Utilizza [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) di [!DNL Sling] per scrivere proprietà nei nodi. Questo può scrivere solo nel nodo immediato (i percorsi di proprietà relativi non sono supportati).

Nota: la chiamata a `.adaptTo(ModifiableValueMap.class)` richiede autorizzazioni di scrittura per la risorsa, altrimenti restituisce null.

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

### Crea una risorsa [!DNL Sling]

ResourceResolver supporta operazioni di base per la creazione di risorse. Durante la creazione di astrazioni di livello superiore (pagine AEM, Assets, tag e così via), utilizza i metodi forniti dai rispettivi manager.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Elimina una risorsa [!DNL Sling]

ResourceResolver supporta la rimozione di una risorsa. Durante la creazione di astrazioni di livello superiore (pagine AEM, Assets, tag e così via), utilizza i metodi forniti dai rispettivi manager.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
