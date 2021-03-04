---
title: Integrazione di Adobe Experience Manager con Adobe Target tramite Cloud Services
seo-title: Integrazione di Adobe Experience Manager (AEM) con Adobe Target utilizzando i servizi cloud precedenti
description: Procedura dettagliata su come integrare Adobe Experience Manager (AEM) con Adobe Target utilizzando AEM Cloud Service
seo-description: Procedura dettagliata su come integrare Adobe Experience Manager (AEM) con Adobe Target utilizzando AEM Cloud Service
feature: Frammenti di esperienza
topic: Personalizzazione
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 4%

---


# Utilizzo dei servizi cloud legacy di AEM

In questa sezione discuteremo come configurare Adobe Experience Manager (AEM) con Adobe Target utilizzando i servizi cloud precedenti.

>[!NOTE]
>
> Il servizio cloud legacy AEM con Adobe Target è **solo** utilizzato per stabilire una connessione back-end diretta tra AEM Author e Adobe Target, facilitando la pubblicazione del contenuto da AEM a Target. Adobe Launch viene utilizzato per esporre Adobe Target sull’esperienza del sito web pubblico fornita da AEM.

Per utilizzare le offerte dei frammenti esperienza AEM per sviluppare le attività di personalizzazione, puoi passare al capitolo successivo e integrare AEM con Adobe Target utilizzando i servizi cloud legacy. Questa integrazione è necessaria per inviare i frammenti esperienza da AEM a Target come offerte HTML/JSON e per mantenere le offerte target sincronizzate con AEM. Questa integrazione è necessaria per implementare [Scenario 1 discusso nella sezione panoramica](./overview.md#personalization-using-aem-experience-fragment).

## Prerequisiti

* **AEM**

   * Per completare questa esercitazione sono necessarie l’istanza di authoring e pubblicazione di AEM. Se non hai ancora configurato la tua istanza AEM, puoi seguire i passaggi [qui](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud con le seguenti soluzioni
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > È necessario effettuare il provisioning dei clienti con Experience Platform Launch e Adobe I/O da [Supporto Adobe](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) oppure contattare l’amministratore di sistema



### Integrazione di AEM con Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Crea Adobe Target Cloud Service utilizzando l&#39;autenticazione Adobe IMS (*Utilizza l&#39;API di Adobe Target*) (00:34)
2. Ottieni il codice client di Adobe Target (01:50)
3. Creare una configurazione Adobe IMS per Adobe Target (02:08)
4. Crea un account tecnico per l’accesso all’API Target nella console Adobe I/O (02:08)
5. Aggiungere Adobe Target Cloud Service ai frammenti esperienza AEM (04:12)

A questo punto, hai integrato con successo [AEM con Adobe Target utilizzando i servizi cloud precedenti](./using-aem-cloud-services.md#integrating-aem-target-options) come descritto nell&#39;Opzione 2. Ora dovresti essere in grado di creare un frammento esperienza all’interno di AEM, pubblicare il frammento esperienza come offerta HTML o offerta JSON ad Adobe Target e può quindi essere utilizzato per creare un’attività.
