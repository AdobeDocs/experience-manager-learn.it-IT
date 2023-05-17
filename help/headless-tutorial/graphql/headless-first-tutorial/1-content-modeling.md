---
title: Modellazione dei contenuti - Prima esercitazione AEM headless
description: Scopri come sfruttare i frammenti di contenuto, creare modelli di frammenti e utilizzare gli endpoint GraphQL in AEM.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---


# Modellazione dei contenuti

Ti diamo il benvenuto nel capitolo tutorial su Frammenti di contenuto e endpoint GraphQL in Adobe Experience Manager (AEM). Scopriremo come sfruttare i frammenti di contenuto, creare modelli di frammenti e utilizzare gli endpoint GraphQL in AEM.

I frammenti di contenuto offrono un approccio strutturato alla gestione dei contenuti tra canali, garantendo flessibilità e riutilizzabilità. L’abilitazione dei frammenti di contenuto in AEM consente la creazione di contenuti modulari, migliorando la coerenza e l’adattabilità.

In primo luogo, ti guideremo attraverso l’abilitazione di Frammenti di contenuto in AEM, coprendo le configurazioni e le impostazioni necessarie per un’integrazione diretta.

Nella sezione che segue viene illustrata la creazione di modelli di frammento, che definiscono struttura e attributi. Scopri come progettare modelli allineati ai requisiti di contenuto e gestirli in modo efficace.

Verrà quindi illustrata la creazione di frammenti di contenuto dai modelli, fornendo indicazioni dettagliate sull’authoring e la pubblicazione.

Inoltre, esploreremo la definizione AEM endpoint GraphQL. GraphQL recupera in modo efficiente i dati da AEM e configureremo e configureremo gli endpoint per esporre i dati desiderati. Le query persistenti ottimizzano le prestazioni e la memorizzazione in cache.

Nel corso dell&#39;esercitazione, forniremo spiegazioni, esempi di codice e suggerimenti pratici. Alla fine, avrai le competenze necessarie per abilitare Frammenti di contenuto, creare modelli di frammenti, generare frammenti e definire AEM endpoint GraphQL e query persistenti. Cominciamo!

## Configurazione in base al contesto

1. Passa a __Strumenti > Browser di configurazione__ per creare una configurazione per l’esperienza headless.

   ![Crea cartella](./assets/1/create-configuration.png)

   Fornisci un __title__ e __name__ e controlla __Query persistenti GraphQL__ e __Modelli per frammenti di contenuto__.


## Modelli per frammenti di contenuto

1. Passa a __Strumenti > Modelli per frammenti di contenuto__ e seleziona la cartella con il nome della configurazione creata nel passaggio 1.

   ![Cartella modello](./assets/1/model-folder.png)

1. All’interno della cartella, seleziona __Crea__ e il nome del modello __Teaser__. Aggiungi i seguenti tipi di dati al __Teaser__ modello.

   | Tipo di dati | Nome | Obbligatorio | Opzioni |
   |----------|------|----------|---------|
   | Riferimento contenuto | Risorsa | sì | Aggiungi un&#39;immagine predefinita, se lo desideri. Ex: /content/dam/wknd-headless/assets/AdobeStock_307513975.mp4 |
   | Testo su riga singola | Titolo | sì |
   | Testo su riga singola | Pre-titolo | no |
   | Testo su più righe | Descrizione | no | Verificare che il tipo predefinito sia RTF |
   | Enumerazione | Stile | sì | Esegui il rendering come menu a discesa. Le opzioni sono Eroe -> eroe e in evidenza -> in primo piano |

   ![Modello teaser](./assets/1/teaser-model.png)

1. All’interno della cartella, crea un secondo modello denominato __Offerta__. Fai clic su crea e assegna al modello il nome &quot;Offerta&quot; e aggiungi i seguenti tipi di dati:

   | Tipo di dati | Nome | Obbligatorio | Opzioni |
   |----------|------|----------|---------|
   | Riferimento contenuto | Risorsa | sì | Aggiungi l&#39;immagine predefinita. Esempio: `/content/dam/wknd-headless/assets/AdobeStock_238607111.jpeg` |
   | Testo su più righe | Descrizione | no |  |
   | Testo su più righe | Articolo | no |  |

   ![Modello di offerta](./assets/1/offer-model.png)

