---
title: Utilizzo dei CAPTCHA con AEM Adaptive Forms
description: Aggiunta e utilizzo di un CAPTCHA con AEM Adaptive Forms.
feature: Adaptive Forms,Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 260
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Utilizzo dei CAPTCHA con AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

Aggiunta e utilizzo di un CAPTCHA con AEM Adaptive Forms.

>[!VIDEO](https://video.tv.adobe.com/v/327009?quality=12&learn=on&captions=ita)

*Questo video illustra il processo di aggiunta di un CAPTCHA a un modulo adattivo di AEM utilizzando sia il servizio AEM CAPTCHA integrato che il servizio Google reCAPTCHA.*

>[!NOTE]
>
>Questa funzione è disponibile solo con AEM 6.3 e versioni successive.

>[!NOTE]
>
>**Per configurare reCaptcha nell&#39;istanza di pubblicazione, eseguire la procedura seguente**
>
>Configurare reCaptach nell’istanza di authoring
>
>apri la [console web](http://localhost:4502/system/console/bundles) Felix nell&#39;istanza di authoring
>
>cerca bundle com.adobe.granite.crypto.file
>
>Prendi nota dell’ID bundle. Nella mia istanza è 20
>
>Passa all’ID bundle nel file system dell’istanza di authoring
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* Copiare i file HMAC e master
>
>Apri la [console Web Felix](http://localhost:4502/system/console/bundles) nell&#39;istanza di pubblicazione. Cerca il bundle com.adobe.granite.crypto.file. Nota l’ID del bundle
>
>Passa all’ID bundle nel file system dell’istanza di pubblicazione
>
>* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* eliminare i file HMAC e master esistenti.
>* incolla i file HMAC e master copiati dall&#39;istanza di authoring
>
>Riavvia il server di pubblicazione AEM

## Materiali di supporto {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
