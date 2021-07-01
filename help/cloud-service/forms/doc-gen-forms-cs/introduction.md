---
title: Microservizi per la generazione di documenti in AEM Forms CS
description: Utilizzare i microservizi di generazione dei documenti da un'applicazione esterna.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Servizi documentali
topic: Sviluppo
kt: 7432
thumbnail: 332439.jpg
source-git-commit: 33cb3d18b744d9a8e54a87152079b42ed09212f2
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 4%

---

# Introduzione

In questo corso, utilizzeremo i microservizi di generazione dei documenti per generare un pdf unendo i dati con un modello XDP. Per utilizzare questi microservizi da un&#39;applicazione esterna, procedere come segue:

1. Genera le credenziali del servizio per un account tecnico AEM
1. Crea un JSON Web Token (JWT) dalle credenziali del servizio e scambialo per un token di accesso
1. Configurare l’accesso per l’account tecnico in AEM
1. Effettuare chiamate HTTP utilizzando il token di accesso
