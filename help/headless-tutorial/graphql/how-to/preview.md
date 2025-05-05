---
title: Anteprima frammento di contenuto
description: Scopri come utilizzare l’anteprima dei frammenti di contenuto per tutti gli autori per vedere rapidamente in che modo le modifiche al contenuto influiscono sulle esperienze AEM Headless.
version: Experience Manager as a Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
exl-id: 247d40a3-ff67-4c1f-86bf-3794d7ce3e32
duration: 463
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---

# Anteprima frammento di contenuto

Le applicazioni AEM Headless supportano l’anteprima integrata dell’authoring. L’esperienza di anteprima collega l’editor dei frammenti di contenuto dell’Autore di AEM con l’app personalizzata (indirizzabile tramite HTTP), consentendo un collegamento diretto nell’app che esegue il rendering del frammento di contenuto visualizzato in anteprima.

>[!VIDEO](https://video.tv.adobe.com/v/3449596?quality=12&learn=on&captions=ita)

Per utilizzare l’anteprima del frammento di contenuto, è necessario soddisfare diverse condizioni:

1. L’app deve essere distribuita in un URL accessibile agli autori
1. L’app deve essere configurata per la connessione al servizio AEM Author (anziché al servizio AEM Publish)
1. L&#39;app deve essere progettata con URL o route che possono utilizzare [ID o percorso frammento di contenuto](#url-expressions) per selezionare i frammenti di contenuto da visualizzare in anteprima nell&#39;esperienza dell&#39;app.

## URL di anteprima

Gli URL di anteprima, che utilizzano [espressioni URL](#url-expressions), sono impostati nelle proprietà del modello per frammenti di contenuto.

![URL anteprima modello frammento di contenuto](./assets/preview/cf-model-preview-url.png)

1. Accedi al servizio AEM Author come amministratore
1. Passa a __Strumenti > Generale > Modelli per frammenti di contenuto__
1. Seleziona il __Modello per frammenti di contenuto__ e fai clic su __Proprietà__ nella barra delle azioni superiore.
1. Immetti l&#39;URL di anteprima per il modello per frammenti di contenuto utilizzando [espressioni URL](#url-expressions)
   + L’URL di anteprima deve puntare a una distribuzione dell’app che si connette al servizio AEM Author.

### Espressioni URL

Per ogni modello per frammenti di contenuto può essere impostato un URL di anteprima. L’URL di anteprima può essere parametrizzato per frammento di contenuto utilizzando le espressioni URL elencate nella tabella seguente. È possibile utilizzare più espressioni URL in un singolo URL di anteprima.

|                                         | Espressione URL | Valore |
| --------------------------------------- | ----------------------------------- | ----------- |
| Percorso frammento di contenuto | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| ID frammento di contenuto | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Variante del frammento di contenuto | `${contentFragment.variation}` | `main` |
| Percorso modello per frammenti di contenuto | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Nome modello per frammenti di contenuto | `${contentFragment.model.name}` | `adventure` |

Esempio di URL di anteprima:

+ Un URL di anteprima nel modello __Adventure__ potrebbe essere simile a `https://preview.app.wknd.site/adventure${contentFragment.path}` e viene risolto in `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ Un URL di anteprima nel modello __Article__ potrebbe avere un aspetto simile a `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` che risolve `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## Anteprima in-app

Qualsiasi frammento di contenuto che utilizza il modello per frammenti di contenuto configurato dispone di un pulsante Anteprima. Il pulsante Preview apre l&#39;URL di anteprima del modello per frammenti di contenuto e inserisce i valori del frammento di contenuto aperto nelle [espressioni URL](#url-expressions).

![Pulsante Anteprima](./assets/preview/preview-button.png)

Esegui un aggiornamento rigido (cancellazione della cache locale del browser) quando visualizzi in anteprima le modifiche ai frammenti di contenuto nell’app.

## Esempio di React

Esploriamo l’app WKND, una semplice applicazione React che mostra le avventure di AEM utilizzando le API GraphQL headless di AEM.

Il codice di esempio è disponibile in [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL e route

Gli URL o i percorsi utilizzati per visualizzare in anteprima un frammento di contenuto devono essere componibili utilizzando [espressioni URL](#url-expressions). In questa versione dell&#39;app WKND abilitata per l&#39;anteprima, i frammenti di contenuto dell&#39;avventura vengono visualizzati tramite il componente `AdventureDetail` associato alla route `/adventure<CONTENT FRAGMENT PATH>`. Pertanto, l&#39;URL di anteprima del modello WKND Adventure deve essere impostato su `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` per risolvere questa route.

L&#39;anteprima dei frammenti di contenuto funziona solo se l&#39;app dispone di un percorso indirizzabile, che può essere compilato con [espressioni URL](#url-expressions) che eseguono il rendering del frammento di contenuto nell&#39;app in modo da consentirne l&#39;anteprima.

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
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### Visualizzare il contenuto creato

Il componente `AdventureDetail` analizza semplicemente il percorso del frammento di contenuto, inserito nell&#39;URL di anteprima tramite l&#39;espressione `${contentFragment.path}` [URL](#url-expressions), dall&#39;URL di route e lo utilizza per raccogliere ed eseguire il rendering di WKND Adventure.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
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
