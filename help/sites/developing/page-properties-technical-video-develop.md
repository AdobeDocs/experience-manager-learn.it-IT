---
title: Estensione delle proprietà di pagina in AEM Sites
description: Scopri come estendere i campi metadati delle Proprietà pagina in Adobe Experience Manager Sites. Questo video illustra il modo più efficace per eseguire questa operazione utilizzando le funzioni di Sling Resource Merger.
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: 94a29a78edff17ec8089f7056dc118fd335ae484
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# Estensione delle proprietà di una pagina {#extending-page-properties-in-aem-sites}

La personalizzazione dei campi di metadati per le Proprietà pagina è un requisito comune in qualsiasi implementazione di Sites. Questo video illustra il modo più efficace per eseguire questa operazione utilizzando le funzioni di Sling Resource Merger.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=9&learn=on)

Il video precedente mostra la personalizzazione delle proprietà della pagina per [Sito di riferimento WKND](https://github.com/adobe/aem-guides-wknd).

## Esempio di pacchetto di proprietà della pagina WKND

Puoi utilizzare i [pacchetto di proprietà della pagina WKND di esempio](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) contenente **WKND** e **Base** le personalizzazioni delle schede mostrate nel video precedente. La **SocialMedia** la personalizzazione delle schede non viene fornita come [Componente Pagina WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) ora utilizza la versione V3 dei componenti core WCM e nella versione V3 la [la condivisione social network è obsoleta](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Tuttavia, a scopo di apprendimento, è possibile indirizzare il componente Pagina WKND alla versione V2 dei componenti core WCM utilizzando `sling:resourceSuperType` e sovrapponi il valore della proprietà [Social media](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) scheda . Per ulteriori informazioni, consulta [Configurazione delle proprietà di pagina](https://experienceleague.adobe.com/docs/experience-manager-64/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Questo pacchetto di esempio deve essere installato nell&#39;istanza locale AEM SDK o AEM 6.X.X a scopo di apprendimento.
