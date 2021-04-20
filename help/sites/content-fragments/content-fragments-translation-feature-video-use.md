---
title: Supporto per la traduzione per i frammenti di contenuto AEM
description: Scopri come localizzare e tradurre i frammenti di contenuto con Adobe Experience Manager. È inoltre possibile estrarre e tradurre le risorse multimediali diverse associate a un frammento di contenuto.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: Business Practitioner
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 2%

---


# Supporto per la traduzione per i frammenti di contenuto AEM {#translation-support-content-fragments}

Scopri come localizzare e tradurre i frammenti di contenuto con Adobe Experience Manager. È inoltre possibile estrarre e tradurre le risorse multimediali diverse associate a un frammento di contenuto.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Casi di utilizzo della traduzione dei frammenti di contenuto {#content-fragment-translation-use-cases}

I frammenti di contenuto sono un tipo di contenuto riconosciuto che AEM estrae per essere inviato a un servizio di traduzione esterno. Sono supportati diversi casi d’uso:

1. Un frammento di contenuto può essere selezionato direttamente nella console Risorse per la copia e la traduzione in lingua
2. I frammenti di contenuto a cui si fa riferimento in una pagina Sites vengono copiati nella cartella della lingua appropriata ed estratti per la traduzione quando la pagina Sites viene selezionata per la copia in lingua
3. Le risorse multimediali in linea incorporate in un frammento di contenuto possono essere estratte e tradotte.
4. Le raccolte di risorse associate a un frammento di contenuto possono essere estratte e tradotte

## Editor regole di traduzione {#translation-rules-editor}

Il comportamento di traduzione di Experience Manager può essere aggiornato utilizzando **Editor regole di traduzione**. Per aggiornare la traduzione, passa a **Strumenti** > **Generale** > **Configurazione traduzione** all&#39;indirizzo [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Le configurazioni predefinite fanno riferimento a Frammenti di contenuto in `fragmentPath` con un tipo di risorsa `core/wcm/components/contentfragment/v1/contentfragment`. Tutti i componenti che ereditano da `v1/contentfragment` vengono riconosciuti dalla configurazione predefinita.

![Editor regole di traduzione](assets/translation-configuration.png)
