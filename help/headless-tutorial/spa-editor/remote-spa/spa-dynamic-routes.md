---
title: Aggiungere componenti modificabili ai percorsi dinamici SPA remoti
description: Scopri come aggiungere componenti modificabili ai percorsi dinamici in un SPA remoto.
topic: Senza testa, SPA, Sviluppo
feature: Editor SPA, componenti core, API, sviluppo
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 1%

---


# Indirizzi dinamici e componenti modificabili

In questo capitolo, offriamo due percorsi dinamici di Adventure Detail per supportare i componenti modificabili; __Bali Surf Camp__ e __Beervana in Portland__.

![Indirizzi dinamici e componenti modificabili](./assets/spa-dynamic-routes/intro.png)

Il percorso di Adventure Detail SPA è definito come `/adventure:path` dove `path` rappresenta il percorso dell&#39;avventura WKND (frammento di contenuto) su cui visualizzare i dettagli.

## Mappare gli URL SPA alle pagine AEM

Nei due capitoli precedenti, abbiamo mappato il contenuto del componente modificabile dalla vista Home SPA alla corrispondente pagina radice SPA remota in AEM in `/content/wknd-app/us/en/`.

La definizione della mappatura per i componenti modificabili per i percorsi dinamici SPA è simile, tuttavia è necessario presentare uno schema di mappatura 1:1 tra le istanze della route e le pagine AEM.

In questa esercitazione, prenderemo il nome del frammento di contenuto dell’avventura WKND, che è l’ultimo segmento del percorso, e lo mapperemo su un percorso semplice in `/content/wknd-app/us/en/adventure`.

| Percorso SPA remoto | Percorso pagina AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /avventura:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/it/home/adventure/__bali-surf-camp__ |
| /avventura:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/it/home/adventure/__beervana-in-portland__ |

Quindi, in base a questa mappatura, dobbiamo creare due nuove pagine AEM in:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mappatura SPA remota

La mappatura per le richieste che lasciano il SPA remoto è configurata tramite la configurazione `setupProxy` eseguita in [Bootstrap il SPA](./spa-bootstrap.md).

## Mappatura dell’editor SPA

La mappatura per le richieste di SPA quando il SPA viene aperto tramite AEM Editor viene configurata tramite la configurazione di mappature Sling eseguita in [Configura AEM](./aem-configure.md).

## Creare pagine di contenuto in AEM

Innanzitutto, crea il segmento di pagina intermedio `adventure` :

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND > us > en > Home page dell&#39;app WKND__
   + Questa pagina AEM è mappata come radice del SPA, quindi questo è il punto in cui iniziamo a creare la struttura della pagina AEM per altri percorsi SPA.
1. Tocca __Crea__ e seleziona __Pagina__
1. Seleziona il modello __Pagina SPA remota__ e tocca __Avanti__
1. Compila le proprietà della pagina
   + __Titolo__: Avventura
   + __Nome__: `adventure`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere al segmento di route del SPA.
1. Toccate __Chiudi__

Quindi, crea le pagine AEM corrispondenti a ciascuno degli URL SPA che richiedono aree modificabili.

1. Passa alla nuova pagina __Avventura__ nell’amministratore del sito
1. Tocca __Crea__ e seleziona __Pagina__
1. Seleziona il modello __Pagina SPA remota__ e tocca __Avanti__
1. Compila le proprietà della pagina
   + __Titolo__: Bali Surf Camp
   + __Nome__: `bali-surf-camp`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere all’ultimo segmento della route del SPA
1. Toccate __Chiudi__
1. Ripeti i passaggi 3-6 per creare la pagina __Beervana in Portland__ con:
   + __Titolo__: Beervana a Portland
   + __Nome__: `beervana-in-portland`
      + Questo valore definisce l’URL della pagina AEM e deve quindi corrispondere all’ultimo segmento della route del SPA

Queste due pagine AEM contengono il rispettivo contenuto creato per i percorsi SPA corrispondenti. Se altre SPA route richiedono l&#39;authoring, è necessario creare nuove pagine AEM nell&#39;URL SPA sotto la pagina principale della pagina SPA remota (`/content/wknd-app/us/en/home`) in AEM.

## Aggiornare l’app WKND

