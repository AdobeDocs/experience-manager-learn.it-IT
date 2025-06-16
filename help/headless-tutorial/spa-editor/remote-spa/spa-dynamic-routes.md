---
title: Aggiungere componenti modificabili alle route dinamiche dell'applicazione a pagina singola remota
description: Scopri come aggiungere componenti modificabili alle route dinamiche in un’applicazione a pagina singola remota.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 202
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Percorsi dinamici e componenti modificabili

{{spa-editor-deprecation}}

In questo capitolo vengono abilitate due route dinamiche Adventure Detail per supportare componenti modificabili: __Bali Surf Camp__ e __Beervana a Portland__.

![Route dinamiche e componenti modificabili](./assets/spa-dynamic-routes/intro.png)

La route dell&#39;applicazione a pagina singola Adventure Detail è definita come `/adventure/:slug`, dove `slug` è una proprietà di identificatore univoco nel frammento di contenuto Adventure.

## Mappare gli URL dell’applicazione a pagina singola sulle pagine AEM

Nei due capitoli precedenti, il contenuto dei componenti modificabili è stato mappato dalla vista Home dell&#39;applicazione a pagina singola alla pagina principale dell&#39;applicazione a pagina singola remota corrispondente in AEM all&#39;indirizzo `/content/wknd-app/us/en/`.

La definizione della mappatura per i componenti modificabili per le route dinamiche dell’applicazione a pagina singola è simile, tuttavia è necessario trovare uno schema di mappatura 1:1 tra le istanze della route e le pagine AEM.

In questo tutorial, prendiamo il nome del frammento di contenuto WKND Adventure, che è l’ultimo segmento del percorso, e lo mappiamo su un percorso semplice in `/content/wknd-app/us/en/adventure`.

| Route remota per applicazioni a pagina singola | Percorso pagina AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /avventura/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /avventura/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

Pertanto, in base a questa mappatura, è necessario creare due nuove pagine di AEM all’indirizzo:

* `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
* `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mappatura SPA remota

Il mapping per le richieste che lasciano l&#39;applicazione a pagina singola remota è configurato tramite la configurazione `setupProxy` eseguita in [Bootstrap l&#39;applicazione a pagina singola](./spa-bootstrap.md).

## Mappatura dell’editor SPA

Il mapping per le richieste di applicazioni a pagina singola quando l&#39;applicazione a pagina singola viene aperta tramite l&#39;editor di applicazioni a pagina singola di AEM è configurato tramite la configurazione di mappature Sling eseguita in [Configura AEM](./aem-configure.md).

## Creare pagine di contenuto in AEM

Innanzitutto, crea il segmento di pagina `adventure` intermedio:

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND > Stati Uniti > en > Home page app WKND__
   1. Questa pagina di AEM è mappata come directory principale dell’applicazione a pagina singola, ed è qui che iniziamo a creare la struttura della pagina di AEM per altre route di applicazioni a pagina singola.
1. Tocca __Crea__ e seleziona __Pagina__
1. Seleziona il modello __Pagina applicazione a pagina singola remota__ e tocca __Avanti__
1. Compila le proprietà della pagina
   1. __Titolo__: avventura
   1. __Nome__: `adventure`
      1. Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere al segmento di route dell’applicazione a pagina singola.
1. Tocca __Fine__

Quindi, crea le pagine AEM corrispondenti a ciascuno degli URL dell’applicazione a pagina singola che richiedono aree modificabili.

1. Accedi alla nuova pagina __Avventura__ nell&#39;amministratore del sito
1. Tocca __Crea__ e seleziona __Pagina__
1. Seleziona il modello __Pagina applicazione a pagina singola remota__ e tocca __Avanti__
1. Compila le proprietà della pagina
   1. __Titolo__: Bali Surf Camp
   1. __Nome__: `bali-surf-camp`
      1. Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere all’ultimo segmento della route dell’applicazione a pagina singola
1. Tocca __Fine__
1. Ripeti i passaggi 3-6 per creare la pagina __Beervana in Portland__, con:
   1. __Titolo__: Beervana a Portland
   1. __Nome__: `beervana-in-portland`
      1. Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere all’ultimo segmento della route dell’applicazione a pagina singola

