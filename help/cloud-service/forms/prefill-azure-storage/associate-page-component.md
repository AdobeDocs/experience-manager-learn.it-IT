---
title: Associa il componente Pagina al nuovo modello di modulo adattivo
description: Creare un nuovo componente pagina
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 5%

---

# Associare il componente Pagina al modello

Il passaggio successivo consiste nell’associare il componente Pagina al nuovo modello di modulo adattivo. In questo modo il codice nel componente Pagina viene eseguito ogni volta che viene eseguito il rendering di un modulo adattivo basato sul nuovo modello. Ai fini del presente tutorial, viene utilizzato un nuovo modello di modulo adattivo denominato **StoreAndRestoreFromAzure** è stato creato in **AzurePortalStorage** cartella.
Passa al nodo /conf/AzurePortalStorage/settings/wcm/templates/storeandrestore efromazure/initial/jcr:content, aggiungi la seguente proprietà e salva le modifiche.

| **Nome proprietà** | **Tipo di proprietà** | **Valore proprietà** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Stringa | azureportalpagecomponent/component/page/storeandfetch |

Passa a /conf/AzurePortalStorage/settings/wcm/templates/storeandrestore efromazure/structure/jcr:content node, aggiungi la seguente proprietà e salva le modifiche.
| **Nome proprietà**  | **Tipo di proprietà** | **Valore proprietà**                                    | --------------------|-------------------|-------------------------------------------------------| | sling:resourceType Stringa | | azureportalpagecomponent/component/page/storeandfetch |


## Passaggi successivi

[Creare l’integrazione con l’archiviazione di Azure](./create-fdm.md)
