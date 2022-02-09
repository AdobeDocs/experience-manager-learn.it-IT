---
title: Generazione di documenti tramite API batch in AEM Forms CS
description: Configura e attiva le operazioni batch per generare documenti.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Introduzione

Una richiesta batch è il luogo in cui vengono generati decine, centinaia o migliaia di documenti simili contemporaneamente. Esempio: Una società finanziaria può generare estratti conto della carta di credito da inviare a tutti i suoi clienti.
Le API Batch (API asincrone) sono adatte a casi d’uso pianificati di alta velocità per la generazione di documenti multipli. Queste API generano documenti in batch. Ad esempio, bollette telefoniche, dichiarazioni con carta di credito e dichiarazioni con benefit generate ogni mese.

Per utilizzare l’API di funzionamento batch AEM Forms CS, sono necessarie le seguenti configurazioni

1. Configurare l’account di archiviazione di Azure
1. Crea configurazione cloud con backup di archiviazione di Azure
1. Creare la configurazione dell’archivio dati in batch
1. Eseguire l’API Batch

Si consiglia di acquisire familiarità con il [Documentazione API](https://experienceleague.corp.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) prima di continuare a utilizzare questa esercitazione.




