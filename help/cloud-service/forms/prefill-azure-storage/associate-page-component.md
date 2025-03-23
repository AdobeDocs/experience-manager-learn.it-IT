---
title: Associa il componente Pagina al nuovo modello di modulo adattivo
description: Creare un nuovo componente pagina
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 8%

---

# Associare il componente Pagina al modello

Il passaggio successivo consiste nell’associare il componente Pagina al nuovo modello di modulo adattivo. In questo modo il codice nel componente Pagina viene eseguito ogni volta che viene eseguito il rendering di un modulo adattivo basato sul nuovo modello. Ai fini di questa esercitazione, nella cartella **AzurePortalStorage** è stato creato un nuovo modello di modulo adattivo denominato **StoreAndRestoreFromAzure**.
Passa al nodo /conf/AzurePortalStorage/settings/wcm/templates/storeandrestore efromazure/initial/jcr:content, aggiungi la seguente proprietà e salva le modifiche.

| **Nome proprietà** | **Tipo di proprietà** | **Valore proprietà** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Stringa | azureportalpagecomponent/component/page/storeandfetch |

Passa a /conf/AzurePortalStorage/settings/wcm/templates/storeandrestore efromazure/structure/jcr:content node, aggiungi la seguente proprietà e salva le modifiche.

| **Nome proprietà** | **Tipo di proprietà** | **Valore proprietà** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Stringa | azureportalpagecomponent/component/page/storeandfetch |


## Passaggi successivi

[Creare l’integrazione con l’archiviazione di Azure](./create-fdm.md)
