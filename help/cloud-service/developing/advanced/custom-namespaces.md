---
title: Spazi dei nomi personalizzati
description: Scopri come definire e distribuire namespace personalizzati da AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# Spazi dei nomi personalizzati

Scopri come definire e distribuire i dati personalizzati [spazi dei nomi](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) AEM as a Cloud Service.

Gli spazi dei nomi personalizzati sono la parte facoltativa di una proprietà JCR che precede una `:`. AEM utilizza diversi namespace, ad esempio:

+ `jcr` per le proprietà del sistema JCR
+ `cq` proprietà per AEM (precedentemente nota come Adobe CQ)
+ `dam` per AEM proprietà specifiche delle risorse DAM
+ `dc` per le proprietà Dublin Core

... e molti altri.

I namespace possono essere utilizzati per indicare l’ambito e l’intento di una proprietà. La creazione di uno spazio dei nomi personalizzato, spesso il nome dell’azienda, aiuta a identificare chiaramente i nodi o le proprietà specifiche dell’implementazione AEM e contiene dati specifici della tua azienda.

Gli spazi dei nomi personalizzati sono gestiti in [Inizializzazione archivio Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) script e implementazioni per AEM configurazioni OSGi as a Cloud Service e aggiunte al [del progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it) `ui.config` progetto.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Riferimenti

+ [Documentazione Sling Repository Initialization (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Codice

Il codice seguente viene utilizzato per configurare un `wknd` spazio dei nomi.

### Configurazione di RepositoryInitializer OSGi

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Ciò consente di utilizzare le proprietà personalizzate `wknd` namespace, come indicato come primo parametro dopo la `register namespace` istruzioni, da utilizzare in AEM. Per definizioni di script più avanzate, consulta gli esempi in [Documentazione Sling Repository Initialization (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
