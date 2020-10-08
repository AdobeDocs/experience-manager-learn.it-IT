---
title: Impostazione  progetto Adobe Firefly per l'estensibilità di calcolo risorse
description: I progetti Asset Compute sono progetti  Project Firefly appositamente definiti e, come tali, richiedono l'accesso a  Project Firefly in  Adobe Developer Console per configurarli e distribuirli.
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

I progetti Asset Compute sono progetti  Project Firefly appositamente definiti e, come tali, richiedono l&#39;accesso a  Project Firefly in  Adobe Developer Console per configurarli e distribuirli.

## Creazione e configurazione  progetto Adobe Firefly in  Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Click-through della configurazione  progetto Adobe Firefly (nessun audio)_

1. Accedete a [console](https://console.adobe.io) Sviluppatore di Adobe utilizzando l&#39;Adobe ID  associato agli [account e ai servizi](./accounts-and-services.md)predisposti. Accertatevi di essere un amministratore __di__ sistema o nel ruolo ____ sviluppatore per l&#39;organizzazione  Adobe corretta.
1. Crea un progetto Firefly toccando __Crea nuovo progetto > Progetto da modello > Progetto Firefly__

   _Se il pulsante__ Crea nuovo progetto __o il tipo__ Progetto Firefly __non è disponibile, significa che l&#39;organizzazione  Adobe non è[fornita con Project Firefly](#request-adobe-project-firefly)._

   + __Titolo__ del progetto: `WKND AEM Asset Compute`
   + __Nome__ app: `wkndAemAssetCompute<YourName>`
      + Il nome __dell&#39;__ app deve essere univoco per tutti i progetti Firefly e non può essere modificato in seguito. È consigliabile anteporre il nome della società o dell’organizzazione e postfissaggio con suffisso significativo, ad esempio: `wkndAemAssetCompute`.
      + Per l&#39;autoabilitazione è spesso consigliabile posticipare il nome al nome __dell&#39;__ app, ad esempio `wkndAemAssetComputeJaneDoe` per evitare conflitti con altri progetti di Project Firefly.
   + In __Workspaces__ aggiungere un nuovo ambiente denominato `Development`
   + In __Adobe I/O Runtime__ accertati che sia selezionata l&#39;opzione __Includi runtime con ogni area di lavoro__
   + Toccate __Salva__ per salvare il progetto
1. Nel progetto  Adobe Firefly, selezionate `Development` dal selettore dell&#39;area di lavoro
1. Toccate __+ Aggiungi servizio > API__ per aprire la procedura guidata __Aggiungi API__ . Utilizzate questo approccio per aggiungere le seguenti API:

   + __Experience Cloud  > Calcolo risorsa__
      + Selezionate __Genera una coppia__ di chiavi e toccate il pulsante __Genera coppia__ di chiavi, quindi salvate il download `config.zip` in una posizione sicura per un utilizzo [successivo](#private-key)
      + Tocca __successivo__
      + Selezionate __Integrazioni profilo di prodotto - Cloud Service__ e toccate __Salva API configurata__
   + __Servizi Adobe > Eventi__ I/O e tocca __Salva API configurata__
   + __Adobe Services > I/O Management API__ e toccare __Salva API configurata__

## Accesso a private.key{#private-key}

Quando si configura l&#39;integrazione [API](#set-up) Asset Compute, viene generata una nuova coppia di chiavi e viene automaticamente scaricato un `config.zip` file. Questo `config.zip` contiene il certificato pubblico generato e il `private.key` file corrispondente.

1. Decomprimete `config.zip` il file system in una posizione sicura, in quanto `private.key` verrà [utilizzato in seguito](../develop/environment-variables.md)
   + I segreti e le chiavi private non dovrebbero mai essere aggiunti a Git come una questione di sicurezza.

## Verifica credenziali account di servizio (JWT)

Le credenziali di questo progetto di I/O  Adobe vengono utilizzate dallo strumento [](../develop/development-tool.md) di sviluppo di calcolo risorse locale per interagire con Adobe I/O Runtime e dovranno essere incorporate nel progetto Asset Compute. Acquisisci familiarità con le credenziali di Service Account (JWT).

![credenziali account del servizio per sviluppatori di Adobi](./assets/firefly/service-account.png)

1. Dal progetto I/O di I/O di  Adobe Firefly, accertatevi che l&#39; `Development` area di lavoro sia selezionata
1. Toccate l&#39;account di __servizio (JWT)__ in __Credenziali__
1. Rivedere le credenziali di I/O  Adobe visualizzate
   + La chiave ____ pubblica elencata nella parte inferiore contiene la controparte __private.key__ `config.zip` scaricata quando l&#39;API __di calcolo delle__ risorse è stata aggiunta a questo progetto.
      + Se la chiave privata viene persa o compromessa, è possibile rimuovere la chiave pubblica corrispondente e creare o caricare una nuova coppia di chiavi in  I/O Adobe utilizzando questa interfaccia.
