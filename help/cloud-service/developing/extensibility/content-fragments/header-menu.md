---
title: Estensioni del menu di intestazione della console Frammenti di contenuto AEM
description: Scopri come creare estensioni del menu di intestazione della console Frammenti di contenuto AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 92d6e98e-24d0-4229-9d30-850f6b72ab43
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Estensione del menu intestazione

![Estensione del menu intestazione](./assets/header-menu/header-menu.png){align="center"}

Estensioni che includono un menu di intestazione, introduce un pulsante nell’intestazione della Console Frammenti di contenuto AEM che viene visualizzata quando __no__ I frammenti di contenuto sono selezionati. Poiché i pulsanti dell’estensione del menu di intestazione vengono visualizzati solo quando non è selezionato alcun frammento di contenuto, in genere non agiscono sui frammenti di contenuto esistenti. Invece, i menu di intestazione in genere:

+ Creare nuovi frammenti di contenuto utilizzando una logica personalizzata, ad esempio creando un set di frammenti di contenuto collegati tramite riferimenti ai contenuti.
+ Agire su un set di frammenti di contenuto selezionato a livello di programmazione, ad esempio esportare tutti i frammenti di contenuto creati nell’ultima settimana.

## Registrazione dell’estensione

`ExtensionRegistration.js` è il punto di ingresso per l’estensione AEM e definisce:

1. Il tipo di estensione; nel caso di un pulsante del menu di intestazione.
1. La definizione del pulsante di estensione, in `getButton()` funzione.
1. Il gestore di clic per il pulsante, nel `onClick()` funzione.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## Finestra modale

![Finestra modale](./assets/modal/modal.png)

Le estensioni del menu di intestazione della Console Frammenti di contenuto AEM possono richiedere:

+ Input aggiuntivo dall’utente per eseguire l’azione desiderata.
+ Possibilità di fornire all’utente informazioni dettagliate sul risultato dell’azione.

Per supportare questi requisiti, l’estensione della Console Frammenti di contenuto AEM consente una finestra modale personalizzata che viene riprodotta come applicazione React.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Passa alla creazione di un modale</p>
      <p class="has-text-blackest">Scopri come creare un modale da visualizzare facendo clic sul pulsante dell’estensione del menu dell’intestazione.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Scopri come creare un modale">Scopri come creare un modale</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Nessuna finestra modale

A volte, le estensioni del menu di intestazione della console Frammenti di contenuto AEM non richiedono un’ulteriore interazione con l’utente, ad esempio:

+ Richiamare un processo back-end che non richiede l’input dell’utente, ad esempio l’importazione o l’esportazione.
+ Apertura di una nuova pagina web, ad esempio per consultare la documentazione interna sulle linee guida sui contenuti.

In questi casi, l’estensione della Console Frammenti di contenuto AEM non richiede un’ [modale](#modal)e può eseguire il lavoro direttamente nel pulsante del menu intestazione `onClick` handler.

L’estensione della console Frammenti di contenuto AEM consente a un indicatore di avanzamento di sovrapporsi alla console Frammenti di contenuto AEM durante l’esecuzione del lavoro, impedendo all’utente di eseguire ulteriori azioni. L&#39;uso dell&#39;indicatore di avanzamento è facoltativo, ma utile per comunicare all&#39;utente l&#39;avanzamento del lavoro sincrono.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
