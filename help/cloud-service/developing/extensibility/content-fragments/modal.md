---
title: Finestra modale dell’estensione della console Frammento di contenuto AEM
description: Scopri come creare un modale di estensione della console dei frammenti di contenuto AEM.
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
source-wordcount: '344'
ht-degree: 0%

---


# Modale di estensione

![Finestra modale dell’estensione del frammento di contenuto AEM](./assets/modal/modal.png){align="center"}

La modalità di estensione AEM frammento di contenuto fornisce un modo per allegare l’interfaccia utente personalizzata alle estensioni dei frammenti di contenuto AEM, siano esse [Barra delle azioni](./action-bar.md) o [Menu Intestazione](./header-menu.md) pulsanti.

I moduli sono applicazioni React, basate su [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/)e può creare qualsiasi interfaccia utente personalizzata richiesta dall’estensione, compresi, tra l’altro:

+ Finestra di dialogo di conferma
+ [Moduli di input](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Indicatori di progresso](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Riepilogo dei risultati](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Messaggi di errore
+ ... o anche un&#39;applicazione React multi-vista completa!

## Linee modali

L’esperienza modale è definita dall’app React dell’estensione App Builder definita in `web-src` cartella. Come per qualsiasi app React, l’esperienza completa viene orchestrata utilizzando [Reagire percorsi](https://reactrouter.com/en/main/components/routes) il rendering [Reagire ai componenti](https://reactjs.org/docs/components-and-props.html).

Per generare la visualizzazione modale iniziale è necessario almeno un percorso. Questa route iniziale viene richiamata nella [registrazione estensione](#extension-registration)s `onClick(..)` , come illustrato di seguito.


+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

            For modals opened from Action Bar extensions, typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="content-fragment/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="content-fragment/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registrazione delle estensioni

Per aprire un modale, effettua una chiamata a `guestConnection.host.modal.showUrl(..)` è creato da `onClick(..)` funzione . `showUrl(..)` viene passato a un oggetto JavaScript con chiave/valori:

+ `title` fornisce all’utente il nome del titolo del modale visualizzato
+ `url` è l’URL che richiama l’ [Reagire via](#modal-routes) responsabile della visualizzazione iniziale del modale.

È imperativo che `url` passato a `guestConnection.host.modal.showUrl(..)` viene risolto in modo da indirizzare nell&#39;estensione, altrimenti non viene visualizzato nulla nel modale.

Consulta la sezione [menu intestazione](./header-menu.md#modal) e [barra delle azioni](./action-bar.md#modal) documentazione relativa alla creazione di URL modali.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-modal";

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

ogni via dell&#39;estensione, [non è il `index` rotta](./extension-registration.md#app-routes), viene mappato su un componente React che può eseguire il rendering nel modale dell’estensione.

Una modale può essere composta da un numero qualsiasi di rotte React, da una semplice modale a una complessa modale multi-rotta.

Di seguito viene illustrato un modale a una sola route, tuttavia questa visualizzazione modale potrebbe contenere collegamenti React che richiamano altri percorsi o comportamenti.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

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

![Pulsante Chiudi modale dell’estensione AEM frammento di contenuto](./assets/modal/close.png){align="center"}

I moduli devono fornire il proprio controllo. Questo fatto richiamando `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
