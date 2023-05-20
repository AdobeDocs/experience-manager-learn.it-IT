---
title: Creazione di un Cloud Service di configurazione cloud Acrobat Sign
description: Crea l’integrazione tra AEM Forms e Acrobat Sign utilizzando la configurazione dei servizi cloud.
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

# Crea configurazione cloud Acrobat Sign

La configurazione dei servizi cloud in AEM consente di creare un’integrazione tra AEM e altre applicazioni cloud.

Il video seguente illustra i passaggi necessari per creare la configurazione dei servizi cloud per integrare l’AEM con Acrobat Sign

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Risoluzione dei problemi

Se ricevi un errore durante la configurazione della configurazione cloud di Adobe Sign, puoi effettuare le seguenti operazioni per risolvere i problemi
* Assicurati che l’URL di reindirizzamento specificato nell’applicazione API Acrobat Sign sia nel seguente formato
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Ad esempio: https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS è il nome del contenitore che conterrà la configurazione cloud
* Verifica che l’URL di OAuth sia corretto
* Verifica l&#39;ID client e il segreto client
* Prova la modalità finestra in incognito

