---
title: Anteprima frammento di contenuto
description: Scopri come utilizzare l’anteprima Frammento di contenuto per tutti gli autori per vedere rapidamente in che modo i cambiamenti di contenuto influiscono sulle esperienze AEM headless.
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# Anteprima frammento di contenuto

AEM Le applicazioni headless supportano l&#39;anteprima integrata dell&#39;authoring. L’esperienza di anteprima collega l’editor Frammento di contenuto di AEM Author alla tua app personalizzata (indirizzabile tramite HTTP), consentendo un collegamento diretto all’app che esegue il rendering del frammento di contenuto visualizzato in anteprima.

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

Per utilizzare l’anteprima Frammento di contenuto, è necessario soddisfare diverse condizioni:

1. L’app deve essere distribuita a un URL accessibile agli autori
1. L’app deve essere configurata per connettersi al servizio AEM Author (anziché al servizio AEM Publish)
1. L’app deve essere progettata con URL o percorsi che possono utilizzare [Percorso o ID del frammento di contenuto](#url-expressions) per selezionare i frammenti di contenuto da visualizzare per l’anteprima nell’esperienza dell’app.

## URL di anteprima

URL di anteprima, utilizzando [Espressioni URL](#url-expressions), sono impostate in Proprietà del modello di frammento di contenuto.

![URL anteprima modello frammento di contenuto](./assets/preview/cf-model-preview-url.png)

1. Accedi al servizio AEM Author come amministratore
1. Passa a __Strumenti > Generale > Modelli per frammenti di contenuto__
1. Seleziona la __Modello per frammento di contenuto__ e seleziona __Proprietà__ nella barra delle azioni superiore.
1. Immetti l’URL di anteprima per il modello di frammento di contenuto utilizzando [Espressioni URL](#url-expressions)
   + L’URL di anteprima deve puntare a una distribuzione dell’app che si connette al servizio AEM Author.

### Espressioni URL

Per ogni modello di frammento di contenuto può essere impostato un URL di anteprima. L’URL di anteprima può essere parametrizzato per frammento di contenuto utilizzando le espressioni URL elencate nella tabella seguente. È possibile utilizzare più espressioni URL in un unico URL di anteprima.

|  | Espressione URL | Valore |
| --------------------------------------- | ----------------------------------- | ----------- |
| Percorso frammento di contenuto | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| ID frammento di contenuto | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Variante del frammento di contenuto | `${contentFragment.variation}` | `main` |
| Percorso modello frammento di contenuto | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Nome modello frammento di contenuto | `${contentFragment.model.name}` | `adventure` |

Esempio di URL di anteprima:

+ Un URL di anteprima sul __Avventura__ modello `https://preview.app.wknd.site/adventure${contentFragment.path}` che risolve `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ Un URL di anteprima sul __Articolo__ modello `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` le risoluzioni `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## Anteprima in-app

Per qualsiasi frammento di contenuto che utilizza il modello di frammento di contenuto configurato, è disponibile un pulsante Anteprima. Il pulsante Anteprima apre l’URL di anteprima del modello di frammento di contenuto e inserisce i valori del frammento di contenuto aperto nel [Espressioni URL](#url-expressions).

![Pulsante Anteprima](./assets/preview/preview-button.png)

Esegui un aggiornamento rigido (cancellando la cache locale del browser) durante l’anteprima delle modifiche al frammento di contenuto nell’app.

## React example

Esploriamo l’app WKND, una semplice applicazione React che visualizza le avventure di AEM utilizzando AEM API GraphQL headless.

Il codice di esempio è disponibile in [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL e percorsi

Gli URL o i percorsi utilizzati per visualizzare l’anteprima di un frammento di contenuto devono essere compositi tramite [Espressioni URL](#url-expressions). In questa versione abilitata per l’anteprima dell’app WKND, i frammenti di contenuto dell’avventura vengono visualizzati tramite il `AdventureDetail` componente legato al percorso `/adventure<CONTENT FRAGMENT PATH>`. Pertanto, l’URL di anteprima del modello WKND Adventure deve essere impostato su `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` per risolvere il problema.

L’anteprima dei frammenti di contenuto funziona solo se l’app ha una route indirizzabile, compilabile con [Espressioni URL](#url-expressions) che esegue il rendering del frammento di contenuto nell’app in modo da consentirne l’anteprima.

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Switch>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*'>
            <AdventureDetail />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

export default App;
```

### Visualizzare il contenuto creato

La `AdventureDetail` il componente analizza semplicemente il percorso del frammento di contenuto, inserito nell’URL di anteprima tramite il `${contentFragment.path}` [Espressione URL](#url-expressions), dall’URL del percorso e lo utilizza per raccogliere ed eseguire il rendering dell’avventura WKND.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the content fragment path value which is the parameter used to query for the adventure's details
    
    // Add the leading '/' back on since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const path = '/' + useParams()[0];

    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
