---
title: Credenziali del servizio AEM Forms
description: Scarica le credenziali del servizio dal Developer Console di AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Credenziali del servizio AEM Forms

Le integrazioni con AEM as a Cloud Service devono essere in grado di autenticare in modo sicuro in AEM. Il Developer Console di AEM genera le credenziali del servizio, utilizzate da applicazioni, sistemi e servizi esterni per interagire in modo programmatico con i servizi di authoring o pubblicazione di AEM tramite HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/342223?quality=12&learn=on&captions=ita)

Il file delle credenziali del servizio scaricato viene memorizzato come file di risorse denominato service_token.json nell’eclissi fornita. I valori nel file service_token vengono utilizzati per generare il codice JWT e scambiare il codice JWT con un token di accesso. La classe di utilità GetServiceCredentials viene utilizzata per recuperare i valori delle proprietà dal file di risorse service_token.json.
