---
title: Registrazione dell’estensione dell’interfaccia utente AEM
description: Scopri come registrare un’estensione dell’interfaccia utente dell’AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Registrazione dell’estensione

Le estensioni dell’interfaccia utente dell’AEM sono app specializzate per App Builder, basate su React e utilizzano [Spettro di reazione](https://react-spectrum.adobe.com/react-spectrum/) Framework interfaccia utente.

Per definire dove e come viene visualizzata l&#39;estensione dell&#39;interfaccia utente AEM, sono necessarie due configurazioni nell&#39;app App Builder dell&#39;estensione: il routing dell&#39;app e la registrazione dell&#39;estensione.

## Route delle app{#app-routes}

Dell&#39;estensione `App.js` dichiara il [Router React](https://reactrouter.com/en/main) che include una route di indice che registra l’estensione nell’interfaccia utente dell’AEM.

La route dell&#39;indice viene richiamata al caricamento iniziale dell&#39;interfaccia utente AEM e la destinazione di questa route definisce il modo in cui l&#39;estensione viene esposta nella console.

+ `./src/aem-ui-extension/web-src/src/components/App.js`

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

`ExtensionRegistration.js` deve essere caricato immediatamente tramite la route di indice dell&#39;estensione e agisce sul punto di registrazione dell&#39;estensione.

In base al modello di estensione dell’interfaccia utente AEM selezionato quando [inizializzazione dell&#39;estensione app Builder](./app-initialization.md), sono supportati diversi punti di estensione.

+ [Punti di estensione dell’interfaccia utente per frammenti di contenuto](./content-fragments/overview.md#extension-points)


## Includi estensioni in modo condizionale

Le estensioni dell’interfaccia utente AEM possono eseguire una logica personalizzata per limitare gli ambienti AEM in cui viene visualizzata l’estensione. Questo controllo viene eseguito prima del `register` chiama in `ExtensionRegistration` e restituisce immediatamente se l’estensione non deve essere visualizzata.

Il contesto disponibile per questo controllo è limitato:

+ L’host AEM su cui viene caricata l’estensione.
+ Token di accesso AEM dell’utente corrente.

I controlli più comuni per il caricamento di un’estensione sono:

+ Utilizzo dell&#39;host AEM (`new URLSearchParams(window.location.search).get('repo')`) per determinare se l&#39;estensione deve essere caricata.
   + Mostra l’estensione solo sugli ambienti AEM che fanno parte di un programma specifico (come mostrato nell’esempio di seguito).
   + Mostra l’estensione solo in un ambiente AEM specifico (host AEM).
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
