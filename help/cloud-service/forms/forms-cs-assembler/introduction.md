---
title: Manipolazione dei PDF in Forms CS tramite richiamare l’operazione DDX
description: Assemblare file PDF utilizzando DDX di richiamo.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
duration: 32
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# Introduzione

In questo corso utilizzeremo la manipolazione e l’archiviazione PDF dei documenti PDF utilizzando Forms CS. Per utilizzare questi microservizi da un’applicazione esterna, è necessario effettuare le seguenti operazioni:

1. Generare le credenziali del servizio per un account tecnico AEM
1. Creare un token web JSON (JWT) dalle credenziali del servizio e scambiarlo con un token di accesso
1. Configurare l’accesso per l’account tecnico in AEM
1. Effettuare chiamate HTTP utilizzando il token di accesso per manipolare i file PDF/generare e convalidare i file PDF/A
