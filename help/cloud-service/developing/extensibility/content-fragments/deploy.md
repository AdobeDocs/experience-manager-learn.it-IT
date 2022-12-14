---
title: Distribuire un’estensione AEM console Frammenti di contenuto
description: Scopri come distribuire un’estensione AEM console Frammenti di contenuto .
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '804'
ht-degree: 0%

---


# Distribuire un&#39;estensione

Per l&#39;utilizzo in ambienti AEM as a Cloud Service, l&#39;app App Builder di estensione deve essere distribuita e approvata.

Ci sono diverse considerazioni da tenere presenti quando si distribuiscono le app App Builder di estensione:

+ Le estensioni sono distribuite nell’area di lavoro del progetto della console Adobe Developer. Le aree di lavoro predefinite sono:
   + __Produzione__ workspace contiene distribuzioni di estensioni disponibili in tutte le AEM as a Cloud Service.
   + __Stage__ workspace funge da area di lavoro per sviluppatori. Le estensioni distribuite nell’area di lavoro Stage non sono disponibili in AEM as a Cloud Service.
Le aree di lavoro della console Adobe Developer non hanno alcuna correlazione diretta con AEM tipi di ambiente as a Cloud Service.
+ Un&#39;estensione implementata nell&#39;area di lavoro Produzione viene visualizzata in tutti gli ambienti AEM as a Cloud Service nell&#39;organizzazione Adobe in cui esiste l&#39;estensione.
Un&#39;estensione non può essere limitata agli ambienti con cui è registrata aggiungendo [logica condizionale che controlla il nome host AEM as a Cloud Service](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ È possibile utilizzare più estensioni in AEM as a Cloud Service. Adobe consiglia di utilizzare ogni app di estensione App Builder per risolvere un singolo obiettivo aziendale. Detto questo, un&#39;unica app App Builder con estensione può implementare più punti di estensione che supportano un obiettivo aziendale comune.

## Distribuzione iniziale

Affinché un&#39;estensione sia disponibile AEM ambienti as a Cloud Service, deve essere implementata in Adobe Developer Console.

Il processo di distribuzione è suddiviso in due passaggi logici:

1. Implementazione dell’app App Builder di estensione nella console Adobe Developer da parte di uno sviluppatore.
1. Approvazione dell&#39;estensione da parte di un manager di distribuzione o di un proprietario aziendale.

### Implementare l&#39;app App Builder di estensione

Distribuisci l&#39;estensione nell&#39;area di lavoro Produzione. Le estensioni distribuite nell’area di lavoro Produzione vengono aggiunte automaticamente a tutti AEM servizi Author as a Cloud Service nell’organizzazione Adobe in cui l’estensione viene distribuita.

1. Apri una riga di comando nella directory principale dell’app App Builder aggiornata.
1. Assicurati che l’area di lavoro Produzione sia attiva

   ```shell
   $ aio app use -w Production
   ```

   Unisci eventuali modifiche a `.env` e `.aio`.

1. Distribuisci l&#39;app App Builder aggiornata.

   ```shell
   $ aio app deploy
   ```

#### Richiedi approvazione della distribuzione

![Invia l&#39;estensione per l&#39;approvazione](./assets/deploy/submit-for-approval.png){align="center"}

1. Accedi a [Console Adobe Developer](https://developer.adobe.com)
1. Seleziona __Console__
1. Passa a __Progetti__
1. Seleziona il progetto associato all’estensione
1. Seleziona la __Produzione__ workspace
1. Seleziona __Invia per approvazione__
1. Completare e inviare il modulo, aggiornando i campi in base alle esigenze.

Nota che è necessaria un&#39;icona. Se non disponi di un&#39;icona, puoi utilizzare [questa icona](./assets/deploy/icon.png).

### Approva la richiesta di distribuzione

![Approvazione dell&#39;estensione](./assets/deploy/adobe-exchange.png){align="center"}

1. Accedi a [Adobe Exchange](https://exchange.adobe.com/)
1. Passa a __Gestisci__ > __App in attesa di revisione__
1. __Revisione__ l’app di estensione App Builder
1. Se le modifiche all&#39;estensione sono accettabili __Accetta__ la revisione. Questo inserisce immediatamente l’estensione su tutti AEM servizi di authoring as a Cloud Service nell’organizzazione Adobe.

Una volta approvata la richiesta di estensione, l’estensione diventa immediatamente attiva nei servizi di authoring as a Cloud Service di AEM.

## Aggiornare un&#39;estensione

L&#39;aggiornamento e l&#39;estensione dell&#39;app Builder seguono lo stesso processo del [distribuzione iniziale](#initial-deployment), con la deviazione che la distribuzione dell&#39;estensione esistente deve prima essere revocata.

### Revoca dell’estensione

Per distribuire una nuova versione di un&#39;estensione, devi prima revocarla (o rimuoverla). L’estensione Revocata non è disponibile AEM console.

1. Accedi a [Adobe Exchange](https://exchange.adobe.com/)
1. Passa a __Gestisci__ > __App di App Builder__
1. __Revoca__ l’estensione da aggiornare

### Distribuire l&#39;estensione

Distribuisci l&#39;estensione nell&#39;area di lavoro Produzione. Le estensioni distribuite nell’area di lavoro Produzione vengono aggiunte automaticamente a tutti AEM servizi Author as a Cloud Service nell’organizzazione Adobe in cui l’estensione viene distribuita.

1. Apri una riga di comando nella directory principale dell’app App Builder aggiornata.
1. Assicurati che l’area di lavoro Produzione sia attiva

   ```shell
   $ aio app use -w Production
   ```

   Unisci eventuali modifiche a `.env` e `.aio`.

1. Distribuisci l&#39;app App Builder aggiornata.

   ```shell
   $ aio app deploy
   ```

#### Richiedi approvazione della distribuzione

![Invia l&#39;estensione per l&#39;approvazione](./assets/deploy/submit-for-approval.png){align="center"}

1. Accedi a [Console Adobe Developer](https://developer.adobe.com)
1. Seleziona __Console__
1. Passa a __Progetti__
1. Seleziona il progetto associato all’estensione
1. Seleziona la __Produzione__ workspace
1. Seleziona __Invia per approvazione__
1. Completare e inviare il modulo, aggiornando i campi in base alle esigenze.

#### Approva la richiesta di distribuzione

![Approvazione dell&#39;estensione](./assets/deploy/adobe-exchange.png){align="center"}

1. Accedi a [Adobe Exchange](https://exchange.adobe.com/)
1. Passa a __Gestisci__ > __App in attesa di revisione__
1. __Revisione__ l’app di estensione App Builder
1. Se le modifiche all&#39;estensione sono accettabili __Accetta__ la revisione. Questo inserisce immediatamente l’estensione su tutti AEM servizi di authoring as a Cloud Service nell’organizzazione Adobe.

Una volta approvata la richiesta di estensione, l’estensione diventa immediatamente attiva nei servizi di authoring as a Cloud Service di AEM.

## Rimuovere un&#39;estensione

![Rimuovere un&#39;estensione](./assets/deploy/revoke.png)

Per rimuovere un&#39;estensione, revocarla (o rimuoverla) da Adobe Exchange. Quando l’estensione viene revocata, viene rimossa da tutti AEM servizi di authoring as a Cloud Service.

1. Accedi a [Adobe Exchange](https://exchange.adobe.com/)
1. Passa a __Gestisci__ > __App di App Builder__
1. __Revoca__ l’estensione da rimuovere
