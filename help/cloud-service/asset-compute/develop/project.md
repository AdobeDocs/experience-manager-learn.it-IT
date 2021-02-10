---
title: Creare un progetto di Asset compute  per  estensibilità del Asset compute
description: ' progetti di Asset compute sono progetti Node.js, generati utilizzando l''interfaccia CLI di Adobe I/O , che aderiscono a una determinata struttura che consente loro di essere distribuiti in Adobe I/O Runtime e integrati con AEM come Cloud Service.'
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 23c91551673197cebeb517089e5ab6591f084846
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 1%

---


# Creare un progetto di Asset compute 

 progetti di Asset compute sono progetti Node.js, generati utilizzando l&#39;interfaccia CLI di Adobe I/O , che aderiscono a una determinata struttura che consente loro di essere distribuiti in Adobe I/O Runtime e integrati con AEM come Cloud Service. Un singolo progetto di Asset compute  può contenere uno o più Asset compute , ciascuno dei quali dispone di un punto finale HTTP separato a cui viene fatto riferimento da un AEM come profilo di elaborazione Cloud Service.

## Generare un progetto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through della generazione di un progetto di Asset compute  (nessun audio)_

Utilizzare il plug-in [ Adobe I/O CLI  Asset compute ](../set-up/development-environment.md#aio-cli) per generare un nuovo progetto  Asset compute vuoto.

1. Dalla riga di comando, andate alla cartella in cui si trova il progetto.
1. Dalla riga di comando, eseguire `aio app init` per iniziare la generazione interattiva di CLI.
   + Questo comando potrebbe generare un browser Web che richiede l&#39;autenticazione per  Adobe I/O. In caso contrario, fornisci le credenziali  Adobe associate ai [servizi e prodotti  Adobe richiesti](../set-up/accounts-and-services.md). Se non è possibile effettuare l&#39;accesso, seguire [le istruzioni seguenti su come generare un progetto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleziona organizzazione__
   + Selezionate l&#39;organizzazione  Adobe che AEM come Cloud Service, Project Firefly è registrato con
1. __Seleziona un progetto__
   + Individuate e selezionate il progetto. Si tratta del [Titolo del progetto](../set-up/firefly.md) creato dal modello del progetto Firefly, in questo caso `WKND AEM Asset Compute`
1. __Seleziona area di lavoro__
   + Selezionare l&#39;area di lavoro `Development`
1. __Quali funzionalità  app Adobe I/O desiderate abilitare per questo progetto? Selezionare i componenti da includere__
   + Seleziona `Actions: Deploy runtime actions`
   + Utilizzare i tasti freccia per selezionare e lo spazio per deselezionare/selezionare, quindi premere Invio per confermare la selezione
1. __Selezionare il tipo di azioni da generare__
   + Seleziona `Adobe Asset Compute Worker`
   + Utilizzare i tasti freccia per selezionare, lo spazio per deselezionare/selezionare e Invio per confermare la selezione
1. __Come assegnare un nome a questa azione?__
   + Utilizzate il nome predefinito `worker`.
   + Se il progetto contiene più lavoratori che eseguono diversi calcoli di risorse, denominarli in modo semantico

## Generate console.json

Lo strumento di sviluppo richiede un file denominato `console.json` che contiene le credenziali necessarie per connettersi a  Adobe I/O. Questo file viene scaricato dalla  console Adobe I/O.

1. Aprire il progetto [ Adobe I/O](https://console.adobe.io) del lavoratore del Asset compute 
1. Selezionate l&#39;area di lavoro del progetto per scaricare le credenziali `console.json`, in questo caso selezionate `Development`
1. Andate alla radice del progetto Adobe I/O  e toccate __Scarica tutto__ nell&#39;angolo superiore destro.
1. Un file viene scaricato come `.json` file con il prefisso del progetto e dell&#39;area di lavoro, ad esempio: `wkndAemAssetCompute-81368-Development.json`
1. Puoi effettuare le seguenti operazioni
   + Rinominare il file come `config.json` e spostarlo nella directory principale del progetto di lavoro del Asset compute . Questo è l&#39;approccio seguito in questa esercitazione.
   + Spostatela in una cartella arbitraria E fate riferimento a tale cartella dal file `.env` con una voce di configurazione `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Il percorso del file può essere assoluto o relativo alla directory principale del progetto. Esempio:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Oppure
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> NOTA
> Il file contiene le credenziali. Se archiviate il file all&#39;interno del progetto, accertatevi di aggiungerlo al file `.gitignore` per evitare che venga condiviso. Lo stesso vale per il file `.env`: questi file di credenziali non devono essere condivisi o memorizzati in Git.

## Rivedere l&#39;anatomia del progetto

Il progetto  Asset compute generato è un progetto Node.js da utilizzare come progetto Firefly di progetto  Adobe specializzato. I seguenti elementi strutturali sono idiosincratici  progetto di Asset compute:

+ `/actions` contiene sottocartelle e ciascuna sottocartella definisce un lavoratore Asset compute .
   + `/actions/<worker-name>/index.js` definisce il codice JavaScript utilizzato per eseguire il lavoro del lavoratore.
      + Il nome della cartella `worker` è un valore predefinito e può essere qualsiasi cosa, purché sia registrato in `manifest.yml`.
      + È possibile definire più cartelle di lavoro in `/actions` in base alle esigenze, ma devono essere registrate nella cartella `manifest.yml`.
+ `/test/asset-compute` contiene le suite di test per ciascun lavoratore. Come per la cartella `/actions`, `/test/asset-compute` può contenere più sottocartelle, ciascuna corrispondente al processo di lavoro di cui esegue il test.
   + `/test/asset-compute/worker`, che rappresenta una suite di test per un lavoratore specifico, contiene sottocartelle che rappresentano un test case specifico, insieme all&#39;input di test, ai parametri e all&#39;output previsto.
+ `/build` contiene l&#39;output, i file di registro e gli artefatti &#39;esecuzione di test case Asset compute.
+ `/manifest.yml` Definisce  lavoratori Asset compute forniti dal progetto. Ogni implementazione del lavoratore deve essere enumerata in questo file per renderle disponibili per AEM come Cloud Service.
+ `/console.json` definisce  configurazioni Adobe I/O
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use`.
+ `/.aio` contiene configurazioni utilizzate dallo strumento CLI aio.
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use`.
+ `/.env` definisce le variabili di ambiente in una  `key=value` sintassi e contiene segreti che non devono essere condivisi. Per proteggere questi segreti, questo file NON deve essere archiviato in Git e viene ignorato tramite il file `.gitignore` predefinito del progetto.
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use`.
   + Le variabili definite in questo file possono essere sostituite da [esportando variabili](../deploy/runtime.md) nella riga di comando.

Per ulteriori dettagli sulla revisione della struttura del progetto, vedere l&#39; [Anatomia di un progetto di Adobe  progetto Firefly](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

La maggior parte dello sviluppo avviene nella cartella `/actions` che sta sviluppando implementazioni dei lavoratori e in `/test/asset-compute` test di scrittura per i lavoratori dei Asset compute  personalizzati.

##  progetto di Asset compute su GitHub

Il progetto finale di Asset compute  è disponibile su GitHub all&#39;indirizzo:

+ [aem-guide-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, vale a dire  `.env`o  `console.json` o  `.aio`._

