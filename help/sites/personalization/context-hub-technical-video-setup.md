---
title: Configurazione di ContextHub per la personalizzazione con AEM Sites
description: ContextHub è un framework per la memorizzazione, la manipolazione e la presentazione dei dati contestuali. L’API Javascript di ContextHub consente di accedere agli archivi per creare, aggiornare ed eliminare i dati in base alle necessità. ContextHub rappresenta un livello di dati sulle pagine. Questa pagina descrive come aggiungere l’hub di contesto alle pagine del sito AEM.
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 3%

---

# Configurazione di ContextHub per la personalizzazione {#set-up-contexthub}

ContextHub è un framework per la memorizzazione, la manipolazione e la presentazione dei dati contestuali. L’API Javascript di ContextHub consente di accedere agli archivi per creare, aggiornare ed eliminare i dati in base alle necessità. ContextHub rappresenta un livello di dati sulle pagine. Questa pagina descrive come aggiungere l’hub di contesto alle pagine del sito AEM.

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>Per questo video utilizziamo il sito di riferimento WKND e non fa parte AEM versione. È possibile scaricare [versione più recente qui](https://github.com/adobe/aem-guides-wknd/releases).

Aggiungi ContextHub alle tue pagine per abilitare le funzionalità di ContextHub e per effettuare il collegamento alle librerie JavaScript di ContextHub. L’API JavaScript ContextHub fornisce l’accesso ai dati contestuali gestiti da ContextHub.

## Aggiunta di ContextHub a un componente pagina {#adding-contexthub-to-a-page-component}

Per abilitare le funzioni ContextHub e per effettuare il collegamento alle librerie JavaScript di ContextHub, includi `contexthub` nel `<head>` della pagina web. Il codice HTL per il componente pagina è simile al seguente esempio:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## Configurazione del sito e segmenti ContextHub {#site-configuration-and-contexthub-segments}

ContextHub include un motore di segmentazione che gestisce i segmenti e determina quali segmenti vengono risolti per il contesto corrente. Sono definiti diversi segmenti. Puoi utilizzare l’API JavaScript per [determinare i segmenti risolti](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). Abilita i segmenti ContextHub per il tuo sito in [[!UICONTROL Browser di configurazione]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=it).

## Creare segmenti {#create-segments}

Crea segmenti AEM che fungono da regole per i teaser. In altre parole, definiscono quando il contenuto all’interno di un teaser viene visualizzato su una pagina web. Il contenuto può quindi essere mirato in modo specifico alle esigenze e agli interessi del visitatore, a seconda dei segmenti a cui corrisponde.

## Assegnazione della configurazione cloud, del percorso del segmento e del percorso ContextHub al sito {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Assegnazione del percorso di configurazione cloud, del percorso di segmentazione e del percorso ContextHub al nodo principale del sito in modo da creare un’esperienza personalizzata per il pubblico. Utilizzando ContextHub, puoi manipolare i dati contestuali e testare i segmenti risolti.

![CRXDE Lite](assets/crx-de-properties.png)

Puoi trovare ulteriori informazioni su ContextHub e la segmentazione di seguito:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Aggiunta di Context Hub alla pagina e accesso ai negozi](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Segmentazione](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Configurazione della segmentazione con ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
