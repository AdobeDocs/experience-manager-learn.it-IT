---
title: Creare un progetto di Asset compute per l’estensibilità di Asset compute
description: I progetti di Asset compute sono progetti Node.js, generati utilizzando Adobe I/O CLI, che aderiscono a una determinata struttura e consentono di implementarli in Adobe I/O Runtime e di integrarli con AEM as a Cloud Service.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: 839152aa67ba7ab2929f2c8093bfdc873761a645
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# Creare un progetto di Asset compute

I progetti di Asset compute sono progetti Node.js, generati utilizzando Adobe I/O CLI, che aderiscono a una determinata struttura che consente di implementarli in Adobe I/O Runtime e di integrarli con AEM as a Cloud Service. Un singolo progetto di Asset compute può contenere uno o più processi di lavoro Asset compute, ciascuno dei quali dispone di un punto finale HTTP discreto a cui fa riferimento un profilo di elaborazione as a Cloud Service AEM.

## Generare un progetto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through della generazione di un progetto di Asset compute (nessun audio)_

Utilizza la [Plug-in Asset compute Adobe I/O CLI](../set-up/development-environment.md#aio-cli) per generare un nuovo progetto di Asset compute vuoto.

1. Dalla riga di comando, passa alla cartella in cui si trova il progetto.
1. Dalla riga di comando, esegui `aio app init` per avviare la generazione interattiva di progetti CLI.
   + Questo comando potrebbe generare un browser Web che richiede ad Adobe I/O l&#39;autenticazione. In caso affermativo, fornisci le credenziali Adobi associate al [servizi e prodotti Adobe richiesti](../set-up/accounts-and-services.md). Se non riesci ad accedere, segui [queste istruzioni su come generare un progetto](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleziona organizzazione__
   + Seleziona l’organizzazione di Adobe con AEM as a Cloud Service, App Builder è registrato con
1. __Seleziona un progetto__
   + Individua e seleziona il Progetto. Questa è la [Titolo del progetto](../set-up/app-builder.md) creato dal modello di progetto App Builder, in questo caso `WKND AEM Asset Compute`
1. __Seleziona area di lavoro__
   + Seleziona la `Development` workspace
1. __Adobe I/O di funzionalità dell&#39;app da abilitare per questo progetto? Selezionare i componenti da includere__
   + Seleziona `Actions: Deploy runtime actions`
   + Utilizzare i tasti freccia per selezionare e lo spazio per deselezionare/selezionare e immettere per confermare la selezione
1. __Selezionare il tipo di azioni da generare__
   + Seleziona `DX Asset Compute Worker v1`
   + Utilizzare i tasti freccia da selezionare, lo spazio per deselezionare/selezionare e immettere per confermare la selezione
1. __Come assegnare un nome a questa azione?__
   + Usa il nome predefinito `worker`.
   + Se il progetto contiene più processi di lavoro che eseguono diversi calcoli delle risorse, denominarli in modo semantico

## Genera console.json

Lo strumento di sviluppo richiede un file denominato `console.json` che contiene le credenziali necessarie per la connessione ad Adobe I/O. Questo file viene scaricato dalla console Adobe I/O.

1. Apri l&#39;unità di lavoro Asset compute [Adobe I/O](https://console.adobe.io) progetto
1. Seleziona l’area di lavoro del progetto da scaricare `console.json` credenziali per, in questo caso seleziona `Development`
1. Vai alla directory principale del progetto Adobe I/O e tocca __Scarica tutto__ nell&#39;angolo in alto a destra.
1. Un file viene scaricato come `.json` file con prefisso del progetto e dell’area di lavoro, ad esempio: `wkndAemAssetCompute-81368-Development.json`
1. Puoi effettuare le seguenti operazioni
   + Rinomina il file come `console.json` e spostalo nella directory principale del progetto di lavoro Asset compute. Questo è l’approccio illustrato in questa esercitazione.
   + Spostalo in una cartella arbitraria E fai riferimento a tale cartella dalla tua `.env` file con una voce di configurazione `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Il percorso del file può essere assoluto o relativo alla directory principale del progetto. Esempio:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Oppure
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> NOTA
> Il file contiene le credenziali. Se archivi il file all’interno del progetto, assicurati di aggiungerlo al tuo `.gitignore` file per impedire la condivisione. Lo stesso vale per il `.env` file — Questi file di credenziali non devono essere condivisi o memorizzati in Git.

## Progetto Asset compute su GitHub

Il progetto di Asset compute finale è disponibile su GitHub all’indirizzo:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, ovvero `.env`, `console.json` o `.aio`._
