---
title: Nozioni di base sullo sviluppo dei servizi OSGi
description: Informazioni di base sullo sviluppo di un servizio OSGi
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8227
thumbnail: 335476.jpeg
last-substantial-update: 2022-09-16T00:00:00Z
exl-id: a3a9bf59-e9a2-4322-ac93-9c12c70b9a75
duration: 492
source-git-commit: a18bf2c8b57eaaac3686a26fa1fb39e6fc075af5
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 1%

---

# Servizi OSGi

Scopri le nozioni di base sullo sviluppo dei servizi OSGi, tra cui:

+ Come convertire un Java POJO in un servizio OSGi
+ Associare un servizio OSGi a un’interfaccia Java

>[!VIDEO](https://video.tv.adobe.com/v/335476?quality=12&learn=on)

## Riferimenti

+ [@Component JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.html)
+ [@ProviderType JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/ProviderType.html)
+ [@Version JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/Version.html)

## Codice

### Activities.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/Activities.java`

```java
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.ProviderType;

@ProviderType
public interface Activities {    
    String getRandomActivity();
}
```

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Component;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {

    private static final String[] ACTIVITIES = new String[] { 
        "Camping", "Skiing",  "Skateboarding"
    };

    //private final int randomIndex = new Random().nextInt(ACTIVITIES.length);
    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(ACTIVITIES.length);
        return ACTIVITIES[randomIndex];
    }    
}
```

### package-info.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/package-info.java`

```java
@Version("1.0")
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.Version;
```

Aggiunta di un `package-info.java` è necessario per garantire che altri bundle OSGi in AEM possano risolvere l’interfaccia di servizio OSGi (o qualsiasi classe Java). Se il `package-info.java` manca, il pacchetto Java e le relative interfanze o classi Java non vengono esportati. Altri bundle OSGi che tentano di importare queste interfacce o classi Java da questo pacchetto Java genereranno un errore con il messaggio __Impossibile risolvere__ nella console AEM OSGi Bundle.
