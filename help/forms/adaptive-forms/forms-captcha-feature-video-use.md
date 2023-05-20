---
title: Utilizzo dei CAPTCHA con AEM Adaptive Forms
description: Aggiunta e utilizzo di un CAPTCHA con AEM Adaptive Forms.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# Utilizzo dei CAPTCHA con AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

Aggiunta e utilizzo di un CAPTCHA con AEM Adaptive Forms.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*Questo video illustra il processo di aggiunta di un CAPTCHA a un modulo adattivo dell’AEM utilizzando sia il servizio integrato AEM CAPTCHA che il servizio Google reCAPTCHA.*

>[!NOTE]
>
>Questa funzione è disponibile solo con AEM 6.3 e versioni successive.

>[!NOTE]
>
>**Per configurare reCaptcha sull’istanza di pubblicazione, segui i passaggi**
>
>Configurare reCaptach nell’istanza di authoring
>
>apri il Felix [console web](http://localhost:4502/system/console/bundles) sull’istanza di authoring
>
>cerca bundle com.adobe.granite.crypto.file
>
>Prendi nota dell’ID bundle. Nella mia istanza è 20
>
>Passa all’ID bundle nel file system dell’istanza di authoring
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copiare i file HMAC e master
>
Apri [console web felix](http://localhost:4502/system/console/bundles) nell’istanza di pubblicazione. Cerca il bundle com.adobe.granite.crypto.file. Nota l’ID del bundle
Passa all’ID bundle nel file system dell’istanza di pubblicazione
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* eliminare i file HMAC e master esistenti.
* incolla i file HMAC e master copiati dall&#39;istanza di authoring
>
Riavvia il server di pubblicazione AEM

## Materiali di supporto {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
