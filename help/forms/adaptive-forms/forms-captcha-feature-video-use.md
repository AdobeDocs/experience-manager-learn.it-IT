---
title: Utilizzo dei CAPTCHA con AEM Adaptive Forms
description: Aggiunta e utilizzo di un CAPTCHA con AEM Adaptive Forms.
feature: Forms adattivo,Flusso di lavoro
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# Utilizzo dei CAPTCHA con AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

Aggiunta e utilizzo di un CAPTCHA con AEM Adaptive Forms.

Visita la pagina [Esempi AEM Forms](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse1) per un collegamento a una demo in tempo reale di questa funzionalità.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Questo video illustra il processo di aggiunta di un CAPTCHA a un modulo adattivo AEM utilizzando sia il servizio AEM CAPTCHA integrato sia il servizio reCAPTCHA di Google.*

>[!NOTE]
>
>Questa funzione è disponibile solo a partire da AEM 6.3.

>[!NOTE]
>
>**Per configurare reCaptcha sull’istanza di pubblicazione, segui i passaggi**
>
>Configura reCaptach sull&#39;istanza dell&#39;autore
>
>apri la console Web Felix [a1/> sull&#39;istanza dell&#39;autore](http://localhost:4502/system/console/bundles)
>
>cerca il bundle com.adobe.granite.crypto.file
>
>Nota l&#39;id del bundle. Nel mio caso sono 20
>
>Passa all’ID bundle nel file system dell’istanza di authoring
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copia i file HMAC e master

Apri la [console web felix](http://localhost:4502/system/console/bundles) nell&#39;istanza di pubblicazione. Cerca il bundle com.adobe.granite.crypto.file . Nota l&#39;id del bundle
Passa all&#39;id bundle nel file system della tua istanza di pubblicazione
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* eliminare i file HMAC e master esistenti.
* incolla i file HMAC e master copiati dall&#39;istanza di authoring

Riavvia il server di pubblicazione AEM

## Materiali di supporto {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

