---
title: Sviluppo di ambiti OAuth in AEM
description: Gli ambiti OAuth estensibili di Adobe Experience Manager consentono il controllo dell'accesso alle risorse da un'applicazione client autorizzata da un utente finale. Il diagramma seguente illustra il flusso di richieste nel contesto di AEM.
version: 6.3, 6.4, 6.5
feature: authentication
topics: authentication, security
activity: develop
audience: developer
doc-type: code
translation-type: tm+mt
source-git-commit: b351a57e6e5be0fe5696dc09842fa77fdd036a27
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---


# Sviluppo di ambiti OAuth

Gli ambiti OAuth estensibili di Adobe Experience Manager consentono il controllo dell&#39;accesso per le risorse da un&#39;applicazione client autorizzata da un utente finale. Il diagramma seguente illustra il flusso di richieste nel contesto di AEM.

![Flusso Oauth Scopes](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM fornisce tre ambiti:

* Profilo
* Accesso offline
* Replica

AEM ambiti OAuth estensibili consentono di definire altri ambiti personalizzati. Ad esempio, un ambito personalizzato può essere sviluppato e distribuito per AEM che consente a un&#39;app mobile autorizzata tramite OAuth di essere limitata alla lettura, ma non alla scrittura di risorse.

OAuth è il metodo preferito per l&#39;autorizzazione di un&#39;applicazione client, in quanto utilizza un token di accesso invece di richiedere che le credenziali di un utente AEM siano fornite a tale applicazione.

* [Visualizzare il codice](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
