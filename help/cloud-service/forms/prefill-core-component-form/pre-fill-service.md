---
title: Modulo adattivo basato su Componente core precompilato
description: Scopri come precompilare un modulo adattivo con i dati
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
duration: 133
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 1%

---

# Implementazione dell&#39;interfaccia DataProvider

Crea un progetto AEM utilizzando l’archetipo più recente e aggiungi il seguente codice al progetto.
In genere i dati vengono recuperati da un’origine dati esterna, ad esempio un database, un archivio di Azure, un bucket S3 o qualsiasi altra origine. Per recuperare i dati dovrai scrivere il codice.

```java
package com.aemforms.prefill;

import com.adobe.forms.common.service.*;
import com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.forms.common.*;

import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.UnsupportedEncodingException;
import java.util.List;
import java.util.Map;

@Component(
        service={DataProvider.class, CustomAFPrefillService.class},
        immediate = true
)
public class CustomAFPrefillService implements DataProvider {
    @Reference
    private DataManager dataManager;

    @Reference
    private StoreAndFetchDataFromAzureStorage storeandfetch;

    private transient Logger  logger = LoggerFactory.getLogger(CustomAFPrefillService.class);

    @Override
    public PrefillData getPrefillData(DataOptions dataOptions) throws FormsException {
        logger.info("Invoking AF custom prefill service");
        // get the value of the request parameter called guid from the url
        String guid = (String) dataOptions.getExtras().get("guid");
        
        // fetch the data associated with the guid using a custom OSGi service or in any other manner
        
        String data = storeandfetch.getBlobData(guid);
        InputStream dataInputStream = null;
        ContentType contentType = dataOptions.getContentType();

        Map<String, Object> extras = dataOptions.getExtras();
        List<FileAttachmentWrapper> fileAttachmentWrappers = null;
        PrefillData prefillData = null;
        Map<String, String> customContext = null;
        if(extras != null && extras.containsKey(DataManager.UNIQUE_ID) && dataManager != null) {
            String dataKey = extras.get(DataManager.UNIQUE_ID).toString();
            if(dataManager.get(dataKey) != null) {
                data = (String) dataManager.get(dataKey);
                fileAttachmentWrappers = (List<FileAttachmentWrapper>) dataManager.get(DataManager.getFileAttachmentMapKey(dataKey));
            }
            customContext = dataManager.getCustomContext(dataKey);
        }
        if(StringUtils.isNotBlank(data)) {
            dataInputStream = getDataInputStream(data);
            prefillData = new PrefillData(dataInputStream, contentType, fileAttachmentWrappers, customContext);
        }
        return prefillData;


    }

    @Override
    public String getServiceName() {
        return "Core Custom Pre-fill Service";
    }

    @Override
    public String getServiceDescription() {
         return "Core Custom Pre-fill Service";
    }
    private InputStream getDataInputStream(String data) {
        InputStream dataInputStream = null;
logger.info("got data  "+data);
        try {
            dataInputStream = new ByteArrayInputStream(data.getBytes("UTF-8"));
        } catch (UnsupportedEncodingException e) {
            logger.error("[Custom AF Prefill] Exception in custom af prefill service", e);
        }
        return dataInputStream;
    }
}
```

## Gestione dati

Includi il codice seguente nel progetto. Il codice elencato sopra utilizza questa classe DataManager

```java
import java.io.Serializable;
import java.util.*;
import java.util.concurrent.ConcurrentHashMap;


import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;


@Component(
        service=DataManager.class,
        immediate = true
)
public class DataManager implements Serializable {
    private static int MAX_ENTRIES = 10000;
    private static int INITIAL_CAPACITY = 100;
    public final static String UNIQUE_ID = "prefillId";

    public static String getFileAttachmentMapKey(String uuid) {
        return uuid + "fileAttachmentMap";
    }

    private Map<Object, Object> cache;

    protected void activate() {
        cache = Collections.synchronizedMap(new LinkedHashMap<Object, Object>(INITIAL_CAPACITY + 1, 1.0f, true) {
            /**
             * Returns <tt>true</tt> if this <code>GuideCache</code> has more entries than the maximum specified when it was
             * created.
             *
             * <p>
             * This method <em>does not</em> modify the underlying <code>Map</code>; it relies on the implementation of
             * <code>LinkedHashMap</code> to do that, but that behavior is documented in the JavaDoc for
             * <code>LinkedHashMap</code>.
             * </p>
             *
             * @param eldest
             *            the <code>Entry</code> in question; this implementation doesn't care what it is, since the
             *            implementation is only dependent on the size of the cache
             * @return <tt>true</tt> if the oldest
             * @see java.util.LinkedHashMap#removeEldestEntry(java.util.Map.Entry)
             */
            protected boolean removeEldestEntry(final Map.Entry eldest) {
                return size() > MAX_ENTRIES;
            }
        });
    }


    public Object get(String key) {
        Object value = null;
        if (cache.containsKey(key)) {
            value = cache.get(key);
        }
        return value;
    }

    public Map <String, String> getCustomContext(String id) {
        Map<String, String> customContext = new HashMap<>();
        customContext.put(UNIQUE_ID, id);
        return customContext;
    }
    public Object put(String key, Object value) {
        return cache.put(key, value);
    }
}
```

## Distribuire il codice

Puoi distribuire il codice nell’ambiente locale predisposto per il cloud o nell’istanza cloud utilizzando Cloud Manager.

### Passaggi successivi

[Testare la soluzione](./test-solution.md)