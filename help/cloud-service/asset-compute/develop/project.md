---
title: Creazione di un progetto Asset Compute per l'estensibilità Asset Compute
description: I progetti Asset Compute sono progetti Node.js, generati utilizzando Adobe I/O CLI, che rispettano una determinata struttura che consente di distribuirli in Adobe I/O Runtime e integrarli in AEM as a Cloud Service.
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# Creazione di un progetto di Asset compute

I progetti Asset Compute sono progetti Node.js, generati utilizzando Adobe I/O CLI, che rispettano una determinata struttura che consente di distribuirli in Adobe I/O Runtime e integrarli in AEM as a Cloud Service. Un singolo progetto di Asset compute può contenere uno o più processi di lavoro di Asset compute, ciascuno con un endpoint HTTP discreto referenziabile da un profilo di elaborazione AEM as a Cloud Service.

## Generare un progetto

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Click-through durante la generazione di un progetto di Asset compute (nessun audio)_

Utilizza il plug-in di Asset compute CLI [Adobe I/O](../set-up/development-environment.md#aio-cli) per generare un nuovo progetto di Asset compute vuoto.

1. Dalla riga di comando, passa alla cartella contenente il progetto.
1. Dalla riga di comando, eseguire `aio app init` per avviare la CLI di generazione del progetto interattivo.
   + Questo comando può generare un browser Web che richiede l&#39;Adobe I/O dell&#39;autenticazione. In caso contrario, fornisci le tue credenziali di Adobe associate ai [servizi e prodotti Adobe richiesti](../set-up/accounts-and-services.md). Se non riesci ad accedere, segui [queste istruzioni su come generare un progetto](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleziona organizzazione__
   + Seleziona l’organizzazione di Adobe con AEM as a Cloud Service; App Builder è registrato con
1. __Seleziona progetto__
   + Individua e seleziona il progetto. Questo è il [Titolo progetto](../set-up/app-builder.md) creato dal modello di progetto App Builder, in questo caso `WKND AEM Asset Compute`
1. __Seleziona Workspace__
   + Seleziona l&#39;area di lavoro `Development`
1. __Quali funzionalità dell&#39;app di Adobe I/O desideri abilitare per questo progetto? Seleziona i componenti da includere__
   + Seleziona `Actions: Deploy runtime actions`
   + Utilizza i tasti freccia per selezionare e inserire lo spazio per deselezionare/selezionare e Invio per confermare la selezione
1. __Selezionare il tipo di azioni da generare__
   + Seleziona `DX Asset Compute Worker v1`
   + Utilizza i tasti freccia per selezionare, spazio per deselezionare/selezionare e Invio per confermare la selezione
1. __Specificare il nome dell&#39;azione.__
   + Utilizzare il nome predefinito `worker`.
   + Se il progetto contiene più processi di lavoro che eseguono calcoli di risorse diversi, denominali semanticamente

## Genera console.json

Lo strumento per sviluppatori richiede un file denominato `console.json` che contenga le credenziali necessarie per connettersi a Adobe I/O. Questo file viene scaricato dalla console Adobe I/O.

1. Apri il progetto [Adobe I/O](https://console.adobe.io) del lavoratore Asset Compute
1. Selezionare l&#39;area di lavoro del progetto per cui scaricare le credenziali `console.json`. In questo caso, selezionare `Development`
1. Vai alla directory principale del progetto di Adobe I/O e tocca __Scarica tutto__ nell&#39;angolo in alto a destra.
1. Un file viene scaricato come file `.json` con il prefisso del progetto e dell&#39;area di lavoro, ad esempio: `wkndAemAssetCompute-81368-Development.json`
1. È possibile:
   + Rinomina il file come `console.json` e spostalo nella directory principale del progetto Asset Compute worker. Questo è l’approccio di questa esercitazione.
   + Spostarlo in una cartella arbitraria E fare riferimento a tale cartella dal file `.env` con una voce di configurazione `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Il percorso del file può essere assoluto o relativo alla directory principale del progetto. Ad esempio:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     Oppure
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> NOTA
> Il file contiene le credenziali. Se il file viene archiviato all&#39;interno del progetto, assicurarsi di aggiungerlo al file `.gitignore` per evitare che venga condiviso. Lo stesso vale per il file `.env`. Questi file di credenziali non devono essere condivisi o archiviati in Git.

## Progetto di Asset compute su GitHub

Il progetto di Asset compute finale è disponibile su GitHub all’indirizzo:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, ovvero `.env`, `console.json` o `.aio`._
