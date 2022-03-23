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
source-git-commit: 136049776140746c61d42ad1496df15a2d226e3a
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 1%

---

# Creare un progetto di Asset compute

I progetti di Asset compute sono progetti Node.js, generati utilizzando Adobe I/O CLI, che aderiscono a una determinata struttura che consente di implementarli in Adobe I/O Runtime e di integrarli con AEM as a Cloud Service. Un singolo progetto di Asset compute può contenere uno o più processi di lavoro Asset compute, ciascuno dei quali dispone di un punto finale HTTP discreto a cui fa riferimento un profilo di elaborazione as a Cloud Service AEM.

## Generare un progetto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through della generazione di un progetto di Asset compute (nessun audio)_

Utilizza la [Plug-in Asset compute Adobe I/O CLI](../set-up/development-environment.md#aio-cli) per generare un nuovo progetto di Asset compute vuoto.

1. Dalla riga di comando, passa alla cartella in cui si trova il progetto.
1. Dalla riga di comando, esegui `aio app init` per avviare la generazione interattiva di progetti CLI.
   + Questo comando potrebbe generare un browser Web che richiede ad Adobe I/O l&#39;autenticazione. In caso affermativo, fornisci le credenziali Adobi associate al [servizi e prodotti Adobe richiesti](../set-up/accounts-and-services.md). Se non riesci ad accedere, segui [queste istruzioni su come generare un progetto](https://www.adobe.io/project-firefly/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleziona organizzazione__
   + Seleziona l&#39;organizzazione Adobe con AEM as a Cloud Service, Project Firefly è registrato con
1. __Seleziona un progetto__
   + Individua e seleziona il Progetto. Questa è la [Titolo del progetto](../set-up/firefly.md) creato dal modello di progetto Firefly, in questo caso `WKND AEM Asset Compute`
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
   + Rinomina il file come `config.json` e spostalo nella directory principale del progetto di lavoro Asset compute. Questo è l’approccio illustrato in questa esercitazione.
   + Spostalo in una cartella arbitraria E fai riferimento a tale cartella dalla tua `.env` file con una voce di configurazione `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Il percorso del file può essere assoluto o relativo alla directory principale del progetto. Ad esempio:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Oppure
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> NOTA
> Il file contiene le credenziali. Se archivi il file all’interno del progetto, assicurati di aggiungerlo al tuo `.gitignore` file per impedire la condivisione. Lo stesso vale per il `.env` file — Questi file di credenziali non devono essere condivisi o memorizzati in Git.

## Analisi dell&#39;anatomia del progetto

Il progetto di Asset compute generato è un progetto Node.js da utilizzare come progetto Firefly di progetto Adobe specializzato. I seguenti elementi strutturali sono idiosincratici al progetto di Asset compute:

+ `/actions` contiene sottocartelle e ogni sottocartella definisce un processo di lavoro Asset compute.
   + `/actions/<worker-name>/index.js` definisce il JavaScript utilizzato per eseguire il lavoro di questo processo di lavoro.
      + Nome della cartella `worker` è un valore predefinito e può essere qualsiasi cosa, purché sia registrato nel `manifest.yml`.
      + È possibile definire più cartelle di lavoro in `/actions` se necessario, tuttavia devono essere registrati nel `manifest.yml`.
+ `/test/asset-compute` contiene le suite di test per ogni lavoratore. Simile al `/actions` cartella `/test/asset-compute` può contenere più sottocartelle, ciascuna corrispondente al processo di lavoro di cui esegue il test.
   + `/test/asset-compute/worker`, che rappresenta una suite di test per un processo di lavoro specifico, contiene sottocartelle che rappresentano un caso di test specifico, insieme all’input del test, ai parametri e all’output previsto.
+ `/build` contiene l&#39;output, i registri e gli artefatti delle esecuzioni di test case di Asset compute.
+ `/manifest.yml` definisce i processi di lavoro Asset compute forniti dal progetto. Per renderle disponibili per AEM as a Cloud Service, è necessario enumerare in questo file ogni implementazione di processo di lavoro.
+ `/console.json` definisce le configurazioni di Adobe I/O
   + Questo file può essere generato/aggiornato utilizzando `aio app use` comando.
+ `/.aio` contiene configurazioni utilizzate dallo strumento aio CLI.
   + Questo file può essere generato/aggiornato utilizzando `aio app use` comando.
+ `/.env` definisce le variabili di ambiente in un `key=value` e contiene segreti che non devono essere condivisi. Per proteggere questi segreti, questo file NON deve essere archiviato in Git e viene ignorato tramite l&#39;impostazione predefinita del progetto `.gitignore` file.
   + Questo file può essere generato/aggiornato utilizzando `aio app use` comando.
   + Le variabili definite in questo file possono essere sostituite da [esportazione di variabili](../deploy/runtime.md) sulla riga di comando.

Per ulteriori dettagli sulla revisione della struttura del progetto, consulta la sezione [Anatomia di un progetto Adobe Progetto Firefly](https://www.adobe.io/project-firefly/docs/guides/).

La maggior parte dello sviluppo avviene nella `/actions` cartella in cui vengono sviluppate le implementazioni dei processi di lavoro e `/test/asset-compute` creazione di test per i processi di lavoro di Asset compute personalizzati.

## Progetto Asset compute su GitHub

Il progetto di Asset compute finale è disponibile su GitHub all’indirizzo:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, ovvero `.env`, `console.json` o `.aio`._
