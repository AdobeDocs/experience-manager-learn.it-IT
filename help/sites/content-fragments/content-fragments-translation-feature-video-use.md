---
title: Supporto di traduzione per AEM frammenti di contenuto
description: Scopri come i frammenti di contenuto possono essere localizzati e tradotti con Adobe Experience Manager. È inoltre possibile estrarre e convertire le risorse multimediali diverse associate a un frammento di contenuto.
feature: Frammenti di contenuto, Multi Site Manager
topics: Localization
role: Professionista
level: Intermedio
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
translation-type: tm+mt
source-git-commit: 4620acc18a08d71994753903b79247a8ed3fd8f5
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---


# Supporto di traduzione per AEM frammenti di contenuto {#translation-support-content-fragments}

Scopri come i frammenti di contenuto possono essere localizzati e tradotti con Adobe Experience Manager. È inoltre possibile estrarre e convertire le risorse multimediali diverse associate a un frammento di contenuto.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Casi di utilizzo di conversione dei frammenti di contenuto {#content-fragment-translation-use-cases}

I frammenti di contenuto sono un tipo di contenuto riconosciuto che AEM estratti da inviare a un servizio di traduzione esterno. Sono supportati diversi casi di utilizzo:

1. Un frammento di contenuto può essere selezionato direttamente nella console Risorse per la copia e la traduzione in lingua
2. I frammenti di contenuto a cui si fa riferimento in una pagina Siti vengono copiati nella cartella della lingua appropriata ed estratti per la traduzione quando la pagina Siti viene selezionata per la copia della lingua
3. Le risorse multimediali in linea incorporate in un frammento di contenuto possono essere estratte e tradotte.
4. Le raccolte di risorse associate a un frammento di contenuto possono essere estratte e tradotte

## Editor regole di traduzione {#translation-rules-editor}

 comportamento di traduzione di Experience Manager può essere aggiornato utilizzando l&#39;editor **Regole di traduzione**. Per aggiornare la traduzione, andate a **Strumenti** > **Generale** > **Configurazione traduzione** all&#39;indirizzo [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Le configurazioni predefinite fanno riferimento a Frammenti di contenuto in `fragmentPath` con un tipo di risorsa `core/wcm/components/contentfragment/v1/contentfragment`. Tutti i componenti che ereditano da `v1/contentfragment` sono riconosciuti dalla configurazione predefinita.

![Editor regole di traduzione](assets/translation-configuration.png)
