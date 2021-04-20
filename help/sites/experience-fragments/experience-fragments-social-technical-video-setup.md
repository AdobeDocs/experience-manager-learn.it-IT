---
title: Configurare la pubblicazione social con frammenti esperienza AEM
description: I frammenti esperienza consentono agli addetti al marketing di pubblicare esperienze create in AEM su piattaforme di social media. Il video seguente descrive la configurazione e la configurazione necessarie per pubblicare Frammenti esperienza su Facebook e Pinterest.
sub-product: siti, content-services
feature: Experience Fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Content Management
role: Administrator, Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 2%

---


# Configurare la pubblicazione social con frammenti esperienza {#set-up-social-posting-with-experience-fragments}

I frammenti esperienza consentono agli addetti al marketing di pubblicare esperienze create in AEM su piattaforme di social media. Il video seguente descrive la configurazione e la configurazione necessarie per pubblicare Frammenti esperienza su Facebook e Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Frammenti esperienza] : configurazione e configurazione per il post social su Facebook e Pinterest*

## Elenco di controllo per la configurazione dei frammenti esperienza da pubblicare su Facebook e Pinterest

1. L’istanza di authoring di AEM è in esecuzione su HTTPS
2. Account Facebook + app per sviluppatori Facebook
3. Account Pinterest + App per sviluppatori Pinterest
4. [!UICONTROL Configurazione di AEM Cloud ] Services - Facebook
5. [!UICONTROL Configurazione di AEM Cloud ] Services - Pinterest
6. Frammento esperienza AEM con Cloud Services per Facebook + Pinterest
7. Variante di frammento esperienza utilizzando il modello di Facebook
8. Variazione di frammento esperienza con il modello Pinterest

## URI di reindirizzamento per frammento esperienza

Questo URI viene utilizzato per le app Facebook e Pinterest come parte del flusso Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