Posizioniamo il componente `<AEMResponsiveGrid...>` creato nell&#39; [ultimo capitolo](./spa-container-component.md) nel componente `AdventureDetail` SPA, creando un contenitore modificabile.

### Posiziona il componente SPA AEMResponsiveGrid

Posizionando il `<AEMResponsiveGrid...>` nel componente `AdventureDetail` si crea un contenitore modificabile in tale percorso. Il problema è che, poiché per il rendering vengono utilizzati più percorsi, è necessario regolare dinamicamente l&#39;attributo `<AEMResponsiveGrid...>'s pagePath`. `AdventureDetail` Il `pagePath` deve essere derivato per puntare alla pagina di AEM corrispondente, in base all’avventura che viene visualizzata l’istanza del percorso.

1. Apri e modifica `react-app/src/components/AdventureDetail.js`
1. Aggiungi la riga seguente prima della seconda istruzione `AdventureDetail(..)'s` `return(..)` , che deriva il nome dell’avventura dal percorso del frammento di contenuto.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. Importa il componente `AEMResponsiveGrid` e posizionalo sopra il componente `<h2>Itinerary</h2>`.
1. Imposta i seguenti attributi sul componente `<AEMResponsiveGrid...>`
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Questo indica al componente `AEMResponsiveGrid` di recuperare il relativo contenuto dalla risorsa AEM:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Aggiorna `AdventureDetail.js` con le seguenti righe:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = adventureData._path.split('/').pop();

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

Il file `AdventureDetail.js` deve essere simile al seguente:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Crea il contenitore in AEM

Con il `<AEMResponsiveGrid...>` attivo e il relativo `pagePath` impostato dinamicamente in base all’avventura di cui si sta eseguendo il rendering, proviamo a creare contenuti al suo interno.

1. Accedi ad AEM Author
1. Passa a __Sites > App WKND > us > en__
1. ____ Modifica della home  __page dell’app__ WKND
   + Passa alla route __Bali Surf Camp__ nella SPA per modificarla
1. Seleziona __Anteprima__ dal selettore della modalità in alto a destra
1. Toccare la scheda __Bali Surf Camp__ nell&#39;SPA per passare al suo percorso
1. Seleziona __Modifica__ dal selettore della modalità
1. Individua l’ area modificabile __Contenitore di layout__ posta immediatamente sopra l’ __Itinerario__
1. Apri la __barra laterale dell&#39;Editor pagina__ e seleziona la __vista Componenti__
1. Trascina alcuni dei componenti abilitati nel __Contenitore di layout__
   + Immagine
   + Testo
   + Titolo

   E crea del materiale di marketing promozionale. Potrebbe assomigliare a questo:

   ![Authoring dei dettagli dell&#39;avventura di Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. ____ Anteprima delle modifiche in AEM Editor pagina
1. Aggiorna l&#39;app WKND in esecuzione localmente su [http://localhost:3000](Http://localhost:3000), vai alla route __Bali Surf Camp__ per vedere le modifiche create!

   ![Bali SPA remoto](./assets/spa-dynamic-routes/remote-spa-final.png)

Quando passi a un percorso di dettaglio dell’avventura che non ha una pagina AEM mappata, non ci sarà alcuna funzionalità di authoring su quell’istanza di percorso. Per abilitare l&#39;authoring su queste pagine, è sufficiente creare una pagina AEM con il nome corrispondente nella pagina __Avventura__!

## Congratulazioni!

Congratulazioni! Hai aggiunto la possibilità di authoring ai percorsi dinamici nel SPA!

+ È stato aggiunto il componente ResponsiveGrid del componente AEM React Modificable a un percorso dinamico
+ Sono state create AEM pagine per supportare l’authoring di due percorsi specifici nel SPA (Bali Surf Camp e Beervana a Portland)
+ Contenuto scritto sulla via dinamica Bali Surf Camp!

Hai completato l’esplorazione dei primi passaggi di come AEM editor di SPA può essere utilizzato per aggiungere aree modificabili specifiche a un SPA remoto.


>[!NOTE]
>
>Rimanete sintonizzati! Questa esercitazione verrà espansa per includere le best practice e raccomandazioni di Adobe su come distribuire la soluzione SPA Editor in AEM come Cloud Service e ambienti di produzione.