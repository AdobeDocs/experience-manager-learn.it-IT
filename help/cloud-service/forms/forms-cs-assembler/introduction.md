---
title: Manipolazione di PDF in Forms CS utilizzando l'operazione di richiamo DDX
description: Assembla i file PDF utilizzando invoke DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# Introduzione

In questo corso utilizzeremo la manipolazione e l’archiviazione PDF dei documenti PDF tramite Forms CS. Per utilizzare questi microservizi da un&#39;applicazione esterna, procedere come segue:

1. Genera le credenziali del servizio per un account tecnico AEM
1. Crea un JSON Web Token (JWT) dalle credenziali del servizio e scambialo per un token di accesso
1. Configurare l’accesso per l’account tecnico in AEM
1. Effettuare chiamate HTTP utilizzando il token di accesso per manipolare i file PDF/generare e convalidare i file PDF/A
