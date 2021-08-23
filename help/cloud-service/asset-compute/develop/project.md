---
title: Creare un progetto di Asset compute per l’estensibilità di Asset compute
description: I progetti di Asset compute sono progetti Node.js, generati utilizzando Adobe I/O CLI, che aderiscono a una determinata struttura e consentono di implementarli in Adobe I/O Runtime e di integrarli con AEM come Cloud Service.
feature: Microservizi di Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
topic: Integrazioni, Sviluppo
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 1%

---


# Creare un progetto di Asset compute

I progetti di Asset compute sono progetti Node.js, generati utilizzando Adobe I/O CLI, che aderiscono a una determinata struttura che consente loro di essere distribuiti in Adobe I/O Runtime e integrati con AEM come Cloud Service. Un singolo progetto di Asset compute può contenere uno o più processi di lavoro Asset compute, ognuno dei quali dispone di un punto finale HTTP discreto referenziabile da un AEM come profilo di elaborazione del Cloud Service.

## Generare un progetto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through della generazione di un progetto di Asset compute (nessun audio)_

Utilizza il [plug-in Asset compute CLI di Adobe I/O](../set-up/development-environment.md#aio-cli) per generare un nuovo progetto di Asset compute vuoto.

1. Dalla riga di comando, passa alla cartella in cui si trova il progetto.
1. Dalla riga di comando, esegui `aio app init` per iniziare la generazione interattiva di progetti CLI.
   + Questo comando potrebbe generare un browser Web che richiede ad Adobe I/O l&#39;autenticazione. In tal caso, fornisci le credenziali Adobe associate ai [servizi e prodotti di Adobe richiesti](../set-up/accounts-and-services.md). Se non riesci ad accedere, segui [queste istruzioni su come generare un progetto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleziona organizzazione__
   + Seleziona l&#39;organizzazione Adobe che ha AEM come Cloud Service, Project Firefly sono registrati con
1. __Seleziona un progetto__
   + Individua e seleziona il Progetto. Si tratta del [Titolo progetto](../set-up/firefly.md) creato dal modello di progetto Firefly, in questo caso `WKND AEM Asset Compute`
1. __Seleziona area di lavoro__
   + Selezionare l&#39;area di lavoro `Development`
1. __Adobe I/O di funzionalità dell&#39;app da abilitare per questo progetto? Seleziona i componenti da includere__
   + Seleziona `Actions: Deploy runtime actions`
   + Utilizzare i tasti freccia per selezionare e lo spazio per deselezionare/selezionare e immettere per confermare la selezione
1. __Selezionare il tipo di azioni da generare__
   + Seleziona `Adobe Asset Compute Worker`
   + Utilizzare i tasti freccia da selezionare, lo spazio per deselezionare/selezionare e immettere per confermare la selezione
1. __Come assegnare un nome a questa azione?__
   + Utilizzare il nome predefinito `worker`.
   + Se il progetto contiene più processi di lavoro che eseguono diversi calcoli delle risorse, denominarli in modo semantico

## Genera console.json

Lo strumento per sviluppatori richiede un file denominato `console.json` che contiene le credenziali necessarie per connettersi ad Adobe I/O. Questo file viene scaricato dalla console Adobe I/O.

1. Apri il progetto [Adobe I/O](https://console.adobe.io) del processo di lavoro di Asset compute
1. Seleziona l’area di lavoro del progetto per scaricare le credenziali `console.json`, in questo caso seleziona `Development`
1. Vai alla directory principale del progetto di Adobe I/O e tocca __Scarica tutto__ nell’angolo in alto a destra.
1. Un file viene scaricato come file `.json` con prefisso per il progetto e l’area di lavoro, ad esempio: `wkndAemAssetCompute-81368-Development.json`
1. Puoi effettuare le seguenti operazioni
   + Rinomina il file come `config.json` e spostalo nella directory principale del progetto di lavoro Asset compute. Questo è l’approccio illustrato in questa esercitazione.
   + Spostalo in una cartella arbitraria E fai riferimento a tale cartella dal file `.env` con una voce di configurazione `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Il percorso del file può essere assoluto o relativo alla directory principale del progetto. Esempio:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Oppure
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> NOTA
> Il file contiene le credenziali. Se archivi il file all’interno del progetto, accertati di aggiungerlo al file `.gitignore` per evitare che venga condiviso. Lo stesso vale per il file `.env`: questi file di credenziali non devono essere condivisi o memorizzati in Git.

## Analisi dell&#39;anatomia del progetto

Il progetto di Asset compute generato è un progetto Node.js da utilizzare come progetto Firefly di progetto Adobe specializzato. I seguenti elementi strutturali sono idiosincratici al progetto di Asset compute:

+ `/actions` contiene sottocartelle e ogni sottocartella definisce un processo di lavoro Asset compute.
   + `/actions/<worker-name>/index.js` definisce il JavaScript utilizzato per eseguire il lavoro di questo processo di lavoro.
      + Il nome della cartella `worker` è un valore predefinito e può essere qualsiasi cosa, purché sia registrato in `manifest.yml`.
      + È possibile definire più cartelle di lavoro in `/actions` in base alle esigenze, tuttavia queste devono essere registrate in `manifest.yml`.
+ `/test/asset-compute` contiene le suite di test per ogni lavoratore. Analogamente alla cartella `/actions`, `/test/asset-compute` può contenere più sottocartelle, ciascuna corrispondente al processo di lavoro di cui esegue il test.
   + `/test/asset-compute/worker`, che rappresenta una suite di test per un processo di lavoro specifico, contiene sottocartelle che rappresentano un caso di test specifico, insieme all’input del test, ai parametri e all’output previsto.
+ `/build` contiene l&#39;output, i registri e gli artefatti delle esecuzioni di test case di Asset compute.
+ `/manifest.yml` definisce i processi di lavoro Asset compute forniti dal progetto. Ogni implementazione di processo di lavoro deve essere enumerata in questo file per renderle disponibili per AEM come Cloud Service.
+ `/console.json` definisce le configurazioni di Adobe I/O
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use` .
+ `/.aio` contiene configurazioni utilizzate dallo strumento aio CLI.
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use` .
+ `/.env` definisce le variabili di ambiente in una  `key=value` sintassi e contiene segreti che non devono essere condivisi. Per proteggere questi segreti, questo file NON deve essere archiviato in Git e viene ignorato tramite il file `.gitignore` predefinito del progetto.
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use` .
   + Le variabili definite in questo file possono essere sostituite da [esportando variabili](../deploy/runtime.md) nella riga di comando.

Per ulteriori dettagli sulla revisione della struttura del progetto, consulta l’ [Anatomia di un progetto Adobe Firefly project](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

La maggior parte dello sviluppo avviene nella cartella `/actions` che sviluppa le implementazioni dei processi di lavoro e in `/test/asset-compute` che scrive i test per i processi di lavoro di Asset compute personalizzati.

## Progetto Asset compute su GitHub

Il progetto di Asset compute finale è disponibile su GitHub all’indirizzo:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene lo stato finale del progetto, compilato completamente con i casi di lavoro e test, ma non contiene credenziali, ovvero  `.env`,  `console.json` o  `.aio`._

