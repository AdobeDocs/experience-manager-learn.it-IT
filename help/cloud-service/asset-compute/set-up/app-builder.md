---
title: Configurare App Builder per l’estensibilità di Asset compute
description: I progetti di Asset compute sono progetti App Builder appositamente definiti e, per configurarli e distribuirli, hanno bisogno dell’accesso ad App Builder in Adobe Developer Console.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 197
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# Configurare App Builder

I progetti di Asset compute sono progetti App Builder appositamente definiti e, per configurarli e distribuirli, hanno bisogno dell’accesso ad App Builder in Adobe Developer Console.

## Creare e configurare App Builder in Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Click-through di configurazione di App Builder (nessun audio)_

1. Accedi a [Adobe Developer Console](https://console.adobe.io) utilizzando l&#39;Adobe ID associato ai [account e servizi](./accounts-and-services.md) per i quali è stato eseguito il provisioning. Assicurati di essere un __Amministratore di sistema__ o nel __Ruolo sviluppatore__ per l&#39;organizzazione di Adobe corretta.
1. Crea un progetto App Builder toccando __Crea nuovo progetto > Progetto da modello > App Builder__

   _Se il pulsante__ Crea nuovo progetto __o il tipo__ App Builder __non è disponibile, significa che per l&#39;organizzazione di Adobe non è stato eseguito il provisioning [con App Builder](#request-adobe-project-app-builder)._

   + __Titolo progetto__: `WKND AEM Asset Compute`
   + __Nome app__: `wkndAemAssetCompute<YourName>`
      + Il __nome app__ deve essere univoco in tutti i progetti FApp Builderirefly e non può essere modificato in un secondo momento. Prefisso il nome della tua azienda o organizzazione e suffisso con un suffisso significativo è un buon approccio, ad esempio: `wkndAemAssetCompute`.
      + Per l&#39;abilitazione automatica, spesso è meglio aggiungere il proprio nome al __nome app__, ad esempio `wkndAemAssetComputeJaneDoe`, per evitare conflitti con altri progetti App Builder.
   + In __Aree di lavoro__ aggiungere un nuovo ambiente denominato `Development`
   + In __Adobe I/O Runtime__ assicurati che sia selezionato __Includi runtime con ogni area di lavoro__
   + Tocca __Salva__ per salvare il progetto
1. Nel progetto App Builder, seleziona `Development` dal selettore dell&#39;area di lavoro
1. Tocca __+ Aggiungi servizio > API__ per aprire la procedura guidata __Aggiungi un&#39;API__. Utilizza questo approccio per aggiungere le seguenti API:

   + __Experience Cloud > Asset compute__
      + Seleziona __Genera una coppia di chiavi__, tocca il pulsante __Genera coppia di chiavi__ e salva `config.zip` scaricato in un percorso sicuro per [uso successivo](#private-key)
      + Tocca __Avanti__
      + Seleziona il profilo di prodotto __Integrazioni - Cloud Service__ e tocca __Salva API configurata__
   + __Adobe Services > Eventi di I/O__ e tocca __Salva API configurata__
   + __Adobe Services > API di gestione I/O__ e tocca __Salva API configurata__

## Accedi a private.key{#private-key}

Durante la configurazione dell&#39;[integrazione Asset Compute API](#set-up) è stata generata una nuova coppia di chiavi ed è stato scaricato automaticamente un file `config.zip`. `config.zip` contiene il certificato pubblico generato e il file `private.key` corrispondente.

1. Decomprimi `config.zip` in un luogo sicuro nel file system perché `private.key` è [utilizzato in seguito](../develop/environment-variables.md)
   + I segreti e le chiavi private non devono mai essere aggiunti a Git per motivi di sicurezza.

## Verifica le credenziali dell’account di servizio (JWT)

Le credenziali di questo progetto di Adobe I/O vengono utilizzate dal [Asset Compute Development Tool](../develop/development-tool.md) locale per interagire con Adobe I/O Runtime e dovranno essere incorporate nel progetto di Asset compute. Acquisisci familiarità con le credenziali dell’account di servizio (JWT).

![Credenziali account servizio Adobe Developer](./assets/app-builder/service-account.png)

1. Nel progetto Adobe I/O Project App Builder, verificare che l&#39;area di lavoro `Development` sia selezionata
1. Tocca __Account di servizio (JWT)__ in __Credenziali__
1. Verifica le credenziali dell’Adobe I/O visualizzate
   + La __chiave pubblica__ elencata in basso ha la sua controparte __privata.key__ in `config.zip` scaricata quando __API Asset Compute__ è stata aggiunta a questo progetto.
      + Se la chiave privata viene persa o compromessa, la chiave pubblica corrispondente può essere rimossa e una nuova coppia di chiavi generata o caricata in Adobe I/O utilizzando questa interfaccia.
