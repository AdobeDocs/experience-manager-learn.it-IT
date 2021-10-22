---
title: Distribuzione in ambiente di sviluppo
description: Distribuire il codice dal ramo dell’archivio cloud manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---


# Distribuzione in ambiente di sviluppo

Nel passaggio precedente abbiamo inviato il nostro ramo principale dal nostro archivio git locale al ramo MyFirstAF dell’archivio cloud manager.

Il passaggio successivo consiste nell&#39;implementare il codice nell&#39;ambiente di sviluppo.
Accedi a cloud manager e seleziona il tuo programma

Seleziona Distribuisci a sviluppo come mostrato di seguito


![primo passo](assets/deploy-first-step1.png)


Seleziona la pipeline di distribuzione come mostrato
![primo passo](assets/deploy1.png)

Selezionare il codice sorgente e il ramo Git appropriato
![primo passo](assets/deploy2.png)
Assicurati di aggiornare le modifiche

Eseguire la pipeline
![conduttura](assets/run-pipeline.png)

Una volta distribuito il codice, dovresti vedere le modifiche nell’istanza di servizio cloud di AEM Forms.
