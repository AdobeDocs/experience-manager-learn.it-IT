---
title: Implementazione nell’ambiente di sviluppo
description: Distribuire il codice dal ramo dell’archivio di Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Code Deployment
jira: KT-8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
duration: 23
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# Implementazione nell’ambiente di sviluppo

Nel passaggio precedente, il ramo principale è stato inviato dall’archivio Git locale al ramo MyFirstAF dell’archivio di Cloud Manager.

Il passaggio successivo consiste nel distribuire il codice nell’ambiente di sviluppo.
Accedi a cloud manager e seleziona il programma

Seleziona la Distribuzione da sviluppare come mostrato di seguito


![primo passaggio](assets/deploy-first-step1.png)


Seleziona la pipeline di implementazione come mostrato
![primo passaggio](assets/deploy1.png)

Seleziona il codice sorgente e il ramo Git appropriato
![primo passaggio](assets/deploy2.png)
Assicurati di aggiornare le modifiche

Eseguire la pipeline
![run-pipeline](assets/run-pipeline.png)

Una volta distribuito il codice, dovresti vedere le modifiche nell’istanza del servizio cloud di AEM Forms.

## Passaggi successivi

[Aggiornamento del progetto dell’archetipo Maven](./updating-project-archetype.md)
