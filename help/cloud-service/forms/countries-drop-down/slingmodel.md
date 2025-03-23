---
title: Creare un modello Sling per il componente
description: Creare un modello Sling per il componente
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: f4a18f02-61a2-4fa3-bfbb-41bf696cd2a8
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Creare un modello Sling per il componente

Un modello Sling in AEM è un framework basato su Java utilizzato per semplificare lo sviluppo di logiche back-end per i componenti. Consente agli sviluppatori di mappare i dati dalle risorse AEM (nodi JCR) agli oggetti Java utilizzando le annotazioni, fornendo un modo pulito ed efficiente di gestire i dati dinamici per i componenti.
Questa classe, CountriesDropDownImpl, è un’implementazione dell’interfaccia CountriesDropDown in un progetto AEM (Adobe Experience Manager). Attiva un componente a discesa in cui gli utenti possono selezionare un paese in base al continente selezionato. I dati dell’elenco a discesa vengono caricati in modo dinamico da un file JSON memorizzato in AEM DAM (Digital Asset Manager).

**Campi nella classe**

* **multiSelect**: indica se il menu a discesa consente selezioni multiple.
Viene inserito dalle proprietà del componente utilizzando @ValueMapValue con il valore predefinito false.
* **richiesta**: rappresenta la richiesta HTTP corrente. Utile per accedere a informazioni specifiche del contesto.
* **continente**: memorizza il continente selezionato per il menu a discesa (ad esempio, &quot;asia&quot;, &quot;europa&quot;).
Inserito dalla finestra di dialogo delle proprietà del componente, con il valore predefinito &quot;asia&quot; se non ne viene fornito alcuno.
* **resourceResolver**:Utilizzato per accedere e manipolare le risorse nell&#39;archivio AEM.
* **jsonData**: oggetto JSONObject che memorizza i dati analizzati dal file JSON corrispondente al continente selezionato.

**Metodi nella classe**

* **getContinent()** Metodo semplice per restituire il valore del campo continente.
Registra il valore restituito a scopo di debug.
* **init()** metodo del ciclo di vita con annotazioni di @PostConstruct, eseguito dopo la costruzione della classe e l&#39;inserimento delle dipendenze.Costruisce dinamicamente il percorso del file JSON in base al valore continente.
Recupera il file JSON dal DAM di AEM utilizzando resourceResolver.
Adatta il file a una risorsa, ne legge il contenuto e lo analizza in un oggetto JSONO.
Registra eventuali errori o avvisi durante il processo.
* **getEnums()** Recupera tutte le chiavi (codici paese) dai dati JSON analizzati.
Ordina le chiavi in ordine alfabetico e le restituisce come array.
Registra il numero di codici paese da restituire.
* **getEnumNames()** Estrae tutti i nomi dei paesi dai dati JSON analizzati.
Ordina i nomi in ordine alfabetico e li restituisce come array.
Registra il numero totale di paesi e ciascun nome di paese recuperato.
* **isMultiSelect()** Restituisce il valore del campo multiSelect per indicare se il menu a discesa consente selezioni multiple.



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## Passaggi successivi

[Genera, distribuisci e testa](./build.md)
