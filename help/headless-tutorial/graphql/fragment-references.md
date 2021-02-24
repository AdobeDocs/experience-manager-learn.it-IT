---
title: Modellazione dati avanzata con riferimenti a frammenti - Guida introduttiva a AEM senza titolo - GraphQL
description: Guida introduttiva ad Adobe Experience Manager (AEM) e GraphQL. Scoprite come utilizzare la funzione Riferimento frammento per la modellazione avanzata dei dati e per creare una relazione tra due diversi frammenti di contenuto. Scoprite come modificare una query GraphQL per includere un campo da un modello di riferimento.
sub-product: assets
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
translation-type: tm+mt
source-git-commit: ce4a35f763862c6d6a42795fd5e79d9c59ff645a
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 1%

---


# Modellazione dati avanzata con i riferimenti ai frammenti

È possibile fare riferimento a un frammento di contenuto dall&#39;interno di un altro frammento di contenuto. Questo consente all&#39;utente di creare complessi modelli di dati con relazioni tra frammenti.

In questo capitolo verrà aggiornato il modello Avventura per includere un riferimento al modello Collaboratore utilizzando il campo **Riferimento frammento**. Verrà inoltre illustrato come modificare una query GraphQL per includere campi da un modello di riferimento.

## Prerequisiti

Si tratta di un&#39;esercitazione con più parti e si presume che i passaggi descritti nelle parti precedenti siano stati completati.

## Obiettivi

In questo capitolo impareremo come:

* Aggiornare un modello di frammento di contenuto per utilizzare il campo Riferimento frammento
* Creare una query GraphQL che restituisce campi da un modello di riferimento

## Aggiungere un riferimento a un frammento {#add-fragment-reference}

Aggiorna il modello di frammento di contenuto di avventura per aggiungere un riferimento al modello Collaboratore.

1. Aprite un nuovo browser e andate a AEM.
1. Dal menu **AEM Start** passare a **Strumenti** > **Risorse** > **Modelli di frammenti di contenuto** > **Sito WKND**.
1. Aprire il modello di frammento di contenuto **Adventure**

   ![Aprire il modello di frammento di contenuto di avventura](assets/fragment-references/adventure-content-fragment-edit.png)

1. In **Tipi di dati**, trascinare un campo **Riferimento frammento** nel pannello principale.

   ![Campo Aggiungi riferimento frammento](assets/fragment-references/add-fragment-reference-field.png)

1. Aggiorna le **proprietà** per questo campo con le seguenti informazioni:

   * Rendering come - `fragmentreference`
   * Etichetta campo - **Collaboratore avventura**
   * Nome proprietà - `adventureContributor`
   * Tipo di modello - Selezionare il modello **Collaboratore**
   * Percorso directory principale - `/content/dam/wknd`

   ![Proprietà dei riferimenti ai frammenti](assets/fragment-references/fragment-reference-properties.png)

   Ora è possibile utilizzare il nome della proprietà `adventureContributor` per fare riferimento a un frammento di contenuto del collaboratore.

1. Salvare le modifiche apportate al modello.

## Assegnare un collaboratore a un&#39;avventura

Ora che il modello di frammento di contenuto di avventura è stato aggiornato, è possibile modificare un frammento esistente e fare riferimento a un collaboratore. È importante notare che la modifica del modello di frammento di contenuto *ha effetto su* eventuali frammenti di contenuto esistenti creati da esso.

1. Passare a **Risorse** > **File** > **WKND Site** > **English** > **Avventure** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Cartella Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Fare clic sul frammento di contenuto **Bali Surf Camp** per aprire l&#39;Editor frammento di contenuto.
1. Aggiorna il campo **Adventure Contributor** e seleziona un collaboratore facendo clic sull’icona della cartella.

   ![Seleziona Stacey Roswells come collaboratore](assets/fragment-references/stacey-roswell-contributor.png)

   *Selezionare il percorso di un frammento collaboratore*

   ![percorso popolato per il collaboratore](assets/fragment-references/populated-path.png)

   Tenere presente che è possibile selezionare solo i frammenti creati utilizzando il modello **Collaboratore**.

