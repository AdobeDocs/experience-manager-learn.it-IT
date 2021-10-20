---
title: Invia progetto AEM all’archivio cloud manager
description: Push dell’archivio Git locale all’archivio cloud manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# Invia progetto AEM a un rappresentante git di cloud manager

Nel passaggio precedente abbiamo sincronizzato il nostro progetto AEM con il Forms adattivo e i temi creati nell&#39;istanza AEM.
Ora è necessario aggiungere queste modifiche al nostro archivio Git locale e quindi inviare l’archivio Git locale all’archivio Git di Cloud Manager

apri il prompt dei comandi e passa a c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .**
```

In questo modo i nuovi file vengono aggiunti al ramo stage dell’archivio Git locale

```
git commit -m "My First AF"
```

In questo modo i file vengono inviati al ramo principale del nostro archivio git locale

```
git push -f bankingapp master:"My First AF"
```

Nel comando di cui sopra stiamo spingendo il nostro ramo principale dal nostro archivio Git locale al ramo My First AF del repository cloud manager identificato dal nome descrittivo bankingapp



