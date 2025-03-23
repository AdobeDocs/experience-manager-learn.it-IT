---
title: Registrazione dell’estensione dell’interfaccia utente di AEM
description: Scopri come registrare un’estensione dell’interfaccia utente di AEM.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
duration: 85
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Registrazione dell’estensione

Le estensioni dell&#39;interfaccia utente di AEM sono app App Builder specializzate, basate su React e utilizzano il framework dell&#39;interfaccia utente [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/).

Per definire dove e come viene visualizzata l’estensione dell’interfaccia utente di AEM, sono necessarie due configurazioni nell’app App Builder dell’estensione: il routing dell’app e la registrazione dell’estensione.

## Route delle app{#app-routes}

Il `App.js` dell&#39;estensione dichiara il [router React](https://reactrouter.com/en/main) che include una route di indice che registra l&#39;estensione nell&#39;interfaccia utente di AEM.

La route dell&#39;indice viene richiamata al caricamento iniziale dell&#39;interfaccia utente di AEM e la destinazione di questa route definisce il modo in cui l&#39;estensione viene esposta nella console.

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

`ExtensionRegistration.js` deve essere caricato immediatamente tramite la route dell&#39;indice dell&#39;estensione e agisce come punto di registrazione dell&#39;estensione.

In base al modello di estensione dell&#39;interfaccia utente di AEM selezionato al momento dell&#39;inizializzazione dell&#39;estensione dell&#39;app App Builder](./app-initialization.md), sono supportati diversi punti di estensione.[

+ [Punti di estensione dell’interfaccia utente per frammenti di contenuto](./content-fragments/overview.md#extension-points)

## Includi estensioni in modo condizionale

Le estensioni dell’interfaccia utente di AEM possono eseguire una logica personalizzata per limitare gli ambienti AEM in cui viene visualizzata l’estensione. Questo controllo viene eseguito prima della chiamata `register` nel componente `ExtensionRegistration` e restituisce immediatamente se l&#39;estensione non deve essere visualizzata.

Il contesto disponibile per questo controllo è limitato:

+ L&#39;host AEM su cui viene caricata l&#39;estensione.
+ Token di accesso AEM dell’utente corrente.

I controlli più comuni per il caricamento di un’estensione sono:

+ Utilizzo dell&#39;host AEM (`new URLSearchParams(window.location.search).get('repo')`) per determinare se l&#39;estensione deve essere caricata.
   + Mostra l’estensione solo negli ambienti AEM che fanno parte di un programma specifico (come mostrato nell’esempio di seguito).
   + Mostra l’estensione solo in un ambiente AEM specifico (host AEM).
+ Utilizzo di un&#39;azione [Adobe I/O Runtime](./runtime-action.md) per effettuare una chiamata HTTP ad AEM per determinare se l&#39;utente corrente deve visualizzare l&#39;estensione.

L&#39;esempio seguente illustra la limitazione dell&#39;estensione a tutti gli ambienti nel programma `p12345`.

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
