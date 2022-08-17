---
title: Aggiungere componenti modificabili ai percorsi dinamici SPA remoti
description: Scopri come aggiungere componenti modificabili ai percorsi dinamici in un SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 1%

---

# Indirizzi dinamici e componenti modificabili

In questo capitolo, offriamo due percorsi dinamici di Adventure Detail per supportare i componenti modificabili; __Bali Surf Camp__ e __Beervana a Portland__.

![Indirizzi dinamici e componenti modificabili](./assets/spa-dynamic-routes/intro.png)

Il percorso di Adventure Detail SPA è definito come `/adventure:path` dove `path` è il percorso dell’avventura WKND (frammento di contenuto) su cui visualizzare i dettagli.

## Mappare gli URL SPA alle pagine AEM

Nei due capitoli precedenti, abbiamo mappato il contenuto del componente modificabile dalla vista Home SPA alla corrispondente pagina radice SPA remota in AEM in `/content/wknd-app/us/en/`.

La definizione della mappatura per i componenti modificabili per i percorsi dinamici SPA è simile, tuttavia è necessario presentare uno schema di mappatura 1:1 tra le istanze della route e le pagine AEM.

In questa esercitazione, prendiamo il nome del frammento di contenuto dell’avventura WKND, che è l’ultimo segmento del percorso, e lo mappamo su un semplice percorso in `/content/wknd-app/us/en/adventure`.

| Percorso SPA remoto | Percorso pagina AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /avventura:/content/dam/wknd/en/adventures/bali-surf-camp/__campo surf-bali__ | /content/wknd-app/us/en/home/adventure/__campo surf-bali__ |
| /avventura:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

Quindi, in base a questa mappatura, dobbiamo creare due nuove pagine AEM in:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mappatura SPA remota

La mappatura per le richieste che lasciano il SPA remoto viene configurata tramite il `setupProxy` configurazione eseguita in [Bootstrap il SPA](./spa-bootstrap.md).

## Mappatura dell’editor SPA

La mappatura per le richieste di SPA quando il SPA viene aperto tramite AEM Editor viene configurata tramite la configurazione di mappature Sling eseguita in [Configurare AEM](./aem-configure.md).

## Creare pagine di contenuto in AEM

Innanzitutto, crea l&#39;intermediario `adventure` Segmento pagina:

1. Accedi ad AEM Author
1. Passa a __Siti > App WKND > noi > en > WKND App Home Page__
   + Questa pagina AEM è mappata come radice del SPA, quindi questo è il punto in cui iniziamo a creare la struttura della pagina AEM per altri percorsi SPA.
1. Tocca __Crea__ e seleziona __Pagina__
1. Seleziona la __Pagina SPA remota__ modello e tocca __Successivo__
1. Compila le proprietà della pagina
   + __Titolo__: Avventura
   + __Nome__: `adventure`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere al segmento di route del SPA.
1. Toccate __Chiudi__

Quindi, crea le pagine AEM corrispondenti a ciascuno degli URL SPA che richiedono aree modificabili.

1. Passa alla nuova __Avventura__ in Site Admin
1. Tocca __Crea__ e seleziona __Pagina__
1. Seleziona la __Pagina SPA remota__ modello e tocca __Successivo__
1. Compila le proprietà della pagina
   + __Titolo__: Bali Surf Camp
   + __Nome__: `bali-surf-camp`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere all’ultimo segmento della route del SPA
1. Toccate __Chiudi__
1. Ripeti i passaggi 3-6 per creare il __Beervana a Portland__ con:
   + __Titolo__: Beervana a Portland
   + __Nome__: `beervana-in-portland`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere all’ultimo segmento della route del SPA

Queste due pagine AEM contengono il contenuto rispettivo creato per i percorsi SPA corrispondenti. Se altri percorsi SPA richiedono l’authoring, è necessario creare nuove pagine AEM nell’URL SPA sotto la pagina principale della pagina SPA remota (`/content/wknd-app/us/en/home`) in AEM.

## Aggiornare l’app WKND

