---
title: Crea Cloud Service di configurazione di Acrobat Sign Cloud
description: Crea l’integrazione AEM Forms e Acrobat Sign utilizzando la configurazione dei servizi cloud.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 7428
thumbnail: 332437.jpg
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# Creare la configurazione di Acrobat Sign Cloud

La configurazione dei servizi cloud in AEM consente di creare l’integrazione tra AEM e altre applicazioni cloud.

Il seguente video illustra i passaggi necessari per creare la configurazione dei servizi cloud per integrare AEM con Acrobat Sign

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Risoluzione dei problemi

Se si verifica un errore durante la configurazione della clonazione cloud di Adobe Sign, è possibile effettuare le seguenti operazioni per risolvere il problema
* Assicurati che l’url di reindirizzamento specificato nell’applicazione API di Acrobat Sign sia nel seguente formato
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Ad esempio: https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS è il nome del contenitore che conterrà la configurazione cloud
* Assicurati che l’URL oAuth sia corretto
* Controlla l&#39;ID client e il segreto client
* Modalità finestra Prova in incognito

