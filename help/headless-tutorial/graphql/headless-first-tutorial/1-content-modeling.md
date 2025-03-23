---
title: 'Modellazione dei contenuti: prima esercitazione AEM Headless'
description: Scopri come sfruttare i frammenti di contenuto, creare modelli di frammenti e utilizzare gli endpoint di GraphQL in AEM.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 6e5e3cb4-9a47-42af-86af-da33fd80cb47
duration: 175
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---

# Modellazione dei contenuti

Benvenuti nel capitolo del tutorial su Frammenti di contenuto ed endpoint GraphQL in Adobe Experience Manager (AEM). Scopriremo come sfruttare i frammenti di contenuto, creare modelli di frammenti e utilizzare gli endpoint di GraphQL in AEM.

I frammenti di contenuto offrono un approccio strutturato alla gestione dei contenuti attraverso i canali, garantendo flessibilità e riutilizzabilità. L’abilitazione di Frammenti di contenuto in AEM consente la creazione di contenuti modulari, migliorando la coerenza e l’adattabilità.

Innanzitutto, ti guideremo attraverso l’abilitazione dei frammenti di contenuto in AEM, coprendo le configurazioni e le impostazioni necessarie per un’integrazione fluida.

Vengono quindi descritti la creazione di modelli per frammenti, che definiscono struttura e attributi. Scopri come progettare modelli allineati ai requisiti di contenuto e gestirli in modo efficace.

Quindi, verrà fornita una dimostrazione della creazione di frammenti di contenuto dai modelli, con istruzioni dettagliate sull’authoring e la pubblicazione.

Inoltre, esploreremo la definizione degli endpoint di AEM GraphQL. GraphQL recupera i dati da AEM in modo efficiente e configurerà gli endpoint per esporre i dati desiderati. Le query persistenti ottimizzano le prestazioni e la memorizzazione in cache.

Nel corso dell’esercitazione forniremo spiegazioni, esempi di codice e suggerimenti pratici. Entro la fine, avrai acquisito le competenze necessarie per abilitare Frammenti di contenuto, creare Modelli di frammenti, generare Frammenti e definire endpoint AEM GraphQL e query persistenti. Iniziamo!

## Configurazione in base al contesto

1. Passa a __Strumenti > Browser configurazioni__ per creare una configurazione per l&#39;esperienza headless.

   ![Crea cartella](./assets/1/create-configuration.png)

   Specifica un __titolo__ e un __nome__, quindi seleziona __Query GraphQL persistenti__ e __Modelli per frammenti di contenuto__.


## Modelli per frammenti di contenuto

1. Passa a __Strumenti > Modelli per frammenti di contenuto__ e seleziona la cartella con il nome della configurazione creata al passaggio 1.

   ![Cartella modelli](./assets/1/model-folder.png)

1. Nella cartella, seleziona __Crea__ e denomina il modello __Teaser__. Aggiungi i seguenti tipi di dati al modello __Teaser__.

   | Tipo di dati | Nome | Obbligatorio | Opzioni |
   |----------|------|----------|---------|
   | Riferimento contenuto | Risorsa | sì | Se lo desideri, aggiungi un’immagine predefinita. Esempio: /content/dam/wknd-headless/assets/AdobeStock_307513975.mp4 |
   | Testo su riga singola | Titolo | sì |
   | Testo su riga singola | Pre-titolo | no |
   | Testo su più righe | Descrizione | no | Assicurati che il tipo predefinito sia RTF |
   | Enumerazione | Stile | sì | Menu a discesa Rendering come. Le opzioni sono Eroe -> protagonista e In primo piano -> in primo piano |

   ![Modello teaser](./assets/1/teaser-model.png)

1. Nella cartella, crea un secondo modello denominato __Offerta__. Fai clic su Crea e assegna al modello il nome &quot;Offerta&quot; e aggiungi i seguenti tipi di dati:

   | Tipo di dati | Nome | Obbligatorio | Opzioni |
   |----------|------|----------|---------|
   | Riferimento contenuto | Risorsa | sì | Aggiungi immagine predefinita. Esempio: `/content/dam/wknd-headless/assets/AdobeStock_238607111.jpeg` |
   | Testo su più righe | Descrizione | no |  |
   | Testo su più righe | Articolo | no |  |

   ![Modello di offerta](./assets/1/offer-model.png)

