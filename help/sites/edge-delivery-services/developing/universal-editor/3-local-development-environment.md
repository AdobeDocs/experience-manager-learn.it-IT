---
title: Configurare un ambiente di sviluppo locale
description: Configurare un ambiente di sviluppo locale per i siti forniti con Edge Delivery Services e modificabili con Universal Editor.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: ecf37e1f964d0cda90eeca11b224ab950727d2ad
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 1%

---

# Configurare un ambiente di sviluppo locale

Un ambiente di sviluppo locale è essenziale per lo sviluppo rapido di siti web forniti dai Edge Delivery Services. L’ambiente utilizza codice sviluppato localmente per l’approvvigionamento dei contenuti dai Edge Delivery Services, consentendo agli sviluppatori di visualizzare immediatamente le modifiche al codice. Questa configurazione supporta lo sviluppo e il test rapidi e iterativi.

Gli strumenti e i processi di sviluppo per un progetto di sito web Edge Delivery Services sono progettati per essere familiari agli sviluppatori web e fornire un’esperienza di sviluppo veloce ed efficiente.

## Topologia di sviluppo

La topologia di sviluppo per un progetto di sito Web Edge Delivery Services modificabile con Editor universale è costituita dai seguenti aspetti:

- **Archivio GitHub**:
   - **Scopo**: ospita il codice del sito Web (CSS e JavaScript).
   - **Struttura**: il **ramo principale** contiene codice pronto per la produzione, mentre altri rami contengono codice funzionante.
   - **Funzionalità**: il codice da qualsiasi ramo può essere eseguito negli ambienti **production** o **preview** senza influire sul sito Web attivo.

- **Servizio di creazione AEM**:
   - **Scopo**: funge da archivio dei contenuti canonici in cui il contenuto del sito Web viene modificato e gestito.
   - **Funzionalità**: il contenuto viene letto e scritto da **Universal Editor**. Il contenuto modificato è pubblicato in **Edge Delivery Services** in **ambienti di produzione** o **anteprima**.

- **Editor universale**:
   - **Scopo**: fornisce un&#39;interfaccia di WYSIWYG per la modifica del contenuto del sito Web.
   - **Funzionalità**: legge e scrive nel **servizio di creazione AEM**. Può essere configurato per utilizzare il codice di qualsiasi ramo nell&#39;archivio **GitHub** per testare e convalidare le modifiche.

- **Edge Delivery Services**:
   - **Ambiente di produzione**:
      - **Scopo**: consegna il contenuto e il codice del sito Web attivo agli utenti finali.
      - **Funzionalità**: fornisce contenuto pubblicato dal servizio **AEM Author** utilizzando il codice del **ramo principale** dell&#39;archivio **GitHub**.
   - **Anteprima ambiente**:
      - **Scopo**: fornisce un ambiente di gestione temporanea per testare e visualizzare in anteprima il contenuto e il codice prima della distribuzione.
      - **Funzionalità**: fornisce contenuto pubblicato dal servizio **AEM Author** utilizzando il codice di qualsiasi ramo dell&#39;archivio **GitHub**, consentendo l&#39;esecuzione di test approfonditi senza influire sul sito Web attivo.

- **Ambiente sviluppatore locale**:
   - **Scopo**: consente agli sviluppatori di scrivere e testare il codice (CSS e JavaScript) localmente.
   - **Struttura**:
      - Clone locale dell&#39;**archivio GitHub** per lo sviluppo basato su ramo.
      - **AEM CLI**, che funge da server di sviluppo, applica le modifiche al codice locale all&#39;**ambiente di anteprima** per un&#39;esperienza di ricaricamento a caldo.
   - **Flusso di lavoro**: gli sviluppatori scrivono il codice localmente, eseguono il commit delle modifiche in un ramo di lavoro, inviano il ramo a GitHub, lo convalidano nell&#39;**Editor universale** (utilizzando il ramo specificato) e lo uniscono nel **ramo principale** quando è pronto per la distribuzione nell&#39;ambiente di produzione.

## Prerequisiti

Prima di avviare lo sviluppo, installa sul computer quanto segue:

