---
title: Spazi dei nomi personalizzati
description: Scopri come definire e distribuire spazi dei nomi personalizzati in AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 520
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# Spazi dei nomi personalizzati

Scopri come definire e implementare gli [namespace](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) all’AEM as a Cloud Service.

Gli spazi dei nomi personalizzati sono parte facoltativa di una proprietà JCR prima di un `:`. L’AEM utilizza diversi namespace, ad esempio:

+ `jcr` per le proprietà del sistema JCR
+ `cq` per le proprietà dell’AEM (precedentemente noto come Adobe CQ)
+ `dam` per le proprietà AEM specifiche delle risorse DAM
+ `dc` per le proprietà Dublin Core

... e molti altri.

Gli spazi dei nomi possono essere utilizzati per indicare l’ambito e l’intento di una proprietà. La creazione di uno spazio dei nomi personalizzato, spesso il nome dell’azienda, consente di identificare chiaramente nodi o proprietà specifici per l’implementazione dell’AEM e contengono dati specifici per la tua azienda.

Gli spazi dei nomi personalizzati vengono gestiti in [Inizializzazione archivio Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) e implementa AEM as a Cloud Service come configurazioni OSGi - e aggiunti al tuo [Progetti AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it) `ui.config` progetto.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Riferimenti

+ [Documentazione sull’inizializzazione dell’archivio Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Codice

Il codice seguente viene utilizzato per configurare un `wknd` spazio dei nomi.

### Configurazione OSGi RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Questo consente di personalizzare le proprietà utilizzando `wknd` dello spazio dei nomi, come indicato come primo parametro dopo il `register namespace` istruzioni per l’uso nell’AEM. Per definizioni di script più avanzate, consulta gli esempi in [Documentazione sull’inizializzazione dell’archivio Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