Queste due pagine di AEM contengono i rispettivi contenuti creati per le route SPA corrispondenti. Se altre route di applicazioni a pagina singola richiedono l&#39;authoring, è necessario creare nuove pagine AEM nell&#39;URL dell&#39;applicazione a pagina singola nell&#39;apposita pagina radice (`/content/wknd-app/us/en/home`) in AEM.

## Aggiornare l’app WKND

Inseriamo il componente `<ResponsiveGrid...>` creato nel [ultimo capitolo](./spa-container-component.md) nel nostro componente `AdventureDetail` per applicazioni a pagina singola, creando un contenitore modificabile.

### Posizionare il componente SPA ResponsiveGrid

Il posizionamento di `<ResponsiveGrid...>` nel componente `AdventureDetail` crea un contenitore modificabile in tale route. Il trucco è dovuto al fatto che più route utilizzano il componente `AdventureDetail` per il rendering. È necessario regolare dinamicamente l&#39;attributo `<ResponsiveGrid...>'s pagePath`. `pagePath` deve essere derivato per puntare alla pagina AEM corrispondente, in base all&#39;avventura visualizzata dall&#39;istanza del percorso.

1. Apri e modifica `react-app-/src/components/AdventureDetail.js`
1. Importare il componente `ResponsiveGrid` e posizionarlo sopra il componente `<h2>Itinerary</h2>`.
1. Impostare i seguenti attributi sul componente `<ResponsiveGrid...>`. L&#39;attributo `pagePath` aggiunge l&#39;attuale `slug` che viene mappato sulla pagina dell&#39;avventura in base alla mappatura definita sopra.
   1. `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   1. `itemPath = 'root/responsivegrid'`

   Questo indica al componente `ResponsiveGrid` di recuperare il contenuto dalla risorsa AEM:

   1. `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

Aggiorna `AdventureDetail.js` con le righe seguenti:

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

Il file `AdventureDetail.js` deve essere simile al seguente:

![DettagliAvventura.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Creare il contenitore in AEM

Con `<ResponsiveGrid...>` attivo e `pagePath` impostato in modo dinamico in base all&#39;avventura di cui viene eseguito il rendering, si prova a creare contenuti al suo interno.

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND > us > en__
1. __Modifica__ la pagina __Home page app WKND__
   1. Passa alla route __Bali Surf Camp__ nell&#39;applicazione a pagina singola per modificarla
1. Seleziona __Anteprima__ dal selettore modalità in alto a destra
1. Tocca la scheda __Bali Surf Camp__ nell&#39;applicazione a pagina singola per passare alla relativa route
1. Seleziona __Modifica__ dal selettore modalità
1. Individua l&#39;area modificabile __Contenitore di layout__ proprio sopra il __Itinerario__
1. Apri la barra laterale dell&#39;__Editor pagine__ e seleziona la __visualizzazione Componenti__
1. Trascina alcuni dei componenti abilitati in __Contenitore di layout__
   1. Immagine
   1. Testo
   1. Titolo

   E crea del materiale promozionale per il marketing. Potrebbe presentarsi così:

   ![Authoring dei dettagli dell&#39;avventura di Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Visualizza in anteprima__ le modifiche nell&#39;Editor pagina di AEM
1. Aggiorna l&#39;app WKND in esecuzione localmente su [http://localhost:3000](http://localhost:3000), passa alla route __Bali Surf Camp__ per visualizzare le modifiche create.

   ![Bali applicazione a pagina singola remota](./assets/spa-dynamic-routes/remote-spa-final.png)

Quando si passa a un percorso di dettaglio avventura privo di una pagina AEM mappata, non è possibile creare nell’istanza del percorso. Per abilitare l&#39;authoring in queste pagine, crea semplicemente una pagina AEM con il nome corrispondente nella pagina __Avventura__.

## Congratulazioni.

Congratulazioni Hai aggiunto la possibilità di authoring per le route dinamiche nell’applicazione a pagina singola.

* Aggiunta del componente ResponsiveGrid del componente AEM React Editable a una route dinamica
* Sono state create pagine AEM per supportare l’authoring di due percorsi specifici nell’applicazione a pagina singola (Bali Surf Camp e Beervana a Portland)
* Contenuti creati sulla strada dinamica Bali Surf Camp!

Ora hai completato l’esplorazione dei primi passaggi su come utilizzare l’Editor delle applicazioni a pagina singola di AEM per aggiungere specifiche aree modificabili a un’applicazione a pagina singola remota.
