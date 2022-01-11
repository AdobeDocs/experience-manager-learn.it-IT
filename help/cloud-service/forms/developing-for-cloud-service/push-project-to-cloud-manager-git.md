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
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Push AEM progetto a cloud manager git repo

Nel passaggio precedente abbiamo sincronizzato il nostro progetto AEM con il Forms adattivo e i temi creati nell&#39;istanza AEM.
Ora è necessario aggiungere queste modifiche al nostro archivio Git locale e quindi inviare l’archivio Git locale all’archivio Git di Cloud Manager.
Apri il prompt dei comandi e passa a c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .
```

In questo modo i nuovi file vengono aggiunti al ramo stage dell’archivio Git locale

```
git commit -m "My First AF"
```

In questo modo i file vengono inviati al ramo principale del nostro archivio git locale

```
git push -f bankingapp master:"MyFirstAF"
```

Nel comando di cui sopra stiamo spingendo il nostro ramo principale dal nostro archivio Git locale al ramo MyFirstAF dell&#39;archivio cloud manager identificato dal nome descrittivo delle applicazioni bancarie.
