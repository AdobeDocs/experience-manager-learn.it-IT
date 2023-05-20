---
title: Sviluppo di ambiti OAuth nell’AEM
description: Gli ambiti OAuth estensibili di Adobe Experience Manager consentono il controllo degli accessi per le risorse da un'applicazione client autorizzata da un utente finale. Il diagramma seguente illustra il flusso delle richieste nel contesto dell’AEM.
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# Sviluppo di ambiti OAuth

Gli ambiti OAuth estensibili di Adobe Experience Manager consentono il controllo degli accessi per le risorse da un&#39;applicazione client autorizzata da un utente finale. Il diagramma seguente illustra il flusso delle richieste nel contesto dell’AEM.

![Flusso ambiti OAuth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

L’AEM ha tre ambiti:

* Profilo
* Accesso offline
* Replica

Gli ambiti OAuth estensibili dell’AEM consentono di definire altri ambiti personalizzati. Ad esempio, è possibile sviluppare e distribuire all’AEM un ambito personalizzato che consenta di limitare la lettura di un’app mobile autorizzata tramite OAuth, ma non la scrittura di risorse.

OAuth è il metodo preferito per autorizzare un&#39;applicazione client in quanto utilizza un token di accesso invece di richiedere che le credenziali di un utente AEM vengano fornite a tale applicazione.

* [Visualizza il codice](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
