---
title: Spazi dei nomi personalizzati
description: Scopri come definire e distribuire spazi dei nomi personalizzati in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 2%

---

# Spazi dei nomi personalizzati

Scopri come definire e distribuire [spazi dei nomi](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html?lang=it) personalizzati in AEM as a Cloud Service.

Gli spazi dei nomi personalizzati sono parte facoltativa di una proprietà JCR prima di un `:`. AEM utilizza diversi spazi dei nomi, ad esempio:

+ `jcr` per le proprietà di sistema JCR
+ `cq` per le proprietà AEM (precedentemente note come Adobe CQ)
+ `dam` per le proprietà AEM specifiche delle risorse DAM
+ `dc` per le proprietà Dublin Core

... e molti altri.

Gli spazi dei nomi possono essere utilizzati per indicare l’ambito e l’intento di una proprietà. La creazione di uno spazio dei nomi personalizzato, spesso il nome dell’azienda, consente di identificare chiaramente nodi o proprietà specifici per l’implementazione di AEM e contengono dati specifici per la tua azienda.

Gli spazi dei nomi personalizzati vengono gestiti negli script [Sling Repository Initialization (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html), distribuiti in AEM as a Cloud Service come configurazioni OSGi e aggiunti al progetto [AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it) `ui.config`.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Riferimenti

+ [Documentazione sull&#39;inizializzazione dell&#39;archivio Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Codice

Il codice seguente viene utilizzato per configurare uno spazio dei nomi `wknd`.

### Configurazione OSGi RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

In questo modo è possibile utilizzare in AEM le proprietà personalizzate che utilizzano lo spazio dei nomi `wknd`, indicato come primo parametro dopo l&#39;istruzione `register namespace`. Per definizioni di script più avanzate, vedere gli esempi nella [documentazione relativa all&#39;inizializzazione dell&#39;archivio Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
