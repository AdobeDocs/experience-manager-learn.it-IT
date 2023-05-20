---
title: Registrazione dell’estensione della console Frammenti di contenuto AEM
description: Scopri come registrare le estensioni della console Frammenti di contenuto.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# Registrazione dell’estensione

Le estensioni della Console Frammenti di contenuto AEM sono app App Builder specializzate, basate su React e utilizzano [Spettro di reazione](https://react-spectrum.adobe.com/react-spectrum/) Framework interfaccia utente.

Per definire dove e come viene visualizzata la console Frammenti di contenuto AEM con l’estensione, sono necessarie due configurazioni specifiche nell’app App Builder dell’estensione: il routing dell’app e la registrazione dell’estensione.

## Route delle app{#app-routes}

Dell&#39;estensione `App.js` dichiara il [Router React](https://reactrouter.com/en/main) che include una route di indice che registra l’estensione nella console Frammenti di contenuto AEM.

La route dell’indice viene richiamata quando inizialmente viene caricata la console Frammenti di contenuto AEM e la destinazione di questa route definisce il modo in cui l’estensione viene esposta nella console.

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registrazione dell’estensione

`ExtensionRegistration.js` deve essere caricato immediatamente tramite la route di indice dell’estensione e agisce sul punto di registrazione dell’estensione, definendo:

1. Il tipo di estensione; a [menu intestazione](./header-menu.md) o [barra delle azioni](./action-bar.md) pulsante.
   + [Menu intestazione](./header-menu.md#extension-registration) le estensioni sono identificate dalla proprietà `headerMenu` proprietà in `methods`.
   + [Barra delle azioni](./action-bar.md#extension-registration) le estensioni sono identificate dalla proprietà `actionBar` proprietà in `methods`.
1. La definizione del pulsante di estensione, in `getButton()` funzione. Questa funzione restituisce un oggetto con campi:
   + `id` è un ID univoco per il pulsante
   + `label` è l’etichetta del pulsante di estensione nella console Frammenti di contenuto AEM
   + `icon` è l’icona del pulsante di estensione nella console Frammenti di contenuto AEM. L’icona è un [Spettro di reazione](https://spectrum.adobe.com/page/icons/) nome dell’icona, con spazi rimossi.
1. Il gestore di clic per il pulsante, in definito in `onClick()` funzione.
   + [Menu intestazione](./header-menu.md#extension-registration) le estensioni non trasmettono parametri al gestore di clic.
   + [Barra delle azioni](./action-bar.md#extension-registration) fornisce un elenco dei percorsi dei frammenti di contenuto selezionati nella `selections` parametro.

### Estensione del menu intestazione

![Estensione del menu intestazione](./assets/extension-registration/header-menu.png)

I pulsanti di estensione del menu Intestazione vengono visualizzati quando non è selezionato alcun frammento di contenuto. Poiché le estensioni del menu di intestazione non agiscono sulla selezione di un frammento di contenuto, non viene fornito alcun frammento di contenuto al relativo `onClick()` handler.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Passa alla creazione di un’estensione del menu di intestazione</p>
      <p class="has-text-blackest">Scopri come registrare e definire un’estensione del menu di intestazione nella console Frammenti di contenuto AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Scopri come creare un’estensione del menu di intestazione">Scopri come creare un’estensione del menu di intestazione</span>
        </a>
      </div>
    </div>
  </div>
</div>

### Estensione della barra delle azioni

![Estensione della barra delle azioni](./assets/extension-registration/action-bar.png)

I pulsanti di estensione della barra delle azioni vengono visualizzati quando sono selezionati uno o più frammenti di contenuto. I percorsi del frammento di contenuto selezionato vengono resi disponibili per l’estensione tramite `selections` parametro, nel pulsante `onClick(..)` handler.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Passa alla creazione di un'estensione della barra delle azioni</p>
      <p class="has-text-blackest">Scopri come registrare e definire un’estensione della barra delle azioni nella console Frammenti di contenuto AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Scopri come creare un’estensione della barra delle azioni">Scopri come creare un’estensione della barra delle azioni</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Includi estensioni in modo condizionale

Le estensioni della Console Frammenti di contenuto dell’AEM possono eseguire una logica personalizzata per limitare il momento in cui l’estensione viene visualizzata nella Console Frammenti di contenuto dell’AEM. Questo controllo viene eseguito prima del `register` chiama in `ExtensionRegistration` e restituisce immediatamente se l’estensione non deve essere visualizzata.

Il contesto disponibile per questo controllo è limitato:

+ L’host AEM su cui viene caricata l’estensione.
+ Token di accesso AEM dell’utente corrente.

I controlli più comuni per il caricamento di un’estensione sono:

+ Utilizzo dell&#39;host AEM (`new URLSearchParams(window.location.search).get('repo')`) per determinare se l&#39;estensione deve essere caricata.
   + Mostra l’estensione solo sugli ambienti AEM che fanno parte di un programma specifico (come mostrato nell’esempio di seguito).
   + Mostrare l’estensione solo in un ambiente AEM specifico (ad es. host AEM).
+ Utilizzo di un [Azione Adobe I/O Runtime](./runtime-action.md) effettuare una chiamata HTTP all’AEM per determinare se l’utente corrente deve visualizzare l’estensione.

L’esempio seguente illustra come limitare l’estensione a tutti gli ambienti nel programma `p12345`.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