1. All&#39;interno della cartella, crea un terzo modello denominato __Elenco immagini__. Fai clic su Crea e assegna al modello il nome &quot;Elenco immagini&quot; e aggiungi i seguenti tipi di dati:

   | Tipo di dati | Nome | Obbligatorio | Opzioni |
   |----------|------|----------|---------|
   | Riferimento frammento | Elementi di elenco | sì | Esegui rendering come campo multiplo. Il modello per frammenti di contenuto consentito è Offerta. |

   ![Modello elenco immagini](./assets/1/imagelist-model.png)

## Frammenti di contenuto

1. Passa ora ad Assets e crea una cartella per il nuovo sito. Fai clic su Crea e assegna un nome alla cartella.

   ![Aggiungi cartella](./assets/1/create-folder.png)

1. Dopo aver creato la cartella, selezionala e apri le relative __proprietà__.
1. Nella scheda __Configurazioni cloud__ della cartella, seleziona la configurazione [creata in precedenza](#enable-content-fragments-and-graphql).

   ![Configurazione cloud AEM headless per cartella risorse](./assets/1/cloud-config.png)

   Fai clic su nella nuova cartella e crea un teaser. Fai clic su __Crea__ e __Frammento di contenuto__ e seleziona il modello __Teaser__. Denomina il modello __Hero__ e fai clic su __Crea__.

   | Nome | Note |
   |----------|------|
   | Risorsa | Lascia come valore predefinito o scegli un’altra risorsa (video o immagine) |
   | Titolo | `Explore. Discover. Live.` |
   | Pre-titolo | `Join use for your next adventure.` |
   | Descrizione | Lascia vuoto |
   | Stile | `Hero` |

   ![frammento hero](./assets/1/teaser-model.png)

## Endpoint GraphQL

1. Passa a __Strumenti > GraphQL__

   ![GraphiQL AEM](./assets/1/endpoint-nav.png)

1. Fai clic su __Crea__, assegna un nome al nuovo endpoint e scegli la configurazione appena creata.

   ![Endpoint AEM Headless GraphQL](./assets/1/endpoint.png)

## Query persistenti GraphQL

1. Testiamo il nuovo endpoint. Passa a __Strumenti > GraphQL Query Editor__ e scegli il nostro endpoint per il menu a discesa in alto a destra nella finestra.

1. Nell’editor delle query, crea alcune query diverse.


   ```graphql
   {
       teaserList {
           items {
           title
           }
       }
   }
   ```

   Dovresti ottenere un elenco contenente il singolo frammento creato [sopra](#create-content).

   Per questo esercizio, crea una query completa utilizzata dall’app headless di AEM. Crea una query che restituisce un singolo teaser per percorso. Nell’editor delle query, immetti la seguente query:

   ```graphql
   query TeaserByPath($path: String!) {
   component: teaserByPath(_path: $path) {
       item {
       __typename
       _path
       _metadata {
           stringMetadata {
           name
           value
           }
       }
       title
       preTitle
       style
       asset {
           ... on MultimediaRef {
           __typename
           _authorUrl
           _publishUrl
           format
           }
           ... on ImageRef {
           __typename
           _authorUrl
           _publishUrl
           mimeType
           width
           height
           }
       }
       description {
           html
           plaintext
       }
       }
   }
   }
   ```

   Nell&#39;input __variabili query__ in basso, immettere:

   ```json
   {
       "path": "/content/dam/pure-headless/hero"
   }
   ```

   >[!NOTE]
   >
   > Potrebbe essere necessario modificare la variabile di query `path` in base ai nomi della cartella e del frammento.


   Esegui la query per ricevere i risultati del frammento di contenuto creato in precedenza.

1. Fai clic su __Salva__ per salvare la query in modo permanente e denominarla __teaser__. Questo consente di fare riferimento alla query per nome nell’applicazione.

## Passaggi successivi

Congratulazioni Hai configurato correttamente AEM as a Cloud Service per consentire la creazione di frammenti di contenuto ed endpoint GraphQL. Hai anche creato un modello per frammenti di contenuto e un frammento di contenuto, definito un endpoint GraphQL e una query persistente. Ora puoi passare al prossimo capitolo dell’esercitazione, dove scoprirai come creare un’applicazione AEM Headless React che utilizza i frammenti di contenuto e l’endpoint GraphQL creati in questo capitolo.

[Prossimo capitolo: API headless di AEM e React](./2-aem-headless-apis-and-react.md)
