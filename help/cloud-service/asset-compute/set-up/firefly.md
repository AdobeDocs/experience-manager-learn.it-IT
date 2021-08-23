---
title: Imposta Adobe Project Firefly per l'estensibilità dell'Asset compute
description: I progetti di Asset compute sono progetti Adobe Firefly appositamente definiti e, come tali, richiedono l’accesso ad Adobe Project Firefly in Adobe Developer Console per configurarli e distribuirli.
feature: Microservizi di Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrazioni, Sviluppo
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 0%

---


# Imposta progetto Adobe Firefly

I progetti di Asset compute sono progetti Adobe Firefly appositamente definiti e, come tali, richiedono l’accesso ad Adobe Project Firefly in Adobe Developer Console per configurarli e distribuirli.

## Creare e configurare Adobe Project Firefly in Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Click-through della configurazione di Adobe Project Firefly (nessun audio)_

1. Accedi a [Adobe Developer Console](https://console.adobe.io) utilizzando l&#39;Adobe ID associato agli account e ai servizi [forniti](./accounts-and-services.md). Assicurati di essere un __amministratore di sistema__ o in __Ruolo sviluppatore__ per l&#39;organizzazione Adobe corretta.
1. Crea un progetto Firefly toccando __Crea nuovo progetto > Progetto da modello > Project Firefly__

   _Se non è disponibile__ Crea nuovo __pulsante di progetto o__ Project __Fireflytype, significa che l’organizzazione di Adobe non è  [disponibile con Project Firefly](#request-adobe-project-firefly)._

   + __Titolo__ del progetto:  `WKND AEM Asset Compute`
   + __Nome__ app:  `wkndAemAssetCompute<YourName>`
      + Il __Nome app__ deve essere univoco in tutti i progetti Firefly e non può essere modificato in seguito. Prefissare il nome dell&#39;azienda o dell&#39;organizzazione e inserire un suffisso significativo è un buon approccio, ad esempio: `wkndAemAssetCompute`.
      + Per l’abilitazione automatica, spesso è meglio impostare il nome su __Nome app__, ad esempio `wkndAemAssetComputeJaneDoe` per evitare conflitti con altri progetti Project Firefly.
   + In __Workspace__ aggiungi un nuovo ambiente denominato `Development`
   + In __Adobe I/O Runtime__ assicurarsi che sia selezionato __Includi runtime con ogni area di lavoro__
   + Tocca __Salva__ per salvare il progetto
1. Nel progetto Adobe Firefly, seleziona `Development` dal selettore dell’area di lavoro
1. Tocca __+ Aggiungi servizio > API__ per aprire la procedura guidata __Aggiungi un&#39;API__, utilizza questo approccio per aggiungere le seguenti API:

   + __Experience Cloud > Asset compute__
      + Seleziona __Genera una coppia di chiavi__ e tocca il pulsante __Genera coppia di chiavi__, quindi salva il `config.zip` scaricato in una posizione sicura per [uso successivo](#private-key)
      + Tocca __Avanti__
      + Seleziona il profilo di prodotto __Integrazioni - Cloud Service__ e tocca __Salva API configurata__
   + __Adobe Services >__ Eventi I/O e tocca  __Salva API configurata__
   + __Adobe Services >__ API di gestione I/O e tocca  __Salva API configurata__

## Accedi a private.key{#private-key}

Quando si imposta l&#39; [integrazione API di Asset compute](#set-up) è stata generata una nuova coppia di chiavi e viene scaricato automaticamente un file `config.zip` . Questo `config.zip` contiene il certificato pubblico generato e il file corrispondente `private.key`.

1. Decomprimi `config.zip` in un punto sicuro del file system, in quanto `private.key` è [utilizzato in seguito](../develop/environment-variables.md)
   + I segreti e le chiavi private non dovrebbero mai essere aggiunti a Git come una questione di sicurezza.

## Rivedere le credenziali dell’account di servizio (JWT)

Le credenziali di questo progetto di Adobe I/O sono utilizzate dallo [strumento di sviluppo Asset compute](../develop/development-tool.md) locale per interagire con Adobe I/O Runtime e dovranno essere incorporate nel progetto di Asset compute. Acquisisci familiarità con le credenziali dell’account di servizio (JWT).

![Credenziali account di Adobe Developer Service](./assets/firefly/service-account.png)

1. Dal progetto Adobe I/O Project Firefly , accertati che sia selezionata l’area di lavoro `Development`
1. Tocca su __Account di servizio (JWT)__ in __Credenziali__
1. Esamina le credenziali Adobe I/O visualizzate
   + La __chiave pubblica__ elencata in basso ha la sua controparte __private.key__ nel `config.zip` scaricato quando __API Asset compute__ è stato aggiunto a questo progetto.
      + Se la chiave privata viene persa o compromessa, la chiave pubblica corrispondente può essere rimossa e una nuova coppia di chiavi generata in o caricata in Adobe I/O utilizzando questa interfaccia.
