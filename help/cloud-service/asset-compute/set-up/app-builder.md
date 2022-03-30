---
title: Configurazione di App Builder per l’estensibilità di Asset compute
description: I progetti di Asset compute sono progetti App Builder definiti in modo specifico e, come tali, richiedono l’accesso ad App Builder nell’Adobe Developer Console per configurarli e distribuirli.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# Configurazione di App Builder

I progetti di Asset compute sono progetti App Builder definiti in modo specifico e, come tali, richiedono l’accesso ad App Builder nell’Adobe Developer Console per configurarli e distribuirli.

## Creare e configurare App Builder in Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Click-through della configurazione di App Builder (senza audio)_

1. Accedi a [Console per sviluppatori di Adobe](https://console.adobe.io) utilizzando l’Adobe ID associato al provisioning [conti e servizi](./accounts-and-services.md). Assicurati di __Amministratore di sistema__ o __Ruolo sviluppatore__ per l&#39;organizzazione Adobe corretta.
1. Creare un progetto App Builder toccando __Crea nuovo progetto > Progetto da modello > App Builder__

   _Se__ Crea nuovo progetto __o__ App Builder __Il tipo non è disponibile, significa che l’organizzazione Adobe non è disponibile [con App Builder](#request-adobe-project-app-builder)._

   + __Titolo del progetto__: `WKND AEM Asset Compute`
   + __Nome app__: `wkndAemAssetCompute<YourName>`
      + La __Nome app__ deve essere univoco in tutti i progetti di FApp Builderirefly e non può essere modificato in seguito. Prefissare il nome dell&#39;azienda o dell&#39;organizzazione e inserire un suffisso significativo è un buon approccio, ad esempio: `wkndAemAssetCompute`.
      + Per l&#39;autoabilitazione spesso è meglio rimettere il tuo nome al __Nome app__, quali `wkndAemAssetComputeJaneDoe` per evitare conflitti con altri progetti di App Builder.
   + Sotto __Aree di lavoro__ aggiungi un nuovo ambiente denominato `Development`
   + Sotto __Adobe I/O Runtime__ assicurare __Includi runtime in ogni area di lavoro__ è selezionato
   + Tocca __Salva__ per salvare il progetto
1. Nel progetto App Builder, seleziona `Development` dal selettore dell’area di lavoro
1. Tocca __+ Aggiungi servizio > API__ per aprire __Aggiungere un’API__ utilizza questo approccio guidato per aggiungere le seguenti API:

   + __Experience Cloud > Asset compute__
      + Seleziona __Generare una coppia di chiavi__ e tocca __Genera coppia di chiavi__ e salva il `config.zip` in una posizione sicura per [utilizzo successivo](#private-key)
      + Tocca __Successivo__
      + Selezionare il profilo di prodotto __Integrazioni - Cloud Service__ e toccare __Salva API configurata__
   + __Adobe Services > Eventi I/O__ e toccare __Salva API configurata__
   + __Adobe Services > API di gestione I/O__ e toccare __Salva API configurata__

## Accedi a private.key{#private-key}

Quando si imposta la [Integrazione API di Asset compute](#set-up) è stata generata una nuova coppia di chiavi e `config.zip` il file è stato scaricato automaticamente. Questo `config.zip` contiene il certificato pubblico generato e la corrispondenza `private.key` file.

1. Decomprimi `config.zip` in una posizione sicura sul tuo file system come `private.key` è [utilizzato successivamente](../develop/environment-variables.md)
   + I segreti e le chiavi private non dovrebbero mai essere aggiunti a Git come una questione di sicurezza.

## Rivedere le credenziali dell’account di servizio (JWT)

Le credenziali del progetto di Adobe I/O vengono utilizzate dal [Strumento di sviluppo Asset compute](../develop/development-tool.md) per interagire con Adobe I/O Runtime e dovrà essere incorporato nel progetto Asset compute. Acquisisci familiarità con le credenziali dell’account di servizio (JWT).

![Credenziali account di Adobe Developer Service](./assets/app-builder/service-account.png)

1. Dal progetto Adobe I/O Project App Builder , assicurati che `Development` area di lavoro selezionata
1. Tocca __Account di servizio (JWT)__ sotto __Credenziali__
1. Esamina le credenziali Adobe I/O visualizzate
   + La __chiave pubblica__ in basso ha __private.key__ controparte nel `config.zip` scaricato quando __API Asset compute__ è stato aggiunto a questo progetto.
      + Se la chiave privata viene persa o compromessa, la chiave pubblica corrispondente può essere rimossa e una nuova coppia di chiavi generata in o caricata in Adobe I/O utilizzando questa interfaccia.
