---
title: Invia progetto AEM all’archivio di Cloud Manager
description: Invia l’archivio Git locale all’archivio di Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# Invia progetto AEM all’archivio Git di Cloud Manager

Nel passaggio precedente abbiamo sincronizzato il progetto AEM con Adaptive Forms e i temi creati nell’istanza AEM.
Ora è necessario aggiungere queste modifiche all’archivio Git locale e quindi inviare l’archivio Git locale all’archivio Git di Cloud Manager.
Apri il prompt dei comandi e passa a c:\cloudmanager\aem-banking-app
Esegui i seguenti comandi

```
git add .
```

In questo modo i nuovi file vengono aggiunti al ramo stage dell’archivio Git locale

```
git commit -m "My First AF"
```

In questo modo i file vengono inviati al ramo principale dell’archivio Git locale

```
git push -f bankingapp master:"MyFirstAF"
```

Con il comando precedente stiamo spingendo il ramo principale dal nostro archivio Git locale al ramo MyFirstAF dell’archivio di Cloud Manager identificato dal nome descrittivo bankingapp.

## Passaggi successivi

[Implementare il progetto nell’ambiente di sviluppo](./deploy-to-dev-environment.md)
