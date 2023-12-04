---
title: Estensibilità dell’interfaccia utente AEM
description: Scopri l’estensibilità dell’interfaccia utente dell’AEM utilizzando App Builder per creare estensioni.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 72
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Estensibilità dell’interfaccia utente AEM

Adobe Experience Manager (AEM) offre una potente interfaccia utente per la creazione di esperienze digitali. Per personalizzare ed estendere l’interfaccia utente, Adobe ha introdotto App Builder. Questo strumento permette agli sviluppatori di migliorare l’esperienza utente senza ricorrere a codifiche complesse tramite JavaScript e React.

App Builder fornisce un livello di implementazione per la creazione di estensioni associate per definire correttamente i punti di estensione nell’AEM. App Builder si integra perfettamente con l’AEM, consentendo l’anteprima e il test in tempo reale. L’implementazione delle modifiche all’AEM è rapida e semplice. Utilizzando App Builder, gli sviluppatori risparmiano tempo e fatica, consentendo una rapida prototipazione e collaborazione con le parti interessate.

## Sviluppare un’estensione dell’interfaccia utente dell’AEM

Le varie interfacce utente di AEM hanno punti di estensione diversi, tuttavia i concetti di base sono gli stessi.

I video e le procedure guidate forniti di seguito mostrano l’utilizzo di un’estensione della Console Frammenti di contenuto per illustrare varie attività. Tuttavia, è importante notare che i concetti descritti possono essere applicati a tutte le estensioni dell’interfaccia utente dell’AEM.

1. [Creare un progetto Adobe Developer Console](./adobe-developer-console-project.md)
1. [Inizializzare un’estensione](./app-initialization.md)
1. [Registrare un&#39;estensione](./extension-registration.md)
1. Implementare un punto di estensione

   I punti di estensione e le relative implementazioni variano in base all’interfaccia utente estesa.

   + [Sviluppare un’estensione dell’interfaccia utente per frammenti di contenuto](./content-fragments/overview.md)

1. [Sviluppare un modale](./modal.md)
1. [Sviluppare un’azione Adobe I/O Runtime](./runtime-action.md)
1. [Verificare un’estensione](./verify.md)
1. [Distribuire un&#39;estensione](./deploy.md)

## Documentazione di Adobe Developer

Adobe Developer contiene informazioni per gli sviluppatori sull’estensibilità dell’interfaccia utente dell’AEM. Rivedi il [Contenuto Adobe Developer per ulteriori dettagli tecnici](https://developer.adobe.com/uix/docs/).
