---
title: Estensioni della barra delle azioni della console Frammento di contenuto AEM
description: Scopri come creare un’estensione della barra delle azioni della console Frammento di contenuto AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# Estensione della barra delle azioni

![Estensione della barra delle azioni](./assets/action-bar/action-bar.png){align="center"}

Estensioni che includono una barra delle azioni, introdurre un pulsante nell’azione della AEM console Frammenti di contenuto che viene visualizzata quando __1 o più__ I frammenti di contenuto sono selezionati. Poiché i pulsanti di estensione della barra delle azioni vengono visualizzati solo quando è selezionato almeno un frammento di contenuto, in genere agiscono sui frammenti di contenuto selezionati. Gli esempi includono:

+ Richiamo di un processo aziendale o di un flusso di lavoro nei frammenti di contenuto selezionati.
+ Aggiornamento o modifica dei dati della selezione dei frammenti di contenuto.

## Registrazione delle estensioni

`ExtensionRegistration.js` è il punto di ingresso dell&#39;estensione AEM e definisce:

1. il tipo di estensione; nel caso di un pulsante della barra delle azioni.
1. Definizione del pulsante di estensione, in `getButton()` funzione .
1. Il gestore di clic per il pulsante, nella `onClick()` funzione .

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## Finestra modale

![Finestra modale](./assets/modal/modal.png)

AEM le estensioni della barra delle azioni della console Frammenti di contenuto possono richiedere:

+ Un ulteriore input dell’utente per eseguire l’azione desiderata.
+ La possibilità di fornire all’utente informazioni dettagliate sul risultato dell’azione.

Per supportare questi requisiti, l’estensione AEM della console Frammenti di contenuto consente un modale personalizzato per il rendering come applicazione React.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Passa alla creazione di un modale</p>
      <p class="has-text-blackest">Scopri come creare un modale da visualizzare facendo clic sul pulsante dell’estensione della barra delle azioni.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Scopri come creare un modale">Scopri come creare un modale</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Nessun modale

A volte, AEM estensioni della console Frammenti di contenuto della barra delle azioni non richiedono ulteriore interazione con l’utente, ad esempio:

+ Richiamare un processo back-end che non richiede l’input dell’utente, ad esempio l’importazione o l’esportazione.

In questi casi, l’estensione della console Frammento di contenuto AEM non richiede un [modale](#modal)ed esegui il lavoro direttamente nel pulsante della barra delle azioni `onClick` handler.

L’estensione AEM console Frammento di contenuto consente di sovrapporre la console Frammento di contenuto AEM durante l’esecuzione del lavoro, impedendo all’utente di eseguire ulteriori azioni. L’utilizzo dell’indicatore di avanzamento è facoltativo, ma utile per comunicare all’utente l’avanzamento del lavoro sincrono.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
