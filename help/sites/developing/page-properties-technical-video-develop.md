---
title: Estensione delle proprietà di pagina in AEM Sites
description: Scopri come estendere i campi di metadati delle Proprietà pagina in Adobe Experience Manager Sites. Questo video illustra il modo più efficace per farlo utilizzando le funzioni di Sling Resource Merger.
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# Estensione delle proprietà di pagina {#extending-page-properties-in-aem-sites}

La personalizzazione dei campi di metadati per le Proprietà pagina è un requisito comune in qualsiasi implementazione di Sites. Questo video illustra il modo più efficace per farlo utilizzando le funzioni di Sling Resource Merger.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

Il video precedente mostra come personalizzare le proprietà di pagina per il [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd).

## Pacchetto di proprietà pagina WKND di esempio

Puoi utilizzare il fornito [pacchetto di esempio delle proprietà della pagina WKND](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) contenente **WKND** e **Base** le personalizzazioni delle schede mostrate nel video precedente. Il **Social media** la personalizzazione della scheda non viene fornita come [Componente pagina WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) ora utilizza la versione V3 dei Componenti core WCM e nella versione V3 la [la condivisione social è obsoleta](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Tuttavia, a scopo di apprendimento, è possibile puntare il componente Pagina WKND alla versione V2 dei Componenti core WCM utilizzando `sling:resourceSuperType` valore della proprietà e sovrapporre [Social media](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) scheda. Per ulteriori informazioni, consulta [Configurazione delle proprietà della pagina](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Questo pacchetto di esempio deve essere installato sull’SDK AEM locale o sull’istanza AEM 6.X.X a scopo di apprendimento.
