---
title: Generazione di documenti tramite API batch in AEM Forms CS
description: Configurare e attivare operazioni batch per generare documenti.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 14%

---

# Introduzione

Una richiesta batch è il luogo in cui vengono generati decine, centinaia o migliaia di documenti simili contemporaneamente. Esempio: una società finanziaria può generare estratti conto della carta di credito da inviare a tutti i propri clienti.
Le API in batch (API asincrone) sono adatte per casi di utilizzo pianificati di generazione di documenti con throughput elevato. Queste API generano documenti in batch. Ad esempio, bollette telefoniche, rendiconti per carta di credito e rendiconti dei benefit generati ogni mese.

Per utilizzare l’API dell’operazione in batch di AEM Forms CS, sono necessarie le seguenti configurazioni

1. Configurare l’account di archiviazione Azure
1. Crea configurazione cloud di archiviazione di Azure supportata
1. Crea configurazione archivio dati batch
1. Eseguire l’API Batch

Prima di continuare a utilizzare questa esercitazione, è consigliabile familiarizzare con la [documentazione API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en).
