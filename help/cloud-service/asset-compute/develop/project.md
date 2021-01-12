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
source-git-commit: 676d4bfceaaec3ae8d4feb9f66294ec04e1ecd2b
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# Creare un progetto di Asset compute 

 progetti di Asset compute sono progetti Node.js, generati utilizzando l&#39;interfaccia CLI di Adobe I/O , che aderiscono a una determinata struttura che consente loro di essere distribuiti in Adobe I/O Runtime e integrati con AEM come Cloud Service. Un singolo progetto di Asset compute  può contenere uno o più Asset compute , ciascuno dei quali dispone di un punto finale HTTP separato a cui viene fatto riferimento da un AEM come profilo di elaborazione Cloud Service.

## Generare un progetto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through della generazione di un progetto di Asset compute  (nessun audio)_

Utilizzate il plug-in di Asset compute [ CLI  Adobe I/O ](../set-up/development-environment.md#aio-cli) per generare un nuovo progetto Asset compute  vuoto.

1. Dalla riga di comando, andate alla cartella in cui si trova il progetto.
1. Dalla riga di comando, eseguire `aio app init` per iniziare la generazione interattiva di CLI.
   + Questo potrebbe generare un browser Web che richiede l&#39;autenticazione per  Adobe I/O. In caso contrario, fornisci le credenziali  Adobe associate ai [servizi e prodotti  Adobe richiesti](../set-up/accounts-and-services.md). Se non è possibile effettuare l&#39;accesso, seguire [queste istruzioni su come generare un progetto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleziona organizzazione__
   + Selezionare l&#39;organizzazione  Adobe che ha AEM come Cloud Service, Project Firefly è registrato a
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

Dal livello principale del progetto di Asset compute  appena creato, eseguite il comando seguente per generare un `console.json`.

```
$ aio app use
```

Verificare che i dettagli dell&#39;area di lavoro corrente siano corretti, premere `Y` o immettere per generare un `console.json`. Se `.env` e `.aio` vengono rilevati come già esistenti, toccate `x` per ignorarne la creazione.

Se si crea una nuova, o si sovrascrive `.env`, aggiungere di nuovo chiavi/valori mancanti alla nuova `.env`:

```
## please provide the following environment variables for the Asset Compute devtool. You can use AWS or Azure, not both:
#ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=
#S3_BUCKET=
#AWS_ACCESS_KEY_ID=
#AWS_SECRET_ACCESS_KEY=
#AWS_REGION=
#AZURE_STORAGE_ACCOUNT=
#AZURE_STORAGE_KEY=
#AZURE_STORAGE_CONTAINER_NAME=
```

## Rivedere l&#39;anatomia del progetto

Il progetto  Asset compute generato è un progetto Node.js per un progetto  Adobe specializzato Firefly, i seguenti sono idiosincratici per  progetto Asset compute:

+ `/actions` contiene sottocartelle e ogni sottocartella definisce un lavoratore Asset compute .
   + `/actions/<worker-name>/index.js` definisce il codice JavaScript eseguito per eseguire il lavoro del lavoratore.
      + Il nome della cartella `worker` è un valore predefinito e può essere qualsiasi cosa, purché sia registrato in `manifest.yml`.
      + È possibile definire più cartelle di lavoro in `/actions` in base alle esigenze, ma devono essere registrate nella cartella `manifest.yml`.
+ `/test/asset-compute` contiene le suite di test per ciascun lavoratore. Come per la cartella `/actions`, `/test/asset-compute` può contenere più sottocartelle, ciascuna corrispondente al lavoro di cui esegue il test.
   + `/test/asset-compute/worker`, che rappresenta una suite di test per un lavoratore specifico, contiene sottocartelle che rappresentano un caso di test specifico, insieme all&#39;input di test, ai parametri e all&#39;output previsto.
+ `/build` contiene l&#39;output, i file di registro e gli artefatti &#39;esecuzione di test case Asset compute.
+ `/manifest.yml` Definisce  lavoratori Asset compute forniti dal progetto. Ogni implementazione del lavoratore deve essere enumerata in questo file per renderle disponibili per AEM come Cloud Service.
+ `/console.json` definisce  configurazioni Adobe I/O
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use`.
+ `/.aio` contiene configurazioni utilizzate dallo strumento CLI aio.
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use`.
+ `/.env` definisce le variabili di ambiente in una  `key=value` sintassi e contiene segreti che non devono essere condivisi. Questo può essere generato o Per proteggere questi segreti, questo file NON deve essere archiviato in Git e viene ignorato tramite il file `.gitignore` predefinito del progetto.
   + Questo file può essere generato/aggiornato utilizzando il comando `aio app use`.
   + Le variabili definite in questo file possono essere sostituite da [esportando variabili](../deploy/runtime.md) nella riga di comando.

Per ulteriori dettagli sulla revisione della struttura del progetto, vedere l&#39; [Anatomia di un progetto Firefly del Adobe ](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

La maggior parte dello sviluppo avviene nella cartella `/actions` che sta sviluppando implementazioni dei lavoratori e in `/test/asset-compute` test di scrittura per i lavoratori dei Asset compute  personalizzati.

## Progetto di Asset compute  su Github

Il progetto finale del Asset compute  è disponibile su Github all&#39;indirizzo:

+ [aem-guide-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, ad esempio. `.env`,  `console.json` o  `.aio`._

