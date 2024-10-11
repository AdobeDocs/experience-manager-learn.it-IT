---
title: Utilizzo del sistema di stili in AEM Forms
description: Creare varianti di stile per il componente pulsante
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# Introduzione

Il sistema di stili di Adobe Experience Manager (AEM) consente agli utenti di creare più varianti visive di un componente e quindi di selezionare lo stile da utilizzare per la creazione di un modulo. Questo rende i componenti più flessibili e riutilizzabili, senza la necessità di creare componenti personalizzati per ogni stile.

Questo articolo ti aiuterà a creare varianti del componente pulsante e a testare le varianti nell’ambiente locale predisposto per il cloud prima di inviare le modifiche all’istanza cloud utilizzando Cloud Manager.

La schermata mostra le 2 varianti di stile per il componente pulsante disponibile per l’autore del modulo.


![button-variables](assets/button-variations.png)

## Prerequisiti

* Istanza AEM Forms cloud ready con i componenti core.
* Clonazione di un tema: è necessario avere familiarità con la clonazione di un tema. Ai fini di questa esercitazione abbiamo clonato il [tema del cavalletto](https://github.com/adobe/aem-forms-theme-easel). Puoi clonare qualsiasi tema disponibile in base alle tue esigenze.

* Installa la versione più recente di Apache Maven. Apache Maven è uno strumento di automazione della build comunemente utilizzato per i progetti Java™. L’installazione della versione più recente garantisce che tu disponga delle dipendenze necessarie per la personalizzazione del tema.
* Installa un editor di testo normale. Microsoft® Visual Studio Code. L&#39;utilizzo di un editor di testo normale come Microsoft® Visual Studio Code fornisce un ambiente semplice per la modifica e la modifica dei file dei temi.



## Passaggi successivi

[Crea criterio di stile](./style-policy.md)
