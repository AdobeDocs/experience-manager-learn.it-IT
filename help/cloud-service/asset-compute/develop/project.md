---
title: Creazione di un progetto Asset Compute per l'estensibilità di Asset Compute
description: I progetti Asset Compute sono progetti Node.js, generati utilizzando l'interfaccia CLI I/O del Adobe , che aderiscono a una determinata struttura che consente loro di essere distribuiti in Adobe I/O Runtime e integrati con AEM come Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# Creazione di un progetto di calcolo risorse

I progetti Asset Compute sono progetti Node.js, generati utilizzando l&#39;interfaccia CLI I/O del Adobe , che aderiscono a una determinata struttura che consente loro di essere distribuiti in Adobe I/O Runtime e integrati con AEM come Cloud Service. Un singolo progetto Asset Compute può contenere uno o più lavoratori Asset Compute, ciascuno dei quali dispone di un punto finale HTTP separato a cui fa riferimento un AEM come profilo di elaborazione Cloud Service.

## Generare un progetto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through per la generazione di un progetto Asset Compute (nessun audio)_


Utilizzate il plug-in [di calcolo risorse CLI I/O di](../set-up/development-environment.md#aio-cli) Adobe per generare un nuovo progetto di calcolo risorse vuoto.

1. Dalla riga di comando, andate alla cartella in cui si trova il progetto.
1. Dalla riga di comando, eseguire `aio app init` per iniziare la generazione interattiva di CLI.
   + Questo potrebbe generare un browser Web che richiede l&#39;autenticazione per  I/O Adobe. In caso affermativo, fornite le credenziali del vostro Adobe  associato ai servizi e ai prodotti [di Adobe ](../set-up/accounts-and-services.md)richiesti. Se non riuscite ad accedere, seguite [queste istruzioni su come generare un progetto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleziona organizzazione__
   + Selezionare l&#39;organizzazione  Adobe che ha AEM come Cloud Service, Project Firefly è registrato a
1. __Seleziona un progetto__
   + Individuate e selezionate il progetto. Questo è il titolo [del](../set-up/firefly.md) progetto creato dal modello del progetto Firefly, in questo caso `WKND AEM Asset Compute`
1. __Seleziona area di lavoro__
   + Selezionare l&#39; `Development` area di lavoro
1. __Quali  funzionalità dell&#39;app I/O di Adobe vuoi abilitare per questo progetto? Selezione dei componenti da includere__
   + Seleziona `Actions: Deploy runtime actions`
   + Utilizzare i tasti freccia per selezionare e lo spazio per deselezionare/selezionare, quindi premere Invio per confermare la selezione
1. __Selezionare il tipo di azioni da generare__
   + Seleziona `Adobe Asset Compute Worker`
   + Utilizzare i tasti freccia per selezionare, lo spazio per deselezionare/selezionare e Invio per confermare la selezione
1. __Come assegnare un nome a questa azione?__
   + Utilizzate il nome predefinito `worker`.
   + Se il progetto contiene più lavoratori che eseguono diversi calcoli di risorse, denominarli in modo semantico

## Generate console.json

Dal livello principale del progetto Asset Compute appena creato, eseguite il comando seguente per generare un `console.json`.

```
$ aio app use
```

Verificate che i dettagli dell’area di lavoro corrente siano corretti e fate clic `Y` o immettete per generare un `console.json`. Se `.env` e `.aio` vengono rilevati come già esistenti, toccate `x` per ignorarne la creazione.

## Rivedere l&#39;anatomia del progetto

Il progetto Asset Compute generato è un progetto Node.js per progetti Firefly di progetti di progetti  Adobe specializzati. Di seguito sono riportati i progetti idiosincratici di Asset Compute:

+ `/actions` contiene sottocartelle e ciascuna sottocartella definisce un lavoratore di elaborazione risorse.
   + `/actions/<worker-name>/index.js` definisce il codice JavaScript eseguito per eseguire il lavoro del lavoratore.
      + Il nome della cartella `worker` è un valore predefinito e può essere qualsiasi cosa, purché sia registrato nel `manifest.yml`.
      + È possibile definire più cartelle di lavoro `/actions` in base alle esigenze, ma queste devono essere registrate nel `manifest.yml`.
+ `/test/asset-compute` contiene le suite di test per ciascun lavoratore. Come per la `/actions` cartella, `/test/asset-compute` possono contenere più sottocartelle, ciascuna corrispondente al lavoro che verifica.
   + `/test/asset-compute/worker`, che rappresenta una suite di test per un lavoratore specifico, contiene sottocartelle che rappresentano un caso di test specifico, insieme all&#39;input di test, ai parametri e all&#39;output previsto.
+ `/build` contiene l’output, i registri e gli artefatti delle esecuzioni di test case per il calcolo delle risorse.
+ `/manifest.yml` Definisce i lavoratori Asset Compute forniti dal progetto. Ogni implementazione del lavoratore deve essere enumerata in questo file per renderle disponibili per AEM come Cloud Service.
+ `/console.json` definisce  configurazioni I/O Adobe
   + Questo file può essere generato/aggiornato utilizzando il `aio app use` comando.
+ `/.aio` contiene configurazioni utilizzate dallo strumento CLI aio.
   + Questo file può essere generato/aggiornato utilizzando il `aio app use` comando.
+ `/.env` definisce le variabili di ambiente in una `key=value` sintassi e contiene segreti che non devono essere condivisi. Questo può essere generato o Per proteggere questi segreti, questo file NON deve essere archiviato in Git e viene ignorato tramite il `.gitignore` file predefinito del progetto.
   + Questo file può essere generato/aggiornato utilizzando il `aio app use` comando.
   + Le variabili definite in questo file possono essere sostituite [esportando le variabili](../deploy/runtime.md) dalla riga di comando.

Per maggiori dettagli sulla revisione della struttura del progetto, consulta l&#39; [Anatomia di un progetto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)Firefly  Adobe.

La maggior parte dello sviluppo viene eseguita nella `/actions` cartella che sta sviluppando le implementazioni dei lavoratori e in `/test/asset-compute` scrittura dei test per i lavoratori di calcolo delle risorse personalizzati.

## Progetto Asset Compute su Github

Il progetto definitivo Asset Compute è disponibile su Github all&#39;indirizzo:

+ [aem-guide-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, ad esempio. `.env`, `console.json` o `.aio`._