1. Salvare le modifiche apportate al frammento.

1. Ripetere i passaggi indicati per assegnare un collaboratore ad avventure come [Yosemite Backpack](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) e [Colorado Rock Climbing](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)

## Query di frammento di contenuto nidificato con GraphiQL

Eseguire quindi una query per un&#39;avventura e aggiungere proprietà nidificate del modello Collaboratore a cui viene fatto riferimento. Verrà utilizzato lo strumento GraphiQL per verificare rapidamente la sintassi della query.

1. Passa allo strumento GraphiQL in AEM: [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. Inserite la seguente query:

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   La query di cui sopra è per una singola avventura per il suo percorso. La proprietà `adventureContributor` fa riferimento al modello Collaboratore ed è possibile richiedere le proprietà dal frammento di contenuto nidificato.

1. Esegui la query e ottieni un risultato come quello riportato di seguito:

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. Prova con altre query come `adventureList` e aggiungi proprietà per il frammento di contenuto di riferimento in `adventureContributor`.

## Aggiornare l&#39;app React per visualizzare il contenuto Collaboratore

Quindi, aggiorna le query utilizzate dall’applicazione React per includere il nuovo collaboratore e visualizza informazioni sul collaboratore come parte della visualizzazione Dettagli di Avventura.

1. Aprite l’app WKND GraphQL React nell’IDE.

1. Aprire il file `src/components/AdventureDetail.js`.

   ![Componente Adventure Detail IDE](assets/fragment-references/adventure-detail-ide.png)

1. Trovare la funzione `adventureDetailQuery(_path)`. La funzione `adventureDetailQuery(..)` racchiude semplicemente una query GraphQL di filtro, che utilizza AEM sintassi `<modelName>ByPath` per eseguire una query su un singolo frammento di contenuto identificato dal relativo percorso JCR.

1. Aggiorna la query per includere informazioni sul collaboratore a cui viene fatto riferimento:

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
           item {
               _path
               adventureTitle
               adventureActivity
               adventureType
               adventurePrice
               adventureTripLength
               adventureGroupSize
               adventureDifficulty
               adventurePrice
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   Con questo aggiornamento, nella query verranno incluse ulteriori proprietà relative a `adventureContributor`, `fullName`, `occupation` e `pictureReference`.

1.  Inspect il componente `Contributor` incorporato nel file `AdventureDetail.js` in `function Contributor(...)`. Questo componente renderà il nome, l’occupazione e l’immagine del collaboratore, se le proprietà esistono.

   Il componente `Contributor` è indicato nel metodo `AdventureDetail(...)` `return`:

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. Salvare le modifiche apportate al file.
1. Avviate React App, se non è già in esecuzione:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Passa a [http://localhost:3000](Http://localhost:3000/) e fai clic su un&#39;avventura con un collaboratore a cui viene fatto riferimento. È ora possibile visualizzare le informazioni sui collaboratori elencate sotto l&#39; **Itinerario**:

   ![Collaboratore aggiunto nell&#39;app](assets/fragment-references/contributor-added-detail.png)

## Congratulazioni!{#congratulations}

Congratulazioni! È stato aggiornato un modello di frammento di contenuto esistente per fare riferimento a un frammento di contenuto nidificato utilizzando il campo **Riferimento frammento**. È stato inoltre appreso come modificare una query GraphQL per includere campi da un modello di riferimento.

## Passaggi successivi {#next-steps}

Nel capitolo successivo, [Implementazione produzione tramite un ambiente AEM Publish](./production-deployment.md), scopri i servizi AEM Author e Publish e il pattern di distribuzione consigliato per le applicazioni headless. Aggiornerete un&#39;applicazione esistente per utilizzare le variabili di ambiente per modificare in modo dinamico un endpoint GraphQL in base all&#39;ambiente di destinazione. Inoltre verrà illustrato come configurare AEM per la condivisione delle risorse tra le origini (CORS).
