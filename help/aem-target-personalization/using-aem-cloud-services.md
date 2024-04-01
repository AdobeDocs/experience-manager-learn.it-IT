---
title: Integrazione di Adobe Experience Manager con Adobe Target tramite Cloud Service
description: Procedura dettagliata su come integrare Adobe Experience Manager (AEM) con Adobe Target tramite AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 357
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 1%

---

# Utilizzo dei Cloud Service legacy dell’AEM

In questa sezione verrà illustrato come configurare Adobe Experience Manager (AEM) con Adobe Target utilizzando Cloud Service legacy.

>[!NOTE]
>
> Il Cloud Service legacy AEM con Adobe Target è **solo** utilizzato per stabilire una connessione back-end diretta da AEM Author ad Adobe Target che faciliti la pubblicazione di contenuti dall’AEM a Target. I tag in Adobe Experience Platform vengono utilizzati per esporre Adobe Target sull’esperienza del sito web rivolto al pubblico fornito dall’AEM.

Per utilizzare le offerte dei frammenti di esperienza AEM per potenziare le attività di personalizzazione, procedi al capitolo successivo e integra l’AEM con Adobe Target utilizzando i servizi cloud precedenti. Questa integrazione è necessaria per inviare i frammenti di esperienza dall’AEM a Target come offerte HTML/JSON e mantenere la sincronizzazione tra le offerte di Target e l’AEM. Questa integrazione è necessaria per implementare [Scenario 1 discusso nella sezione panoramica](./overview.md#personalization-using-aem-experience-fragment).

## Prerequisiti

* **AEM**

   * Per completare questa esercitazione sono necessarie l’istanza di authoring e pubblicazione di AEM. Se non hai ancora configurato l’istanza AEM, puoi seguire i passaggi seguenti [qui](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Accesso al Adobe Experience Cloud delle organizzazioni - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > È necessario fornire al cliente la raccolta dati e l’Adobe I/O da [Supporto Adobe](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) o contattare l&#39;amministratore di sistema

### Integrazione dell’AEM con Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Creazione di un Cloud Service Adobe Target tramite autenticazione IMS di Adobe (*Utilizza l’API di Adobe Target*) (00:34)
2. Ottenere il codice client di Adobe Target (01:50)
3. Creare una configurazione Adobe IMS per Adobe Target (02:08)
4. Creare un account tecnico per accedere all’API di Target nella console Adobe I/O (02:08)
5. Aggiungere un Cloud Service Adobe Target ai frammenti esperienza AEM (04:12)

A questo punto, hai integrato correttamente [AEM con Adobe Target utilizzando Cloud Service legacy](./using-aem-cloud-services.md#integrating-aem-target-options) come descritto nell&#39;opzione 2. Ora dovresti essere in grado di creare un frammento di esperienza in AEM e pubblicare il frammento di esperienza come offerta HTML o offerta JSON in Adobe Target, per poi utilizzarlo per creare un’attività.