Mettiamo il `<AEMResponsiveGrid...>` creato in [ultimo capitolo](./spa-container-component.md)nella `AdventureDetail` SPA componente, creazione di un contenitore modificabile.

### Posiziona il componente SPA AEMResponsiveGrid

Posizionamento del `<AEMResponsiveGrid...>` in `AdventureDetail` in questo percorso viene creato un contenitore modificabile. Il trucco è dovuto al fatto che più percorsi utilizzano il `AdventureDetail` per eseguire il rendering, è necessario regolare dinamicamente il  `<AEMResponsiveGrid...>'s pagePath` attributo. La `pagePath` deve essere derivato per puntare alla pagina di AEM corrispondente, in base all&#39;avventura che viene visualizzata l&#39;istanza del percorso.

1. Apri e modifica `react-app/src/components/AdventureDetail.js`
1. Aggiungi la riga seguente prima di `AdventureDetail(..)'s` secondo `return(..)` , che deriva il nome dell’avventura dal percorso del frammento di contenuto.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = _path.split('/').pop();
   ...
   ```

1. Importa `AEMResponsiveGrid` e posizionarlo sopra il `<h2>Itinerary</h2>` componente.
1. Imposta i seguenti attributi nel `<AEMResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Questo istruisce il `AEMResponsiveGrid` per recuperare il relativo contenuto dalla risorsa AEM:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Aggiorna `AdventureDetail.js` con le seguenti linee:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = _path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

La `AdventureDetail.js` dovrebbe essere simile a:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Crea il contenitore in AEM

Con la `<AEMResponsiveGrid...>` sul posto e `pagePath` impostati in modo dinamico in base all’avventura di cui si esegue il rendering, proviamo a creare contenuti al suo interno.

1. Accedi ad AEM Author
1. Passa a __Sites > WKND App > noi > it__
1. __Modifica__ la __Home page app WKND__ page
   + Passa a __Bali Surf Camp__ instradare il SPA per modificarlo
1. Seleziona __Anteprima__ dal selettore modalità in alto a destra
1. Tocca __Bali Surf Camp__ scheda nella SPA per passare al percorso
1. Seleziona __Modifica__ dal selettore modalità
1. Individua il __Contenitore di layout__ area modificabile sopra __Itinerario__
1. Apri __Barra laterale dell’Editor pagina__, quindi seleziona la __Vista Componenti__
1. Trascina alcuni dei componenti abilitati nella sezione __Contenitore di layout__
   + Immagine
   + Testo
   + Titolo

   E crea del materiale di marketing promozionale. Potrebbe assomigliare a questo:

   ![Authoring dei dettagli dell&#39;avventura di Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Anteprima__ le modifiche in AEM Editor pagina
1. Aggiorna l’app WKND in esecuzione localmente su [http://localhost:3000](Http://localhost:3000), passa alla __Bali Surf Camp__ route per vedere le modifiche create!

   ![Bali SPA remoto](./assets/spa-dynamic-routes/remote-spa-final.png)

Quando si passa a un percorso di dettaglio dell’avventura che non ha una pagina di AEM mappata, non è disponibile alcuna funzionalità di authoring in tale istanza di percorso. Per abilitare l’authoring su queste pagine, è sufficiente creare una pagina AEM con il nome corrispondente sotto il __Avventura__ pagina!

## Congratulazioni!

Congratulazioni! Hai aggiunto la possibilità di authoring ai percorsi dinamici nel SPA!

+ È stato aggiunto il componente ResponsiveGrid del componente AEM React Modificable a un percorso dinamico
+ Sono state create AEM pagine per supportare l’authoring di due percorsi specifici nel SPA (Bali Surf Camp e Beervana a Portland)
+ Contenuto scritto sulla via dinamica Bali Surf Camp!

Hai completato l’esplorazione dei primi passaggi di come AEM editor di SPA può essere utilizzato per aggiungere aree modificabili specifiche a un SPA remoto.


>[!NOTE]
>
>Rimanete sintonizzati! Questa esercitazione verrà espansa per includere le best practice e raccomandazioni di Adobe su come distribuire la soluzione SPA Editor agli ambienti di produzione e as a Cloud Service AEM.
