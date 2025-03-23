---
title: Estensione delle proprietà di pagina in AEM Sites
description: Scopri come estendere i campi di metadati delle Proprietà pagina in Adobe Experience Manager Sites. Questo video illustra il modo più efficace per farlo utilizzando le funzioni di Sling Resource Merger.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Experience Manager as a Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 1%

---

# Estensione delle proprietà di pagina {#extending-page-properties-in-aem-sites}

La personalizzazione dei campi di metadati per le Proprietà pagina è un requisito comune in qualsiasi implementazione di Sites. Questo video illustra il modo più efficace per farlo utilizzando le funzioni di Sling Resource Merger.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

Il video precedente mostra come personalizzare le proprietà di pagina per il [sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd).

## Pacchetto di proprietà pagina WKND di esempio

Puoi utilizzare il [pacchetto di esempio delle proprietà della pagina WKND](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) fornito contenente **WKND** e le personalizzazioni delle schede **Basic** mostrate nel video precedente. La personalizzazione della scheda **SocialMedia** non è stata fornita in quanto [il componente Pagina WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) utilizza ora la versione V3 dei componenti core WCM e nella versione V3 la condivisione social [è obsoleta](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Tuttavia, a scopo di apprendimento, è possibile puntare il componente Pagina WKND alla versione V2 dei Componenti core WCM utilizzando il valore della proprietà `sling:resourceSuperType` e sovrapporre la scheda [Social Media](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95). Per ulteriori informazioni, vedere [Configurazione delle proprietà di pagina](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Questo pacchetto di esempio deve essere installato sull’istanza locale di AEM SDK o AEM 6.X.X a scopo di apprendimento.
