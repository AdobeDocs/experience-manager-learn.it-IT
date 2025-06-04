---
title: Configurare un ambiente di sviluppo locale
description: Configurare un ambiente di sviluppo locale per i siti da distribuire tramite Edge Delivery Services e modificabili con l’editor universale.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '994'
ht-degree: 100%

---

# Configurare un ambiente di sviluppo locale

Un ambiente di sviluppo locale è essenziale per lo sviluppo rapido di siti web distribuiti da Edge Delivery Services. L’ambiente utilizza codice sviluppato localmente per contenuti forniti da Edge Delivery Services, consentendo agli sviluppatori di visualizzare immediatamente le modifiche apportate al codice. Questa configurazione supporta lo sviluppo e il test rapidi e iterativi.

Gli strumenti e i processi di sviluppo per un progetto di sito web Edge Delivery Services sono progettati per risultare familiari agli sviluppatori web e fornire un’esperienza di sviluppo veloce ed efficiente.

## Topologia di sviluppo

Questo video offre una panoramica della topologia di sviluppo di un progetto di sito web Edge Delivery Services modificabile con l’editor universale.

>[!VIDEO](https://video.tv.adobe.com/v/3443986/?learn=on&enablevpops&captions=ita)

+++Ulteriori dettagli sulla topologia di sviluppo

- **Archivio GitHub**:
   - **Scopo**: ospita il codice del sito web (CSS e JavaScript).
   - **Struttura**: il **ramo principale** contiene codice pronto per la produzione, mentre altri rami contengono codice funzionante.
   - **Funzionalità**: il codice presente i tutti i rami può essere eseguito per ambienti **di produzione** o **di anteprima**, senza influire sul sito web live.

- **Servizio AEM Author**
   - **Scopo**: funge da archivio dei contenuti canonici in cui il contenuto del sito web viene modificato e gestito.
   - **Funzionalità**: il contenuto viene letto e scritto dall’**editor universale**. Il contenuto modificato viene pubblicato in **Edge Delivery Services** in ambienti **di produzione** o **di anteprima**.

- **Editor universale**:
   - **Scopo**: fornisce un’interfaccia WYSIWYG per la modifica del contenuto del sito web.
   - **Funzionalità**: legge e scrive nel **servizio AEM Author**. Può essere configurato per utilizzare il codice di qualsiasi ramo nell’archivio **GitHub** per testare e convalidare le modifiche.

- **Edge Delivery Services**:
   - **Ambiente di produzione**:
      - **Scopo**: consegna agli utenti finali il contenuto e il codice del sito web live.
      - **Funzionalità**: fornisce il contenuto pubblicato dal **servizio AEM Author** utilizzando il codice del **ramo principale** dell’**archivio GitHub**.
   - **Ambiente di anteprima**:
      - **Scopo**: fornisce un ambiente di staging in cui testare e visualizzare in anteprima il contenuto e il codice prima dell’implementazione.
      - **Funzionalità**: fornisce il contenuto pubblicato dal **servizio di authoring AEM** utilizzando il codice di qualsiasi ramo dell’**archivio GitHub**, consentendo l’esecuzione di test approfonditi senza influire sul sito web live.

- **Ambiente di sviluppo locale**:
   - **Scopo**: consente agli sviluppatori di scrivere e testare il codice (CSS e JavaScript) localmente.
   - **Struttura**:
      - Clone locale dell’**archivio GitHub** per lo sviluppo basato su rami.
      - La **CLI di AEM**, che funge da server di sviluppo, applica le modifiche del codice locale all’**ambiente di anteprima** per un’esperienza di ricaricamento immediato.
   - **Flusso di lavoro**: gli sviluppatori scrivono il codice localmente, eseguono il commit delle modifiche in un ramo di lavoro, inviano il ramo a GitHub, lo convalidano nell’**editor universale** (utilizzando il ramo specificato) e lo uniscono nel **ramo principale** quando è pronto per essere implementato nell’ambiente di produzione.

+++

## Prerequisiti

Prima di avviare lo sviluppo, installa sul computer quanto segue:

1. [Git](https://git-scm.com/)
1. [Node.js e npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) (o altro editor di codice simile)

## Clonare l’archivio GitHub

Clona nell’ambiente di sviluppo locale l’[archivio GitHub creato nel capitolo sul nuovo progetto di codice](./1-new-code-project.md), contenente il progetto di codice AEM Edge Delivery Services.

![Clone dell’archivio GitHub](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Viene creata una nuova cartella `aem-wknd-eds-ue` nella directory `Code`, che funge da directory principale del progetto. Anche se il progetto può essere clonato in qualsiasi posizione sul computer, per questo tutorial viene utilizzato `~/Code` come directory principale.

## Installare le dipendenze del progetto

Passare alla cartella del progetto e installare le dipendenze richieste tramite `npm install`. Anche se i progetti Edge Delivery Services non utilizzano i sistemi di build Node.js tradizionali, come Webpack o Vite, richiedono comunque diverse dipendenze per lo sviluppo locale.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## Installare le CLI di AEM

AEM CLI è uno strumento per riga di comando di Node.js progettato per semplificare lo sviluppo di siti web AEM basati su Edge Delivery Services, che fornisce un server di sviluppo locale per lo sviluppo e il test rapidi del sito web.

Per installare AEM CLI, esegui:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

AEM CLI può essere installato anche a livello globale utilizzando `npm install --global @adobe/aem-cli`.

## Avviare il server di sviluppo AEM locale

Il comando `aem up` avvia il server di sviluppo locale e apre automaticamente una finestra del browser all’URL del server. Questo server funge da proxy inverso per l’ambiente Edge Delivery Services, distribuendo i contenuti da tale ambiente e utilizzando la base di codice locale per lo sviluppo.

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

AEM CLI apre il sito web nel browser all’indirizzo `http://localhost:3000/`. Le modifiche al progetto vengono effettuate con caricamento rapido automatico nel browser web, mentre le modifiche al contenuto [richiedono la pubblicazione nell’ambiente di anteprima](./6-author-block.md) e l’aggiornamento del browser web.

Se il sito web si apre con una pagina 404, è probabile che i file [fstab.yaml o paths.json](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) aggiornati nel [nuovo progetto di codice](./1-new-code-project.md) non siano configurati correttamente o che le modifiche non siano state applicate al ramo `main`.

## Creare frammenti JSON

I progetti Edge Delivery Services, creati utilizzando il [modello standard XWalk di AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk), si basano su configurazioni JSON che abilitano l’authoring dei blocchi nell’editor universale.

- **Frammenti JSON**: sono memorizzati con i blocchi associati e definiscono i modelli dei blocchi, le definizioni e i filtri.
   - **Frammenti di modello**: sono memorizzati in `/blocks/example/_example.json`.
   - **Frammenti di definizione**: sono memorizzati in `/blocks/example/_example.json`.
   - **Frammenti di filtro**: sono memorizzati in `/blocks/example/_example.json`.


Il [modello di progetto standard XWalk di AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk) include un hook di pre-commit [Husky](https://typicode.github.io/husky/) che rileva le modifiche ai frammenti JSON e le compila nei file `component-*.json` appropriati al momento del `git commit`.

I seguenti script NPM possono essere eseguiti manualmente tramite `npm run` per generare i file JSON, ma in genere questo non è necessario in quanto l’hook di pre-commit Husky lo gestisce automaticamente.

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| Script NPM | Descrizione |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | Crea tutti i file JSON dai frammenti e li aggiunge ai file `component-*.json` appropriati. |
| `build:json:models` | Genera frammenti JSON dei blocchi e li compila in `/component-models.json`. |
| `build:json:definitions` | Genera frammenti JSON di pagine e li compila in `/component-definitions.json`. |
| `build:json:filters` | Genera frammenti JSON di pagine e li compila in `/component-filters.json`. |

>[!TIP]
>
> Esegui `npm run build:json` dopo eventuali modifiche ai file di frammento per rigenerare i file JSON principali.

## Linting

Il linting garantisce la qualità e la coerenza del codice, necessarie per i progetti Edge Delivery Services prima che si possa procedere all’unione delle modifiche nel ramo `main`.

Gli script NPM possono essere eseguiti tramite `npm run`, ad esempio:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| Script NPM | Descrizione |
|------------------|--------------------------------------------------|
| `lint:js` | Esegue i controlli di linting JavaScript. |
| `lint:css` | Esegue i controlli di linting CSS. |
| `lint` | Esegue i controlli di linting sia JavaScript che CSS. |

### Correzione automatica degli errori di linting

È possibile risolvere automaticamente gli errori di linting aggiungendo i seguenti `scripts` al file `package.json` del progetto ed eseguendoli tramite `npm run`:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

Questi script non vengono preconfigurati con il modello standard XWalk di AEM, ma possono essere aggiunti al file `package.json`:

>[!BEGINTABS]

>[!TAB Script aggiuntivi]

| Script NPM | Comando | Descrizione |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | Corregge automaticamente gli errori di linting JavaScript. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | Corregge automaticamente gli errori di linting CSS. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | Esegue entrambi gli script di correzione JS e CSS per una pulizia rapida. |

>[!TAB Esempio di package.json]

Le seguenti voci di script possono essere aggiunte all’array `scripts` di `package.json`.

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
