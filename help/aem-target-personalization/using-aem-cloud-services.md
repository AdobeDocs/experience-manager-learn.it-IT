---
title: Integrazione di Adobe Experience Manager con Adobe Target utilizzando i Cloud Services
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: Procedura dettagliata su come integrare Adobe Experience Manager (AEM) con Adobe Target utilizzando AEM Cloud Service
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 3%

---


# Utilizzo AEM Cloud Services legacy

In questa sezione verrà illustrato come configurare Adobe Experience Manager (AEM) con Adobe Target utilizzando Cloud Services legacy.

>[!NOTE]
>
> Il Cloud Service legacy AEM con Adobe Target è **solo** utilizzato per stabilire una connessione back-end diretta tra AEM Author e Adobe Target, facilitando la pubblicazione del contenuto da AEM a Target. Adobe Launch viene utilizzato per esporre Adobe Target nell’esperienza del sito web pubblico fornita da AEM.

Per utilizzare AEM offerte di Frammenti esperienza per abilitare le attività di personalizzazione, puoi passare al capitolo successivo e integrare AEM con Adobe Target utilizzando i servizi cloud legacy. Questa integrazione è necessaria per inviare i frammenti esperienza da AEM a Target come offerte HTML/JSON e per mantenere le offerte target sincronizzate con AEM. Questa integrazione è necessaria per implementare [Scenario 1 discusso nella sezione panoramica](./overview.md#personalization-using-aem-experience-fragment).

## Prerequisiti

* **AEM**

   * Per completare questa esercitazione sono necessarie AEM’istanza di authoring e pubblicazione. Se non hai ancora configurato la tua istanza AEM, puoi seguire i passaggi [qui](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > È necessario effettuare il provisioning del cliente con Experience Platform Launch ed Adobe I/O dal [supporto per l&#39;Adobe](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) o contattare l&#39;amministratore di sistema


### Integrazione di AEM con Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Crea un Cloud Service Adobe Target utilizzando l&#39;autenticazione IMS di Adobe (*utilizza l&#39;API Adobe Target*) (00:34)
2. Ottenere il codice client Adobe Target (01:50)
3. Crea configurazione Adobe IMS per Adobe Target (02:08)
4. Crea un account tecnico per l’accesso all’API Target nella console Adobe I/O (02:08)
5. Aggiungere un Cloud Service Adobe Target a AEM frammenti esperienza (04:12)

A questo punto, hai integrato con successo [AEM con Adobe Target utilizzando Cloud Services legacy](./using-aem-cloud-services.md#integrating-aem-target-options) come descritto nell&#39;Opzione 2. Ora dovresti essere in grado di creare un frammento esperienza in AEM, pubblicare il frammento esperienza come offerta HTML o offerta JSON ad Adobe Target e può quindi essere utilizzato per creare un’attività.
