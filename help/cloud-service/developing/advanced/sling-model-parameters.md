---
title: Parametrizza modelli Sling da HTL
description: Scopri come passare parametri da HTL a un modello Sling in AEM.
version: Experience Manager as a Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
exl-id: 5d852617-720a-4a00-aecd-26d0ab77d9b3
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Parametrizza modelli Sling da HTL

Adobe Experience Manager (AEM) offre un solido framework per la creazione di applicazioni web dinamiche e adattabili. Una delle sue potenti funzionalità è la possibilità di parametrizzare i modelli Sling, migliorandone la flessibilità e la riutilizzabilità. Questa esercitazione ti guiderà attraverso la creazione di un modello Sling con parametri e il suo utilizzo in HTL (HTML Template Language) per il rendering del contenuto dinamico.

## Script HTL

In questo esempio, lo script HTL invia due parametri al modello Sling `ParameterizedModel`. Il modello modifica questi parametri nel metodo `getValue()` e restituisce il risultato per la visualizzazione.

Questo esempio passa due parametri String, tuttavia è possibile passare qualsiasi tipo di valore o oggetto al modello Sling, purché il tipo di campo [Modello Sling, annotato con `@RequestAttribute`](#sling-model-implementation), corrisponda al tipo di oggetto o valore passato da HTL.

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **Parameterizzazione:** L&#39;istruzione `data-sly-use` crea un&#39;istanza di `ParameterizedModel` con `myParameterOne` e `myParameterTwo`.
- **Rendering condizionale:** `data-sly-test` verifica se il modello è pronto prima di visualizzare il contenuto.
- **Chiamata segnaposto:** `placeholderTemplate` gestisce i casi in cui il modello non è pronto.

## Implementazione del modello Sling

Ecco come implementare il modello Sling:

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **Annotazione modello:** L&#39;annotazione `@Model` designa questa classe come modello Sling, adattabile da `SlingHttpServletRequest` e implementando l&#39;interfaccia `ParameterizedModel`.
- **Attributi richiesta:** l&#39;annotazione `@RequestAttribute` inserisce i parametri HTL nel modello.
- **Metodi:** `getValue()` concatena i parametri e `isReady()` verifica che i parametri non siano vuoti.

L&#39;interfaccia `ParameterizedModel` è definita come segue:

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## Output di esempio

Con i parametri `"Hello"` e `"World"`, lo script HTL genera il seguente output:

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

Questo dimostra come i modelli Sling con parametri in AEM possono essere influenzati in base ai parametri di input forniti tramite HTL.
