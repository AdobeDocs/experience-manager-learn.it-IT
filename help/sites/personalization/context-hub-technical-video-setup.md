---
title: Setup ContextHub for Personalization with  AEM Sites
description: ContextHub è un framework per la memorizzazione, la manipolazione e la presentazione dei dati contestuali. L'API ContextHub Javascript consente di accedere agli store per creare, aggiornare ed eliminare i dati secondo necessità. ContextHub rappresenta pertanto un livello dati sulle pagine. In questa pagina viene descritto come aggiungere context hub alle pagine del sito AEM.
feature: context-hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 5%

---


# Setup ContextHub for Personalization {#set-up-contexthub}

ContextHub è un framework per la memorizzazione, la manipolazione e la presentazione dei dati contestuali. L&#39;API ContextHub Javascript consente di accedere agli store per creare, aggiornare ed eliminare i dati secondo necessità. ContextHub rappresenta pertanto un livello dati sulle pagine. In questa pagina viene descritto come aggiungere context hub alle pagine del sito AEM.

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>Utilizziamo il sito di riferimento WKND per questo video e non fa parte di AEM release. Puoi scaricare la versione [più recente qui](https://github.com/adobe/aem-guides-wknd/releases).

Aggiungete ContextHub alle pagine per abilitare le funzionalità ContextHub e per collegarvi alle librerie JavaScript ContextHub. L&#39;API JavaScript ContextHub fornisce l&#39;accesso ai dati contestuali gestiti da ContextHub.

## Aggiunta di ContextHub a un componente Pagina {#adding-contexthub-to-a-page-component}

Per abilitare le funzionalità ContextHub e per collegarsi alle librerie JavaScript ContextHub, includi il `contexthub` componente nella `<head>` sezione della pagina Web. Il codice HTL per il componente pagina è simile al seguente esempio:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## Configurazione del sito e segmenti ContextHub {#site-configuration-and-contexthub-segments}

ContextHub include un motore di segmentazione che gestisce i segmenti e determina quali segmenti vengono risolti per il contesto corrente. Sono definiti diversi segmenti. Puoi utilizzare l&#39;API Javascript per [determinare i segmenti](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)risolti. Attiva i segmenti ContextHub per il tuo sito in Browser di configurazione.

## Creazione di segmenti {#create-segments}

Creare segmenti AEM che fungono da regole per i teaser. In altre parole, definiscono quando il contenuto all’interno di un teaser viene visualizzato su una pagina Web. Il contenuto può quindi essere mirato per le esigenze e gli interessi del visitatore, a seconda dei segmenti di corrispondenza.

## Assegnazione della configurazione cloud, del percorso del segmento e del percorso ContextHub al sito {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Assegnazione del percorso di configurazione di Cloud, del percorso di segmentazione e del percorso ContextHub al nodo principale del sito per creare un&#39;esperienza personalizzata per il pubblico. Utilizzando ContextHub, puoi manipolare i dati contestuali e testare i segmenti risolti.

![CRXDE Lite](assets/crx-de-properties.png)

Per ulteriori informazioni su ContextHub e segmentazione, consulta:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Aggiunta di Context Hub alla pagina e accesso agli store](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Segmentazione](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Configurazione della segmentazione con ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
