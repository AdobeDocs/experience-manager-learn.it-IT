---
title: 'Query GraphQL persistenti: concetti avanzati di AEM Headless - GraphQL'
description: In questo capitolo di Concetti avanzati di Adobe Experience Manager (AEM) Headless, scopri come creare e aggiornare query GraphQL persistenti con parametri. Scopri come trasmettere i parametri di controllo cache nelle query persistenti.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
duration: 183
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---

# Query GraphQL persistenti

Le query persistenti sono query memorizzate sul server Adobe Experience Manager (AEM). I client possono inviare una richiesta HTTP GET con il nome della query per eseguirla. Il vantaggio di questo approccio è la possibilità di memorizzazione in cache. Mentre le query GraphQL lato client possono essere eseguite anche utilizzando richieste HTTP POST, che non possono essere memorizzate nella cache, le query persistenti possono essere memorizzate nella cache da cache HTTP o da una rete CDN, migliorando le prestazioni. Le query persistenti ti consentono di semplificare le richieste e migliorare la sicurezza, in quanto sono incapsulate nel server e l’amministratore di AEM ne ha il pieno controllo. È **buona prassi ed è vivamente consigliato** utilizzare le query persistenti quando si lavora con l&#39;API GraphQL di AEM.

Nel capitolo precedente, hai esplorato alcune query GraphQL avanzate per raccogliere dati per l’app WKND. In questo capitolo, rendi le query persistenti in AEM e scopri come utilizzare il controllo della cache sulle query persistenti.

## Prerequisiti {#prerequisites}

Questo documento fa parte di un&#39;esercitazione in più parti. Prima di procedere con questo capitolo, assicurati che il [capitolo precedente](explore-graphql-api.md) sia stato completato.

## Obiettivi {#objectives}

In questo capitolo, scopri come:

* Mantenere le query GraphQL con i parametri
* Utilizzare parametri di controllo cache con query persistenti

## Rivedi l&#39;impostazione di configurazione _Query GraphQL persistenti_

Esaminiamo che _le query persistenti di GraphQL_ sono abilitate per il progetto del sito WKND nell&#39;istanza di AEM.

1. Passa a **Strumenti** > **Generale** > **Browser configurazioni**.

1. Seleziona **Condiviso WKND**, quindi seleziona **Proprietà** nella barra di navigazione superiore per aprire le proprietà di configurazione. Nella pagina Proprietà di configurazione, dovresti notare che l&#39;autorizzazione **Query persistenti GraphQL** è abilitata.

   ![Proprietà configurazione](assets/graphql-persisted-queries/configuration-properties.png)

## Mantenere le query GraphQL utilizzando lo strumento GraphiQL Explorer incorporato

In questa sezione, rendiamo persistente la query GraphQL che viene successivamente utilizzata nell’applicazione client per recuperare ed eseguire il rendering dei dati dei frammenti di contenuto Adventure.

1. Immetti la seguente query in GraphiQL Explorer:

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   Verifica che la query funzioni prima di salvarla.

1. Tocca quindi Salva con nome e immetti `adventure-details-by-slug` come nome della query.

   ![Mantieni query GraphQL](assets/graphql-persisted-queries/persist-graphql-query.png)

## Esecuzione di query persistenti con variabili tramite codifica di caratteri speciali

Comprendiamo in che modo le query persistenti con variabili vengono eseguite dall’applicazione lato client codificando i caratteri speciali.

Per eseguire una query persistente, l’applicazione client effettua una richiesta GET utilizzando la seguente sintassi:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

Per eseguire una query persistente _con una variabile_, la sintassi precedente cambia in:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

I caratteri speciali come punto e virgola (;), segno di uguale (=), barre (/) e spazio devono essere convertiti per utilizzare la codifica UTF-8 corrispondente.

Eseguendo la query `getAllAdventureDetailsBySlug` dal terminale della riga di comando, questi concetti vengono esaminati in azione.

1. Apri GraphiQL Explorer e fai clic sui **puntini di sospensione** (...) accanto alla query persistente `getAllAdventureDetailsBySlug`, quindi fai clic su **Copia URL**. Incolla l’URL copiato in un riquadro di testo, come illustrato di seguito:

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. Aggiungi `yosemite-backpacking` come valore della variabile

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. Codificare i punti e virgola (;) e il segno di uguale (=) caratteri speciali

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. Apri un terminale della riga di comando e utilizzando [Curl](https://curl.se/) esegui la query

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    Se esegui la query di cui sopra nell’ambiente AEM Author, devi inviare le credenziali. Consulta [Token di accesso per lo sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html?lang=it) per una dimostrazione e [Chiamata dell&#39;API AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=it#calling-the-aem-api) per i dettagli della documentazione.

Rivedi inoltre [Come eseguire una query persistente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=it#execute-persisted-query), [Utilizzando le variabili di query](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=it#query-variables) e [Codificare l&#39;URL della query per l&#39;utilizzo da parte di un&#39;app](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=it#encoding-query-url) per apprendere l&#39;esecuzione di query persistenti da parte delle applicazioni client.

## Aggiornamento dei parametri di controllo cache nelle query persistenti {#cache-control-all-adventures}

L’API GraphQL di AEM ti consente di aggiornare i parametri predefiniti di controllo cache alle query per migliorare le prestazioni. I valori predefiniti per il controllo della cache sono:

* 60 secondi è il valore TTL predefinito (maxage=60) per il client (ad esempio, un browser)

* 7200 secondi, TTL predefinito (s-maxage=7200) per Dispatcher e CDN; noto anche come cache condivise

Utilizzare la query `adventures-all` per aggiornare i parametri di controllo cache. La risposta alla query è grande ed è utile controllarne `age` nella cache. Questa query persistente viene utilizzata in seguito per aggiornare [l&#39;applicazione client](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. Apri GraphiQL Explorer e fai clic sui **puntini di sospensione** (...) accanto alla query persistente, quindi fai clic su **Intestazioni** per aprire il modale **Configurazione cache**.

   ![Opzione Intestazione GraphQL Persistente](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. Nel modale **Configurazione cache**, aggiorna il valore dell&#39;intestazione `max-age` a `600 `secondi (10 minuti), quindi fai clic su **Salva**

   ![Mantieni configurazione cache di GraphQL](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


Rivedi [Memorizzazione in cache delle query persistenti](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=it#caching-persisted-queries) per ulteriori informazioni sui parametri predefiniti di controllo cache.


## Congratulazioni.

Congratulazioni Ora hai imparato a rendere persistenti le query GraphQL con parametri, aggiornare le query persistenti e utilizzare parametri di controllo cache con le query persistenti.

## Passaggi successivi

Nel [prossimo capitolo](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md), implementerai le richieste di query persistenti nell&#39;app WKND.
