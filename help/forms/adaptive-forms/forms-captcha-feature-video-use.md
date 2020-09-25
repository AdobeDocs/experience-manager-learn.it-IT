---
title: Utilizzo di CAPTCHA con AEM Forms adattivo
seo-title: Utilizzo di CAPTCHA con AEM Forms adattivo
description: Aggiunta e utilizzo di un CAPTCHA con AEM Forms adattivo.
seo-description: Aggiunta e utilizzo di un CAPTCHA con AEM Forms adattivo.
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---


# Utilizzo di CAPTCHA con AEM Forms adattivo{#using-captchas-with-aem-adaptive-forms}

Aggiunta e utilizzo di un CAPTCHA con AEM Forms adattivo.

Visitate la [pagina di esempi](https://forms.enablementadobe.com/content/samples/samples.html?query=0) AEM Forms per un collegamento a una dimostrazione live di questa funzionalità.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Questo video illustra il processo di aggiunta di un CAPTCHA a un AEM modulo adattivo utilizzando sia il servizio AEM CAPTCHA incorporato che il servizio reCAPTCHA di Google.*

>[!NOTE]
>
>Questa funzione è disponibile solo a partire da AEM 6.3.

>[!NOTE]
>
>**Per configurare reCaptcha all’istanza di pubblicazione, seguite i passaggi**
>
>Configurare il recuperoCaptach sull’istanza di creazione
>
>aprire la console [](http://localhost:4502/system/console/bundles) Web felix nell’istanza di creazione
>
>cerca il bundle com.adobe.granite.crypto.file
>
>Prendete nota dell’ID bundle. Nel mio caso è 20
>
>Passate all’ID bundle nel file system dell’istanza di creazione
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copiare i file HMAC e master

Aprite la console [Web](http://localhost:4502/system/console/bundles) felix nell’istanza di pubblicazione. Cercate il bundle com.adobe.granite.crypto.file. Notate l’ID bundle
Andate al bundle id nel file system dell’istanza di pubblicazione
* &lt;direct-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* eliminare i file HMAC e master esistenti.
* incollare i file HMAC e master copiati dall’istanza di creazione

Riavviate il server di pubblicazione AEM

## Materiali di supporto {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

