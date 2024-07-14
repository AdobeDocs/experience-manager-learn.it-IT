---
title: Estensione modale dell’interfaccia utente AEM
description: Scopri come creare un’estensione modale dell’interfaccia utente dell’AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
duration: 127
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Finestra modale di estensione

![Estensione dell&#39;interfaccia utente AEM modale](./assets/modal/modal.png){align="center"}

L&#39;estensione modale dell&#39;interfaccia utente AEM consente di collegare un&#39;interfaccia utente personalizzata alle estensioni dell&#39;interfaccia utente AEM.

I moduli sono applicazioni React basate su [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) e possono creare qualsiasi interfaccia utente personalizzata richiesta dall&#39;estensione, tra cui:

+ Finestre di dialogo di conferma
+ [Moduli di input](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Indicatori di avanzamento](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Riepilogo risultati](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Messaggi di errore
+ ... o anche un&#39;applicazione multi-view React completa.

## Route modali

L&#39;esperienza modale è definita dall&#39;estensione App Builder React app definita nella cartella `web-src`. Come per qualsiasi app React, l&#39;esperienza completa è orchestrata utilizzando [route React](https://reactrouter.com/en/main/components/routes) che eseguono il rendering di [componenti React](https://reactjs.org/docs/components-and-props.html).

Per generare la vista modale iniziale è necessaria almeno una route. Questa route iniziale viene richiamata nella funzione `onClick(..)` della registrazione [estensione](#extension-registration), come illustrato di seguito.


+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions.
            Depending on the extension point, different parameters are passed to the modal.
            This example illustrates a modal for the AEM Content Fragment Console (list view), where typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Where as Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="aem-ui-extension/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="aem-ui-extension/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registrazione dell’estensione

Per aprire un modale, viene effettuata una chiamata a `guestConnection.host.modal.showUrl(..)` dalla funzione `onClick(..)` dell&#39;estensione. `showUrl(..)` ha passato un oggetto JavaScript con chiave/valori:

+ `title` fornisce il nome del titolo del modale visualizzato all&#39;utente
+ `url` è l&#39;URL che richiama la [route React](#modal-routes) responsabile della visualizzazione iniziale del modale.

È fondamentale che `url` passato a `guestConnection.host.modal.showUrl(..)` risolva la route nell&#39;estensione, altrimenti non viene visualizzato nulla nel modale.

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/aem-ui-extension/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## Componente modale

Ogni route dell&#39;estensione, [che non è la route `index`](./extension-registration.md#app-routes), viene mappata a un componente React che può eseguire il rendering nella finestra modale dell&#39;estensione.

Un modale può essere composto da un numero qualsiasi di route React, da una semplice modale a una complessa modale a più route.

Di seguito viene illustrata una semplice finestra modale a una route, tuttavia questa visualizzazione modale potrebbe contenere collegamenti React che richiamano altre route o comportamenti.

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## Chiudi il modale

![Pulsante di chiusura modale dell&#39;estensione dell&#39;interfaccia utente AEM](./assets/modal/close.png){align="center"}

I moduli devono fornire il proprio stretto controllo. A tale scopo, richiamare `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
