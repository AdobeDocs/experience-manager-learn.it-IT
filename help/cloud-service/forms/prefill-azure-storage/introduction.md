---
title: Implementare le funzionalità di salvataggio e ripristino per un modulo adattivo
description: Scopri come archiviare e recuperare i dati dei moduli adattivi dall’account di archiviazione Azure.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 3%

---

# Introduzione

In questo tutorial verrà implementato un semplice caso d’uso per consentire ai compilatori dei moduli di salvare e riprendere il processo di compilazione dei moduli. Il flusso del caso d’uso è il seguente

* L’utente inizia a compilare un modulo adattivo.
* L’utente salva il modulo e desidera continuare la compilazione del modulo in un secondo momento.
* L&#39;utente riceve una notifica e-mail con un collegamento al modulo salvato.

## Prerequisiti

* Esperienza con AEM Forms CS, in particolare nella creazione di modelli di dati dei moduli.
* Esperienza nell’implementazione del codice tramite Cloud Manager.
* Accesso all’istanza cloud ready di AEM Forms CS.

Per implementare il caso d’uso precedente in AEM Forms CS, è necessario quanto segue

* [Istanza AEM Forms CS cloud ready](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Account del portale di Azure](https://portal.azure.com/)
* [Account SendGrid](https://sendgrid.com/)

### Passaggi successivi

[Crea componente pagina](./page-component.md)
