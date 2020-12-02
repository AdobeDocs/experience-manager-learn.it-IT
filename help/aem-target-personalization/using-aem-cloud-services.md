---
title: Integrazione di Adobe Experience Manager con  Adobe Target con Cloud Services
seo-title: Integrazione di Adobe Experience Manager (AEM) con  Adobe Target con Cloud Services precedenti
description: Procedura dettagliata sull'integrazione di Adobe Experience Manager (AEM) con  Adobe Target con AEM Cloud Service
seo-description: Procedura dettagliata sull'integrazione di Adobe Experience Manager (AEM) con  Adobe Target con AEM Cloud Service
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---


# Utilizzo AEM Cloud Services precedenti

In questa sezione verrà illustrato come impostare Adobe Experience Manager (AEM) con  Adobe Target utilizzando Cloud Services legacy.

>[!NOTE]
>
> Il Cloud Service AEM Legacy con  Adobe Target è **solo** utilizzato per stabilire una connessione back-end diretta di AEM Author  Adobe Target, facilitando la pubblicazione di contenuto da AEM a Target.  Adobe Launch viene utilizzato per esporre  Adobe Target sull&#39;esperienza del sito Web pubblico gestita da AEM.

Per utilizzare AEM offerte di frammenti esperienza per sviluppare attività di personalizzazione, puoi passare al capitolo successivo e integrare AEM con  Adobe Target utilizzando i servizi cloud precedenti. Questa integrazione è necessaria per inviare i frammenti esperienza da AEM a Target come offerte HTML/JSON e per mantenere le offerte di destinazione sincronizzate con AEM. Questa integrazione è necessaria per implementare [Scenario 1 descritto nella sezione panoramica](./overview.md#personalization-using-aem-experience-fragment).

## Prerequisiti

* **AEM**

   * AEM’istanza di creazione e pubblicazione è necessaria per completare questa esercitazione. Se non avete ancora impostato l&#39;istanza AEM, potete seguire i passaggi [qui](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - <https://>`<yourcompany>`.experienceecCloud.adobe.com
   *  Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > È necessario effettuare il provisioning del cliente con Experience Platform Launch e Adobe I/O da [ supporto Adobe](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) oppure contattare l&#39;amministratore di sistema



### Integrazione di AEM con  Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Creare  Cloud Service Adobe Target utilizzando  autenticazione IMS Adobe (*Utilizza  API Adobe Target*) (00:34)
2. Ottenere  codice cliente Adobe Target (01:50)
3. Creare  configurazione IMS Adobe per  Adobe Target (02:08)
4. Crea un account tecnico per accedere all&#39;API Target all&#39;interno  console Adobe I/O (02:08)
5. Aggiungere  Cloud Service Adobe Target a AEM frammenti esperienza (04:12)

A questo punto, avete integrato con successo [AEM con  Adobe Target utilizzando Cloud Services legacy](./using-aem-cloud-services.md#integrating-aem-target-options) come descritto nell&#39;opzione 2. A questo punto dovreste essere in grado di creare un frammento esperienza all&#39;interno di AEM, pubblicare il frammento esperienza come offerta HTML o offerta JSON per  Adobe Target, e quindi poter essere utilizzati per creare un&#39;attività.
