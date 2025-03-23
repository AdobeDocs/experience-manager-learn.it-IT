---
title: Sviluppo di ambiti OAuth in AEM
description: Gli ambiti OAuth estensibili di Adobe Experience Manager consentono il controllo degli accessi per le risorse da un'applicazione client autorizzata da un utente finale. Il diagramma seguente illustra il flusso delle richieste nel contesto di AEM.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 1%

---

# Sviluppo di ambiti OAuth

Gli ambiti OAuth estensibili di Adobe Experience Manager consentono il controllo degli accessi per le risorse da un&#39;applicazione client autorizzata da un utente finale. Il diagramma seguente illustra il flusso delle richieste nel contesto di AEM.

![Flusso Ambiti Oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM fornisce tre ambiti:

* Profilo
* Accesso offline
* Replica

Gli ambiti OAuth estensibili di AEM consentono di definire altri ambiti personalizzati. Ad esempio, è possibile sviluppare e distribuire in AEM un ambito personalizzato che consenta di limitare la lettura di un’app mobile autorizzata tramite OAuth, ma non la scrittura di risorse.

OAuth è il metodo preferito per autorizzare un’applicazione client in quanto utilizza un token di accesso invece di richiedere che le credenziali di un utente AEM vengano fornite a tale applicazione.

* [Visualizza il codice](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
