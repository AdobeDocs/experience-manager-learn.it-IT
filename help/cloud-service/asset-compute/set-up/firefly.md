---
title: Imposta  progetto Adobe Firefly per  estensibilità del Asset compute
description: ' progetti di Asset compute sono specifici  progetti Firefly di progetto Adobe e, come tale, richiedono l''accesso a  progetto  Firefly nella  console per sviluppatori di Adobe per configurarli e distribuirli.'
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Imposta  progetto Adobe Firefly

 progetti di Asset compute sono specifici  progetti Firefly di progetto Adobe e, come tale, richiedono l&#39;accesso a  progetto  Firefly nella  console per sviluppatori di Adobe per configurarli e distribuirli.

## Creazione e configurazione  progetto Adobe Firefly in  Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Click-through della configurazione  progetto Adobe Firefly (nessun audio)_

1. Accedete a [ Adobe Developer Console](https://console.adobe.io) utilizzando l&#39;Adobe ID  associato agli account e ai servizi [predisposti](./accounts-and-services.md). Assicurarsi di essere un __amministratore di sistema__ o in __ruolo sviluppatore__ per l&#39;organizzazione  Adobe corretta.
1. Crea un progetto Firefly toccando __Crea nuovo progetto > Progetto da modello > Progetto Firefly__

   _Se__ Create un nuovo __pulsante di progetto o__ Project __Fireflytype non è disponibile, significa che l&#39;organizzazione  Adobe non viene  [fornita con Project Firefly](#request-adobe-project-firefly)._

   + __Titolo__ del progetto:  `WKND AEM Asset Compute`
   + __Nome__ app:  `wkndAemAssetCompute<YourName>`
      + Il __nome app__ deve essere univoco in tutti i progetti Firefly e non può essere modificato in seguito. È consigliabile anteporre il nome della società o dell’organizzazione e postfissaggio con suffisso significativo, ad esempio: `wkndAemAssetCompute`.
      + Per l&#39;autoabilitazione è spesso consigliabile posticipare il nome al __nome app__, ad esempio `wkndAemAssetComputeJaneDoe` per evitare conflitti con altri progetti di Project Firefly.
   + In __Workspaces__ aggiungere un nuovo ambiente denominato `Development`
   + In __Adobe I/O Runtime__ assicurarsi che l&#39;opzione __Includi runtime con ogni area di lavoro__ sia selezionata
   + Toccate __Salva__ per salvare il progetto
1. Nel progetto Firefly Adobe , selezionate `Development` dal selettore dell&#39;area di lavoro
1. Toccate __+ Aggiungi servizio > API__ per aprire la procedura guidata __Aggiungi un&#39;API__, utilizzate questo approccio per aggiungere le seguenti API:

   + __Experience Cloud  > Asset compute__
      + Selezionare __Generare una coppia di chiavi__ e toccare il pulsante __Genera coppia di chiavi__, quindi salvare il `config.zip` scaricato in una posizione sicura per [utilizzare in un secondo momento](#private-key)
      + Toccare __Next__
      + Selezionate il profilo di prodotto __Integrazioni - Cloud Service__ e toccate __Salva API configurata__
   + __Adobe Services > I/O__ Events e tocca  __Salva API configurata__
   + __Adobe Services >__ API di gestione I/O e tocca  __Salva API configurata__

## Accedi a private.key{#private-key}

Quando si configura l&#39; [ integrazione API Asset compute](#set-up) è stata generata una nuova coppia di chiavi e viene automaticamente scaricato un file `config.zip`. Questo `config.zip` contiene il certificato pubblico generato e il file `private.key` corrispondente.

1. Decomprimete `config.zip` in una posizione sicura nel file system, in quanto la `private.key` è [utilizzata in seguito](../develop/environment-variables.md)
   + I segreti e le chiavi private non dovrebbero mai essere aggiunti a Git come una questione di sicurezza.

## Verifica credenziali account di servizio (JWT)

Le credenziali  progetto Adobe I/O sono utilizzate dallo [ Asset compute Development Tool](../develop/development-tool.md) locale per interagire con Adobe I/O Runtime e dovranno essere incorporate nel progetto di Asset compute . Acquisisci familiarità con le credenziali di Service Account (JWT).

![ credenziali account del servizio per sviluppatori di Adobi](./assets/firefly/service-account.png)

1. Dal  progetto Adobe I/O Project Firefly, verificare che l&#39;area di lavoro `Development` sia selezionata
1. Toccare su __Account di servizio (JWT)__ in __Credenziali__
1. Rivedete  Credenziali Adobe I/O visualizzate
   + La __chiave pubblica__ elencata in fondo ha la __controparte privata.key__ in `config.zip` scaricata quando è stata aggiunta a questo progetto l&#39;__Asset compute API__.
      + Se la chiave privata viene persa o compromessa, è possibile rimuovere la chiave pubblica corrispondente e creare o caricare una nuova coppia di chiavi in Adobe I/O  utilizzando questa interfaccia.
