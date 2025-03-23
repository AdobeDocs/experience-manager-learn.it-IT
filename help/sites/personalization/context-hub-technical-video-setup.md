---
title: Configurare ContextHub per Personalization con AEM Sites
description: ContextHub è un framework per l’archiviazione, la manipolazione e la presentazione dei dati contestuali. L’API JavaScript di ContextHub consente di accedere agli store per creare, aggiornare ed eliminare i dati in base alle esigenze. ContextHub rappresenta un livello dati sulle pagine. Questa pagina descrive come aggiungere context hub alle pagine del sito AEM.
feature: Context Hub
version: Experience Manager 6.4, Experience Manager 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 357
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 2%

---

# Configurare ContextHub per Personalization {#set-up-contexthub}

ContextHub è un framework per l’archiviazione, la manipolazione e la presentazione dei dati contestuali. L’API JavaScript di ContextHub consente di accedere agli store per creare, aggiornare ed eliminare i dati in base alle esigenze. ContextHub rappresenta un livello dati sulle pagine. Questa pagina descrive come aggiungere context hub alle pagine del sito AEM.

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>Per questo video utilizziamo il sito di riferimento WKND e non fa parte della versione di AEM. Puoi scaricare la [versione più recente qui](https://github.com/adobe/aem-guides-wknd/releases).

Aggiungi ContextHub alle pagine per abilitare le funzioni di ContextHub e per collegare le librerie JavaScript di ContextHub. L’API JavaScript di ContextHub consente di accedere ai dati contestuali gestiti da ContextHub.

## Aggiunta di ContextHub a un componente pagina {#adding-contexthub-to-a-page-component}

Per abilitare le funzionalità di ContextHub e collegare le librerie JavaScript di ContextHub, includi il componente `contexthub` nella sezione `<head>` della pagina Web. Il codice HTL del componente page è simile al seguente esempio:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## Configurazione del sito e segmenti ContextHub {#site-configuration-and-contexthub-segments}

ContextHub include un motore di segmentazione che gestisce i segmenti e determina quali segmenti vengono risolti per il contesto corrente. Sono definiti diversi segmenti. È possibile utilizzare l&#39;API JavaScript per [determinare i segmenti risolti](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). Abilita i segmenti ContextHub per il tuo sito in [[!UICONTROL Browser configurazioni]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=it).

## Creare segmenti {#create-segments}

Crea segmenti di AEM che fungono da regole per i teaser. In altre parole, definiscono quando il contenuto all’interno di un teaser viene visualizzato in una pagina web. Il contenuto può quindi essere mirato in modo specifico per le esigenze e gli interessi del visitatore, a seconda dei segmenti di corrispondenza.

## Assegnazione della configurazione cloud, del percorso del segmento e del percorso ContextHub al sito {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Assegnazione del percorso di configurazione Cloud, del percorso di segmentazione e del percorso ContextHub al nodo principale del sito per creare un’esperienza personalizzata per il pubblico. Utilizzando ContextHub, puoi manipolare i dati contestuali e testare i segmenti risolti.

![CRXDE Lite](assets/crx-de-properties.png)

Di seguito sono disponibili ulteriori informazioni su ContextHub e sulla segmentazione:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Aggiunta di ContextHub alla pagina e accesso agli store](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Segmentazione](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Configurazione della segmentazione con ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
