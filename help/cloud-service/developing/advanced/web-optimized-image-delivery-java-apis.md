---
title: Consegna di immagini ottimizzata per il web Java&trade; API
description: Scopri come utilizzare le API Java&trade di AEM as a Cloud Service per la consegna delle immagini ottimizzate per il web per sviluppare esperienze web ad alte prestazioni.
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
exl-id: c6bb9d6d-aef0-42d5-a189-f904bbbd7694
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 2%

---

# API Java™ per la consegna delle immagini ottimizzate per il web

Scopri come utilizzare le API Java™ per la consegna delle immagini ottimizzate per il web di AEM as a Cloud Service per sviluppare esperienze web ad alte prestazioni.

Supporto AEM as a Cloud Service [consegna di immagini ottimizzate per il web](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=it) che genera automaticamente le rappresentazioni web ottimizzate delle immagini delle risorse. È possibile utilizzare tre approcci principali per la consegna di immagini ottimizzate per il web:

1. [Utilizzare i componenti WCM core AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
2. Crea un componente personalizzato che [estende il componente immagine del componente WCM core AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html#tackling-the-image-problem)
3. Crea un componente personalizzato che utilizza l’API Java™ AssetDelivery per generare URL di immagini ottimizzate per il web.

Questo articolo esplora l’utilizzo delle API Java™ per immagini ottimizzate per il web in un componente personalizzato, in modo da consentire il funzionamento basato su codice sia su AEM as a Cloud Service che su AEM SDK.

## API Java™

Il [API AssetDelivery](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) è un servizio OSGi che genera URL di consegna ottimizzati per il web per le risorse di immagini. `AssetDelivery.getDeliveryURL(...)` le opzioni consentite sono [documentato qui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

Il `AssetDelivery` Il servizio OSGi è soddisfatto solo quando viene eseguito in AEM as a Cloud Service. Sull&#39;SDK dell&#39;AEM, i riferimenti `AssetDelivery` Restituzione servizio OSGi `null`. È consigliabile utilizzare l’URL ottimizzato per il web in modo condizionale quando viene eseguito su AEM as a Cloud Service e utilizzare un URL di immagine di fallback sull’SDK AEM. In genere il rendering web della risorsa è un fallback sufficiente.


### Utilizzo dell’API nel servizio OSGi

Contrassegna`AssetDelivery` Nei servizi OSGi personalizzati, fai riferimento a come facoltativo in modo che il servizio OSGi personalizzato rimanga disponibile nell’SDK dell’AEM.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### Utilizzo dell’API nel modello Sling

Contrassegna`AssetDelivery` Nei modelli Sling personalizzati puoi fare riferimento a come facoltativo, in modo che il modello Sling personalizzato rimanga disponibile nell’SDK per AEM.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### Uso condizionale dell’API

Restituisce in modo condizionale l’URL dell’immagine ottimizzata per il web o l’URL di fallback in base al `AssetDelivery` Disponibilità del servizio OSGi. L’uso condizionale consente il funzionamento del codice durante l’esecuzione del codice sull’SDK dell’AEM.

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## Esempio di codice

Il codice che segue crea un componente di esempio che visualizza un elenco di risorse immagine utilizzando gli URL di immagini ottimizzati per il web.

Quando il codice viene eseguito su AEM as a Cloud Service, nel componente personalizzato vengono utilizzate le rappresentazioni delle immagini Web ottimizzate per il web.

![Immagini ottimizzate per il web su AEM as a Cloud Service](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEM as a Cloud Service supporta l’API AssetDelivery, per cui viene utilizzata la rappresentazione web ottimizzata per il web_

Quando il codice viene eseguito sull’SDK dell’AEM, vengono utilizzate le rappresentazioni web statiche meno ottimali, consentendo al componente di funzionare durante lo sviluppo locale.

![Immagini di fallback ottimizzate per il web sull’SDK per AEM](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_L’SDK di AEM non supporta l’API AssetDelivery, pertanto viene utilizzata la rappresentazione web statica di fallback (PNG o JPEG)_

L’implementazione è suddivisa in tre parti logiche:

1. Il `WebOptimizedImage` Il servizio OSGi funge da &quot;proxy intelligente&quot; per i dati forniti dall’AEM `AssetDelivery` Servizio OSGi in grado di gestire l’esecuzione sia in AEM as a Cloud Service che in AEM SDK.
2. Il `ExampleWebOptimizedImages` Il modello Sling fornisce la logica di business per la raccolta dell’elenco delle risorse di immagini e dei relativi URL ottimizzati per il web da visualizzare.
3. Il `example-web-optimized-images` Il componente AEM implementa HTL per visualizzare l’elenco delle immagini ottimizzate per il web.

Il codice di esempio seguente può essere copiato nella base di codice e aggiornato in base alle esigenze.

### Servizio OSGi

Il `WebOptimizedImage` Il servizio OSGi è suddiviso in un’interfaccia pubblica indirizzabile (`WebOptimizedImage`) e un&#39;implementazione interna (`WebOptimizedImageImpl`). Il `WebOptimizedImageImpl` restituisce un URL di immagine ottimizzata per il web quando viene eseguito su AEM as a Cloud Service e un URL di rappresentazione web statico sull’SDK AEM, che consente al componente di rimanere funzionale sull’SDK AEM.

#### Interfaccia

L’interfaccia definisce il contratto di servizio OSGi con cui altri codici, come i modelli Sling, possono interagire.

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### Implementazione

L’implementazione del servizio OSGi include un riferimento opzionale all’AEM `AssetDelivery` Servizio OSGi e logica di fallback per selezionare un URL immagine appropriato quando `AssetDelivery` è `null` sull&#39;SDK dell&#39;AEM. La logica di fallback può essere aggiornata in base ai requisiti.

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### Modello Sling

Il `ExampleWebOptimizedImages` Il modello Sling è suddiviso in un’interfaccia pubblica indirizzabile (`ExampleWebOptimizedImages`) e un&#39;implementazione interna (`ExampleWebOptimizedImagesImpl`);

Il `ExampleWebOptimizedImagesImpl` Il modello Sling raccoglie l’elenco delle risorse immagine da visualizzare e richiama il modello personalizzato. `WebOptimizedImage` Servizio OSGi per ottenere l’URL dell’immagine ottimizzata per il web. Poiché questo modello Sling rappresenta un componente AEM, utilizza i metodi usuali, come `isEmpty()`, `getId()`, e `getData()` tuttavia, questi metodi non sono direttamente rilevanti per l’utilizzo di immagini ottimizzate per il web.

#### Interfaccia

L’interfaccia definisce il contratto del modello Sling con cui altri codici, come HTL, possono interagire.

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### Implementazione

Il modello Sling utilizza il `WebOptimizeImage` Servizio OSGi per raccogliere gli URL delle immagini ottimizzate per il web per le risorse immagine visualizzate dal suo componente.

In questo esempio, viene utilizzata una semplice query per raccogliere le risorse immagine.

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### Componente AEM

Un componente AEM è associato al tipo di risorsa Sling del `WebOptimizedImagesImpl` Implementazione del modello Sling ed è responsabile della visualizzazione dell’elenco delle immagini.



Il componente riceve un elenco di `Img` oggetti tramite `getImages()` che includono le immagini WEBP ottimizzate per il web quando vengono eseguite su AEM as a Cloud Service . Il componente riceve un elenco di `Img` oggetti tramite `getImages()` che includono immagini web PNG/JPEG statiche quando vengono eseguite sull’SDK dell’AEM.

#### HTL

HTL utilizza `WebOptimizedImages` Modello Sling ed esegue il rendering dell’elenco di  `Img` oggetti restituiti da `getImages()`.

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```
