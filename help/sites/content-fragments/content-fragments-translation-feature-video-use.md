---
title: Supporto per la traduzione di frammenti di contenuto AEM
description: Scopri come localizzare e tradurre i frammenti di contenuto con Adobe Experience Manager. È inoltre possibile estrarre e tradurre le risorse multimediali diverse associate a un frammento di contenuto.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
jira: KT-201
thumbnail: 18131.jpg
doc-type: Feature Video
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
duration: 223
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 2%

---

# Supporto per la traduzione di frammenti di contenuto AEM {#translation-support-content-fragments}

Scopri come localizzare e tradurre i frammenti di contenuto con Adobe Experience Manager. È inoltre possibile estrarre e tradurre le risorse multimediali diverse associate a un frammento di contenuto.

>[!VIDEO](https://video.tv.adobe.com/v/18131?quality=12&learn=on)

## Casi di utilizzo della traduzione di frammenti di contenuto {#content-fragment-translation-use-cases}

I frammenti di contenuto sono un tipo di contenuto riconosciuto che l’AEM estrae per inviarli a un servizio di traduzione esterno. Sono supportati diversi casi d’uso preconfigurati:

1. Un frammento di contenuto può essere [selezionati direttamente nella console Assets per la copia e la traduzione in lingua](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).
2. I frammenti di contenuto a cui si fa riferimento in una pagina Sites vengono copiati nella cartella della lingua appropriata ed estratti per la traduzione quando la pagina Sites è selezionata per la copia per lingua.
3. Le risorse multimediali in linea incorporate in un frammento di contenuto possono essere estratte e tradotte.
4. Le raccolte di risorse associate a un frammento di contenuto possono essere estratte e tradotte.

## Editor regole di traduzione {#translation-rules-editor}

Il comportamento di traduzione di Experience Manager può essere aggiornato utilizzando **Editor regole di traduzione**. Per aggiornare la traduzione, passa a **Strumenti** > **Generale** > **Configurazione traduzione** a [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Le configurazioni pronte all’uso fanno riferimento a Frammenti di contenuto in `fragmentPath` con un tipo di risorsa `core/wcm/components/contentfragment/v1/contentfragment`. Tutti i componenti che ereditano da `v1/contentfragment` sono riconosciute dalla configurazione predefinita.

![Editor regole di traduzione](assets/translation-configuration.png)
