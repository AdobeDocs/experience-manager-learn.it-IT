---
title: Modellazione dati avanzata con riferimenti a frammenti - Guida introduttiva a AEM Headless - GraphQL
description: Guida introduttiva ad Adobe Experience Manager (AEM) e GraphQL. Scopri come utilizzare la funzione Riferimento frammento per la modellazione avanzata dei dati e come creare una relazione tra due diversi frammenti di contenuto. Scopri come modificare una query GraphQL per includere un campo da un modello di riferimento.
version: Cloud Service
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d85b7ac3-42c1-4655-9394-29a797c0e1d7
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '847'
ht-degree: 1%

---

# Modellazione dati avanzata con riferimenti a frammenti

È possibile fare riferimento a un frammento di contenuto all’interno di un altro frammento di contenuto. Questo consente all’utente di creare modelli di dati complessi con relazioni tra i frammenti.

In questo capitolo verrà aggiornato il modello Avventura per includere un riferimento al modello Collaboratore utilizzando **Riferimento frammento** campo . Inoltre verrà illustrato come modificare una query GraphQL per includere campi da un modello di riferimento.

## Prerequisiti

Si tratta di un tutorial in più parti e si presume che i passaggi descritti nelle parti precedenti siano stati completati.

## Obiettivi

In questo capitolo impareremo come:

* Aggiornare un modello di frammento di contenuto per utilizzare il campo Riferimento frammento
* Crea una query GraphQL che restituisce campi da un modello di riferimento

## Aggiungere un riferimento a un frammento {#add-fragment-reference}

Aggiorna il modello di frammento di contenuto avventura per aggiungere un riferimento al modello Collaboratore.

1. Apri un nuovo browser e passa a AEM.
1. Da **Inizio AEM** menu vai a **Strumenti** > **Risorse** > **Modelli per frammenti di contenuto** > **Sito WKND**.
1. Apri **Avventura** Modello per frammento di contenuto

   ![Apri il modello di frammento di contenuto di avventura](assets/fragment-references/adventure-content-fragment-edit.png)

1. Sotto **Tipi di dati**, trascina e rilascia una **Riferimento frammento** nel pannello principale.

   ![Campo Aggiungi riferimento frammento](assets/fragment-references/add-fragment-reference-field.png)

1. Aggiorna **Proprietà** per questo campo:

   * Rendering come - `fragmentreference`
   * Etichetta campo - **Collaboratore di avventura**
   * Nome proprietà - `adventureContributor`
   * Tipo di modello - Seleziona il **Collaboratore** model
   * Percorso directory principale - `/content/dam/wknd`

   ![Proprietà dei riferimenti ai frammenti](assets/fragment-references/fragment-reference-properties.png)

   Nome della proprietà `adventureContributor` può ora essere utilizzato per fare riferimento a un frammento di contenuto da collaboratore.

1. Salva le modifiche apportate al modello.

## Assegnare un collaboratore a un’avventura

Ora che il modello Frammento di contenuto avventura è stato aggiornato, possiamo modificare un frammento esistente e fare riferimento a un collaboratore. È opportuno notare che la modifica del modello Frammento di contenuto *influenze* eventuali frammenti di contenuto esistenti creati da esso.

1. Passa a **Risorse** > **File** > **Sito WKND** > **Inglese** > **Avventure** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Cartella Bali Surf Camp](../quick-setup/assets/setup/bali-surf-camp-folder.png)

1. Fai clic su **Bali Surf Camp** frammento di contenuto per aprire l’Editor frammento di contenuto.
1. Aggiorna **Collaboratore di avventura** e seleziona un collaboratore facendo clic sull’icona della cartella.

   ![Seleziona Stacey Roswells come collaboratore](assets/fragment-references/stacey-roswell-contributor.png)

   *Selezionare un percorso per un frammento collaboratore*

   ![percorso popolato per il collaboratore](assets/fragment-references/populated-path.png)

   Tenere presente che solo i frammenti creati con la funzione **Collaboratore** è possibile selezionare il modello.

1. Salvare le modifiche apportate al frammento.

1. Ripeti i passaggi precedenti per assegnare un collaboratore a avventure come [Yosemite Backpack](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) e [Arrampicata su Rock del Colorado](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)

## Query di frammento di contenuto nidificato con GraphiQL

Quindi, esegui una query per un’avventura e aggiungi proprietà nidificate del modello Contributor a cui si fa riferimento. Useremo lo strumento GraphiQL per verificare rapidamente la sintassi della query.

1. Passa allo strumento GraphiQL in AEM: [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. Inserisci la seguente query:

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

   La query di cui sopra è per un singolo Avventura dal suo percorso. La `adventureContributor` fa riferimento al modello Collaboratore e possiamo quindi richiedere le proprietà dal frammento di contenuto nidificato.

1. Esegui la query e ottieni un risultato simile al seguente:

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

1. Sperimenta con altre query come `adventureList` e aggiungere proprietà per il frammento di contenuto a cui si fa riferimento in `adventureContributor`.

## Aggiornare l’app React per visualizzare il contenuto del collaboratore

Quindi, aggiorna le query utilizzate dall’applicazione React in modo da includere il nuovo Collaboratore e visualizza informazioni sul Collaboratore come parte della visualizzazione dei dettagli Avventura.

1. Apri l’app WKND GraphQL React nell’IDE.

1. Aprire il file `src/components/AdventureDetail.js`.

   ![IDE componente Dettagli avventura](assets/fragment-references/adventure-detail-ide.png)

1. Trova la funzione `adventureDetailQuery(_path)`. La `adventureDetailQuery(..)` funzione racchiude semplicemente una query GraphQL di filtro, che utilizza AEM `<modelName>ByPath` per eseguire query su un singolo frammento di contenuto identificato dal relativo percorso JCR.

1. Aggiorna la query per includere informazioni sul collaboratore a cui si fa riferimento:

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

   Con questo aggiornamento vengono aggiornate le proprietà aggiuntive relative al `adventureContributor`, `fullName`, `occupation`e `pictureReference` verranno inclusi nella query.

1. Inspect `Contributor` componente incorporato nel `AdventureDetail.js` file a `function Contributor(...)`. Questo componente esegue il rendering del nome, dell’occupazione e dell’immagine del collaboratore, se le proprietà esistono.

   La `Contributor` nel `AdventureDetail(...)` `return` metodo:

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

1. Salva le modifiche apportate al file.
1. Avvia React App, se non è già in esecuzione:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Passa a [http://localhost:3000](Http://localhost:3000/) e fai clic su un’avventura con un collaboratore a cui si fa riferimento. Ora puoi visualizzare le informazioni sui collaboratori elencate di seguito **Itinerario**:

   ![Collaboratore aggiunto nell’app](assets/fragment-references/contributor-added-detail.png)

## Congratulazioni!{#congratulations}

Congratulazioni! È stato aggiornato un modello di frammento di contenuto esistente per fare riferimento a un frammento di contenuto nidificato utilizzando il **Riferimento frammento** campo . Hai anche imparato a modificare una query GraphQL per includere campi da un modello di riferimento.

## Passaggi successivi {#next-steps}

Nel capitolo successivo, [Distribuzione di produzione tramite un ambiente di pubblicazione AEM](./production-deployment.md), scopri i servizi AEM Author e Publish e il modello di implementazione consigliato per le applicazioni headless. Sarà aggiornata un&#39;applicazione esistente per utilizzare le variabili di ambiente per modificare dinamicamente un endpoint GraphQL in base all&#39;ambiente di destinazione. Scoprirai anche come configurare AEM per la condivisione delle risorse tra origini (Cross-Origin resource sharing, CORS).
