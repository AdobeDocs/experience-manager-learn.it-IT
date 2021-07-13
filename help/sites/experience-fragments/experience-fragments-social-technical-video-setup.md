---
title: Configurare la pubblicazione social con frammenti esperienza AEM
description: I frammenti esperienza consentono agli addetti al marketing di pubblicare esperienze create in AEM sulle piattaforme di social media. Il video seguente descrive la configurazione e la configurazione necessarie per pubblicare Frammenti esperienza in Facebook e Pinterest.
sub-product: siti, content-services
feature: Frammenti di esperienza
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Gestione dei contenuti
role: Admin, Developer
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 2%

---


# Configurare la pubblicazione social con i frammenti esperienza {#set-up-social-posting-with-experience-fragments}

I frammenti esperienza consentono agli addetti al marketing di pubblicare esperienze create in AEM sulle piattaforme di social media. Il video seguente descrive la configurazione e la configurazione necessarie per pubblicare Frammenti esperienza in Facebook e Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Frammenti esperienza] : configurazione e configurazione per pubblicare contenuti social in Facebook e Pinterest*

## Elenco di controllo per la configurazione dei frammenti esperienza da inviare a Facebook e Pinterest

1. L’istanza di authoring di AEM è in esecuzione su HTTPS
2. Account facebook + app per sviluppatori Facebook
3. Account pinterest + app per sviluppatori Pinterest
4. [!UICONTROL Configurazione ] dei servizi cloud AEM - Facebook
5. [!UICONTROL Configurazione ] dei servizi cloud AEM - Pinterest
6. AEM Frammento esperienza con Cloud Services per Facebook + Pinterest
7. Variazione dei frammenti esperienza con il modello Facebook
8. Variazione dei frammenti esperienza con il modello Pinterest

## URI di reindirizzamento per frammento esperienza

Questo URI viene utilizzato per le app Facebook e Pinterest come parte del flusso Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

