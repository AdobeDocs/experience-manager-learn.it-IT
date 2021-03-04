---
title: Sviluppo di ambiti OAuth in AEM
description: Gli ambiti OAuth estensibili di Adobe Experience Manager consentono il controllo degli accessi alle risorse da un’applicazione client autorizzata da un utente finale. Il diagramma seguente illustra il flusso di richieste nel contesto di AEM.
version: 6.3, 6.4, 6.5
feature: autenticazione
topics: authentication, security
activity: develop
audience: developer
doc-type: code
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---


# Sviluppo di ambiti OAuth

Gli ambiti OAuth estensibili di Adobe Experience Manager consentono il controllo degli accessi per le risorse da un’applicazione client autorizzata da un utente finale. Il diagramma seguente illustra il flusso di richieste nel contesto di AEM.

![Flusso degli ambiti oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM fornisce tre ambiti:

* Profilo
* Accesso offline
* Replica

Gli ambiti estensibili OAuth di AEM consentono di definire altri ambiti personalizzati. Ad esempio, è possibile sviluppare e distribuire un ambito personalizzato in AEM che consente a un’app mobile autorizzata tramite OAuth di essere limitata alla lettura, ma non alla scrittura di risorse.

OAuth è il metodo preferito per autorizzare un’applicazione client in quanto utilizza un token di accesso invece di richiedere che le credenziali di un utente AEM vengano fornite a tale applicazione.

* [Visualizza il codice](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
