---
title: Credenziali del servizio AEM
description: Scarica le credenziali del servizio da AEM Console per sviluppatori.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Moduli adattivi
topic: Sviluppo
kt: 8192
thumbnail: 330519.jpg
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 2%

---


# Credenziali del servizio

Le integrazioni con AEM come Cloud Service devono essere in grado di eseguire l’autenticazione in modo sicuro per AEM. AEM’s Developer Console genera le credenziali del servizio, utilizzate da applicazioni, sistemi e servizi esterni per interagire programmaticamente con i servizi di authoring o pubblicazione AEM tramite HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Il file di credenziali del servizio scaricato viene memorizzato come file di risorse denominato service_token.json nell’eclipse fornita. I valori nel file service_token vengono utilizzati per generare il JWT e scambiare il JWT per un token di accesso. La classe di utilità GetServiceCredentials viene utilizzata per recuperare i valori della proprietà dal file di risorse service_token.json.
