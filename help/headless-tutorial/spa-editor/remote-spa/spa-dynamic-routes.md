---
title: Aggiunta di componenti modificabili alle route dinamiche SPA remote
description: Scopri come aggiungere componenti modificabili alle route dinamiche in un SPA remoto.
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Percorsi dinamici e componenti modificabili

In questo capitolo vengono abilitate due route dinamiche Adventure Detail per supportare componenti modificabili: __Bali Surf Camp__ e __Beervana a Portland__.

![Route dinamiche e componenti modificabili](./assets/spa-dynamic-routes/intro.png)

La route SPA Adventure Detail è definita come `/adventure/:slug`, dove `slug` è una proprietà di identificatore univoco nel frammento di contenuto Adventure.

## Mappare gli URL dell’SPA alle pagine AEM

Nei due capitoli precedenti, il contenuto dei componenti modificabili è stato mappato dalla visualizzazione Home dell&#39;SPA alla corrispondente pagina principale dell&#39;SPA remoto nell&#39;AEM in `/content/wknd-app/us/en/`.

La definizione della mappatura per i componenti modificabili per le route dinamiche SPA è simile, tuttavia è necessario elaborare uno schema di mappatura 1:1 tra le istanze della route e le pagine AEM.

In questo tutorial, prendiamo il nome del frammento di contenuto WKND Adventure, che è l’ultimo segmento del percorso, e lo mappiamo su un percorso semplice in `/content/wknd-app/us/en/adventure`.

| Route SPA remota | Percorso pagina AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /avventura/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /avventura/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

Quindi, sulla base di questa mappatura, dobbiamo creare due nuove pagine AEM all&#39;indirizzo:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mappatura remota SPA

La mappatura per le richieste che lasciano l&#39;SPA remoto è configurata tramite la configurazione `setupProxy` eseguita in [Bootstrap SPA](./spa-bootstrap.md).

## Mappatura dell’editor SPA

La mappatura per le richieste SPA quando l&#39;SPA viene aperto tramite l&#39;editor AEM SPA è configurata tramite la configurazione di mappature Sling eseguita in [Configurazione AEM](./aem-configure.md).

## Creare pagine di contenuti in AEM

Innanzitutto, crea il segmento di pagina `adventure` intermedio:

1. Accedi a AEM Author
1. Passa a __Sites > App WKND > Stati Uniti > en > Home page app WKND__
   + Questa pagina dell&#39;AEM è mappata come la radice dell&#39;SPA, ed è qui che iniziamo a costruire la struttura della pagina dell&#39;AEM per altre vie SPA.
1. Tocca __Crea__ e seleziona __Pagina__
1. Seleziona il modello __Pagina SPA remota__ e tocca __Avanti__
1. Compila le proprietà della pagina
   + __Titolo__: avventura
   + __Nome__: `adventure`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere al segmento di route dell’SPA.
1. Tocca __Fine__

Quindi, crea le pagine AEM corrispondenti a ciascuno degli URL SPA che richiedono aree modificabili.

1. Accedi alla nuova pagina __Avventura__ nell&#39;amministratore del sito
1. Tocca __Crea__ e seleziona __Pagina__
1. Seleziona il modello __Pagina SPA remota__ e tocca __Avanti__
1. Compila le proprietà della pagina
   + __Titolo__: Bali Surf Camp
   + __Nome__: `bali-surf-camp`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere all’ultimo segmento della route dell’SPA
1. Tocca __Fine__
1. Ripeti i passaggi 3-6 per creare la pagina __Beervana in Portland__, con:
   + __Titolo__: Beervana a Portland
   + __Nome__: `beervana-in-portland`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere all’ultimo segmento della route dell’SPA

Queste due pagine dell’AEM contengono i rispettivi contenuti creati per le rispettive vie dell’SPA. Se altre route SPA richiedono l&#39;authoring, è necessario creare nuove pagine AEM con il relativo URL SPA nella pagina principale della pagina SPA remota (`/content/wknd-app/us/en/home`) in AEM.

## Aggiornare l’app WKND

Inseriamo il componente `<ResponsiveGrid...>` creato nell&#39;[ultimo capitolo](./spa-container-component.md) nel componente `AdventureDetail` dell&#39;SPA, creando un contenitore modificabile.

### Posizionare il componente SPA ResponsiveGrid

Il posizionamento di `<ResponsiveGrid...>` nel componente `AdventureDetail` crea un contenitore modificabile in tale route. Il trucco è dovuto al fatto che più route utilizzano il componente `AdventureDetail` per il rendering. È necessario regolare dinamicamente l&#39;attributo `<ResponsiveGrid...>'s pagePath`. `pagePath` deve essere derivato per puntare alla pagina AEM corrispondente, in base all&#39;avventura visualizzata dall&#39;istanza della route.

1. Apri e modifica `react-app-/src/components/AdventureDetail.js`
1. Importare il componente `ResponsiveGrid` e posizionarlo sopra il componente `<h2>Itinerary</h2>`.
1. Impostare i seguenti attributi sul componente `<ResponsiveGrid...>`. L&#39;attributo `pagePath` aggiunge l&#39;attuale `slug` che viene mappato sulla pagina dell&#39;avventura in base alla mappatura definita sopra.
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   Questo indica al componente `ResponsiveGrid` di recuperare il contenuto dalla risorsa AEM:

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

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

1. Accedi a AEM Author
1. Passa a __Sites > App WKND > us > en__
1. __Modifica__ la pagina __Home page app WKND__
   + Passa alla route __Bali Surf Camp__ nell&#39;SPA per modificarla
1. Seleziona __Anteprima__ dal selettore modalità in alto a destra
1. Tocca la scheda __Bali Surf Camp__ nell&#39;SPA per passare alla sua route
1. Seleziona __Modifica__ dal selettore modalità
1. Individua l&#39;area modificabile __Contenitore di layout__ proprio sopra il __Itinerario__
1. Apri la barra laterale dell&#39;__Editor pagine__ e seleziona la __visualizzazione Componenti__
1. Trascina alcuni dei componenti abilitati in __Contenitore di layout__
   + Immagine
   + Testo
   + Titolo

   E crea del materiale promozionale per il marketing. Potrebbe presentarsi così:

   ![Authoring dei dettagli dell&#39;avventura di Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Visualizzare in anteprima__ le modifiche nell&#39;Editor pagina AEM
1. Aggiorna l&#39;app WKND in esecuzione localmente su [http://localhost:3000](http://localhost:3000), passa alla route __Bali Surf Camp__ per visualizzare le modifiche create.

   ![Bali SPA remoto](./assets/spa-dynamic-routes/remote-spa-final.png)

Quando si passa a un percorso di dettagli dell’avventura privo di una pagina AEM mappata, non è possibile creare l’istanza del percorso. Per abilitare l&#39;authoring in queste pagine, crea semplicemente una pagina AEM con il nome corrispondente nella pagina __Avventura__.

## Congratulazioni.

Congratulazioni Hai aggiunto la possibilità di authoring per le route dinamiche nell&#39;SPA.

+ È stato aggiunto il componente ResponsiveGrid del componente React Editable dell’AEM a una route dinamica
+ Sono state create pagine AEM per supportare la creazione di due percorsi specifici nell&#39;SPA (Bali Surf Camp e Beervana a Portland)
+ Contenuti creati sulla strada dinamica Bali Surf Camp!

Ora hai completato l’esplorazione dei primi passaggi su come utilizzare l’Editor SPA dell’AEM per aggiungere specifiche aree modificabili a un SPA remoto.
