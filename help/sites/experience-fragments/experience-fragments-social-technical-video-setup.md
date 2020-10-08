---
title: Impostazione della registrazione tramite social network con AEM frammenti esperienza
description: I frammenti esperienza consentono agli esperti di marketing di pubblicare esperienze create in AEM alle piattaforme di social media. Il video seguente illustra la configurazione e la configurazione necessarie per pubblicare i frammenti esperienza su Facebook e Pinterest.
sub-product: siti, servizi di contenuto
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---


# Impostazione della pubblicazione per social network con i frammenti esperienza {#set-up-social-posting-with-experience-fragments}

I frammenti esperienza consentono agli esperti di marketing di pubblicare esperienze create in AEM alle piattaforme di social media. Il video seguente illustra la configurazione e la configurazione necessarie per pubblicare i frammenti esperienza su Facebook e Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Frammenti]esperienza - Configurazione e configurazione per i post social su Facebook e Pinterest*

## Elenco di controllo per la configurazione di frammenti esperienza da pubblicare su Facebook e Pinterest

1. L&#39;istanza di AEM Author è in esecuzione su HTTPS
2. Account Facebook + app sviluppatore Facebook
3. Account Pinterest + App per sviluppatori Pinterest
4. [!UICONTROL Configurazione dei servizi] AEM Cloud - Facebook
5. [!UICONTROL Configurazione dei servizi] AEM Cloud - Pinterest
6. AEM Frammento esperienza con Cloud Services per Facebook + Pinterest
7. Variazione frammento esperienza tramite il modello Facebook
8. Variazione dei frammenti esperienza utilizzando il modello Pinterest

## URI di reindirizzamento frammento esperienza

Questo URI viene utilizzato per le app Facebook e Pinterest come parte del flusso Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