1. All’interno della cartella, crea un terzo modello denominato __Elenco immagini__. Fai clic su crea e assegna al modello il nome &quot;Elenco immagini&quot; e aggiungi i seguenti tipi di dati:

   | Tipo di dati | Nome | Obbligatorio | Opzioni |
   |----------|------|----------|---------|
   | Riferimento frammento | Elementi di elenco | sì | Esegui il rendering come campo multiplo. Il modello di frammento di contenuto consentito è Offerta. |

   ![Modello a elenco immagini](./assets/1/imagelist-model.png)

## Frammenti di contenuto

1. Ora passa a Risorse e crea una cartella per il nuovo sito. Fai clic su crea e denomina la cartella.

   ![Aggiungi cartella](./assets/1/create-folder.png)

1. Dopo aver creato la cartella, selezionala e aprila __Proprietà__.
1. Nella cartella __Configurazioni cloud__ seleziona la configurazione [creato in precedenza](#enable-content-fragments-and-graphql).

   ![Cartella risorse AEM configurazione cloud headless](./assets/1/cloud-config.png)

   Fai clic nella nuova cartella e crea un teaser. Fai clic su __Crea__ e __Frammento di contenuto__ e seleziona la __Teaser__ modello. Denomina il modello __Eroe__ e fai clic su __Crea__.

   | Nome | Note |
   |----------|------|
   | Risorsa | Lascia come valore predefinito o scegli una risorsa diversa (video o immagine) |
   | Titolo | `Explore. Discover. Live.` |
   | Pre-titolo | `Join use for your next adventure.` |
   | Descrizione | Lascia vuoto |
   | Stile | `Hero` |

   ![frammento eroe](./assets/1/teaser-model.png)

## Endpoint GraphQL

1. Passa a __Strumenti > GraphQL__

   ![AEM GraphiQL](./assets/1/endpoint-nav.png)

1. Fai clic su __Crea__ assegna un nome al nuovo endpoint e scegli la configurazione appena creata.

   ![AEM endpoint GraphQL headless](./assets/1/endpoint.png)

## Query persistenti GraphQL

1. Proviamo il nuovo endpoint. Passa a __Strumenti > Editor query GraphQL__ e scegli il nostro punto finale per il menu a discesa in alto a destra della finestra.

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

   Dovrebbe essere visualizzato un elenco contenente il singolo frammento creato [sopra](#create-content).

   Per questo esercizio, crea una query completa utilizzata dall’app AEM headless. Crea una query che restituisce un singolo teaser in base al percorso. Nell’editor delle query, immetti la seguente query:

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

   In __variabili di query__ in basso, immettere:

   ```json
   {
       "path": "/content/dam/pure-headless/hero"
   }
   ```

   >[!NOTE]
   >
   > Potrebbe essere necessario regolare la variabile di query `path` ha basato i nomi della cartella e del frammento.


   Esegui la query per ricevere i risultati del frammento di contenuto creato in precedenza.

1. Fai clic su __Salva__  per mantenere (salvare) la query e assegnare un nome alla query __teaser__. Questo ci consente di fare riferimento alla query per nome nell’applicazione.

## Passaggi successivi

Congratulazioni. È stata configurata AEM as a Cloud Service per consentire la creazione di frammenti di contenuto e endpoint GraphQL. Hai anche creato un modello per frammenti di contenuto e un frammento di contenuto, hai definito un endpoint GraphQL e hai mantenuto la query. È ora possibile passare al capitolo successivo dell’esercitazione, in cui verrà illustrato come creare un’applicazione AEM Headless React che utilizzi i frammenti di contenuto e l’endpoint GraphQL creati in questo capitolo.

[Capitolo successivo: AEM API headless e React](./2-aem-headless-apis-and-react.md)