1. [Git](https://git-scm.com/)
1. [Node.js e npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) (o editor di codice simile)

## Clonare l’archivio GitHub

Clona l&#39;archivio [GitHub creato nel nuovo capitolo del progetto di codice](./1-new-code-project.md) che contiene il progetto di codice dei Edge Delivery Services AEM nell&#39;ambiente di sviluppo locale.

![Clone archivio GitHub](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Nella directory `Code` viene creata una nuova cartella `aem-wknd-eds-ue` che funge da radice del progetto. Anche se il progetto può essere clonato in qualsiasi posizione sul computer, questa esercitazione utilizza `~/Code` come directory principale.

## Installare le dipendenze del progetto

Passare alla cartella del progetto e installare le dipendenze richieste con `npm install`. Anche se i progetti di Edge Delivery Services non utilizzano i sistemi di build tradizionali di Node.js come Webpack o Vite, richiedono ancora diverse dipendenze per lo sviluppo locale.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## Installare AEM CLI

L’AEM CLI è uno strumento da riga di comando di Node.js progettato per semplificare lo sviluppo di siti web AEM basati su Edge Delivery Services, che fornisce un server di sviluppo locale per lo sviluppo e il test rapidi del sito web.

Per installare AEM CLI, eseguire:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

L&#39;interfaccia della riga di comando AEM può essere installata anche a livello globale utilizzando `npm install --global @adobe/aem-cli`.

## Avviare il server di sviluppo AEM locale

Il comando `aem up` avvia il server di sviluppo locale e apre automaticamente una finestra del browser all&#39;URL del server. Questo server funge da proxy inverso per l&#39;ambiente dei Edge Delivery Services, distribuendo i contenuti da tale ambiente e utilizzando la base di codice locale per lo sviluppo.

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

L&#39;interfaccia della riga di comando AEM apre il sito Web nel browser in `http://localhost:3000/`. Le modifiche al progetto vengono ricaricate automaticamente a caldo nel browser Web, mentre le modifiche al contenuto [richiedono la pubblicazione nell&#39;ambiente di anteprima](./6-author-block.md) e l&#39;aggiornamento del browser Web.

Se il sito Web viene aperto con una pagina 404, è probabile che [fstab.yaml o paths.json](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) aggiornati nel [nuovo progetto di codice](./1-new-code-project.md) non siano configurati correttamente o che le modifiche non siano state applicate al ramo `main`.

## Creare frammenti JSON

I progetti di Edge Delivery Services, creati utilizzando il modello XWalk boilerplate [AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk), si basano su configurazioni JSON che abilitano l&#39;authoring dei blocchi nell&#39;editor universale.

- **Frammenti JSON**: memorizzati con i blocchi associati e definiscono i modelli di blocco, le definizioni e i filtri.
   - **Frammenti modello**: archiviati in `/blocks/example/_example.json`.
   - **Frammenti di definizione**: archiviati in `/blocks/example/_example.json`.
   - **Frammenti filtro**: archiviati in `/blocks/example/_example.json`.

Gli script NPM compilano questi frammenti JSON e li collocano nelle posizioni appropriate nella directory principale del progetto. Per generare file JSON, utilizza gli script NPM forniti. Ad esempio, per compilare tutti i frammenti, esegui:

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| Script NPM | Descrizione |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | Crea tutti i file JSON dai frammenti e li aggiunge ai file `component-*.json` appropriati. |
| `build:json:models` | Genera frammenti JSON bloccati e li compila in `/component-models.json`. |
| `build:json:definitions` | Genera frammenti JSON della pagina e li compila in `/component-definitions.json`. |
| `build:json:filters` | Genera frammenti JSON della pagina e li compila in `/component-filters.json`. |

>[!TIP]
>
> Esegui `npm run build:json` dopo eventuali modifiche ai file di frammento per rigenerare i file JSON principali.

## Linting

L&#39;indicazione garantisce la qualità e la coerenza del codice, necessarie per i progetti di Edge Delivery Services prima di unire le modifiche nel ramo `main`.

Gli script NPM possono essere eseguiti tramite `npm run`, ad esempio:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| Script NPM | Descrizione |
|------------------|--------------------------------------------------|
| `lint:js` | Esegue i controlli di linting di JavaScript. |
| `lint:css` | Esegue i controlli di linting CSS. |
| `lint` | Esegue sia i controlli di linting JavaScript che CSS. |

### Correzione automatica dei problemi di linting

È possibile risolvere automaticamente i problemi di assegnazione dei punteggi aggiungendo i `scripts` seguenti al progetto `package.json` ed eseguendoli tramite `npm run`:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

Questi script non sono preconfigurati con il modello XWalk boilerplate dell&#39;AEM, ma possono essere aggiunti al file `package.json`:

>[!BEGINTABS]

>[!TAB Script aggiuntivi]

| Script NPM | Comando | Descrizione |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | Corregge automaticamente i problemi di linting di JavaScript. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | Corregge automaticamente i problemi di linting CSS. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | Esegue gli script di correzione JS e CSS per la pulizia rapida. |

>[!TAB esempio package.json]

Le seguenti voci di script possono essere aggiunte all&#39;array `scripts` di `package.json`.

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
