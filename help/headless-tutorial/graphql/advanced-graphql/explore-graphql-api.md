---
title: Esplora l’API GraphQL di AEM - Concetti avanzati di AEM Headless - GraphQL
description: Invia query GraphQL tramite IDE GraphiQL. Scopri le query avanzate utilizzando filtri, variabili e direttive. Eseguire una query per i riferimenti a frammenti e contenuti, inclusi i riferimenti da campi di testo su più righe.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
duration: 438
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# Esplorare l’API GraphQL di AEM

L’API GraphQL in AEM consente di esporre i dati dei frammenti di contenuto alle applicazioni a valle. Nell&#39;esercitazione di base [esercitazione di GraphQL in più passaggi](../multi-step/explore-graphql-api.md), hai utilizzato GraphiQL Explorer per testare e perfezionare le query GraphQL.

In questo capitolo, puoi utilizzare GraphiQL Explorer per definire query più avanzate per raccogliere i dati dei frammenti di contenuto creati nel [capitolo precedente](../advanced-graphql/author-content-fragments.md).

## Prerequisiti {#prerequisites}

Questo documento fa parte di un&#39;esercitazione in più parti. Prima di procedere con questo capitolo, assicurati che i capitoli precedenti siano stati completati.

## Obiettivi {#objectives}

In questo capitolo imparerai a:

* Filtrare un elenco di frammenti di contenuto con riferimenti utilizzando variabili di query
* Filtrare il contenuto all’interno di un riferimento a un frammento
* Query per contenuto in linea e riferimenti a frammenti da un campo di testo su più righe
* Eseguire query tramite direttive
* Query per il tipo di contenuto Oggetto JSON

## Utilizzo di GraphiQL Explorer


Lo strumento [GraphiQL Explorer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=it) consente agli sviluppatori di creare e testare query sul contenuto nell&#39;ambiente AEM corrente. Lo strumento GraphiQL consente inoltre agli utenti di **salvare o mantenere** le query da utilizzare dalle applicazioni client in un&#39;impostazione di produzione.

Quindi, esplora la potenza dell’API GraphQL di AEM utilizzando GraphiQL Explorer incorporato.

1. Dalla schermata iniziale di AEM, passa a **Strumenti** > **Generale** > **Editor query GraphQL**.

   ![Passa a IDE GraphiQL](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>In, alcune versioni di AEM (6.X.X) con lo strumento GraphiQL Explorer (o IDE GraphiQL) devono essere installate manualmente, segui [le istruzioni qui](../how-to/install-graphiql-aem-6-5.md).

1. Nell&#39;angolo in alto a destra, accertarsi che l&#39;endpoint sia impostato su **Endpoint condiviso WKND**. Se si modifica il valore dell&#39;elenco a discesa _Endpoint_, nell&#39;angolo in alto a sinistra verranno visualizzate le _query persistenti_ esistenti.

   ![Imposta endpoint GraphQL](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

Tutte le query verranno estese ai modelli creati nel progetto **WKND Shared**.


## Filtrare un elenco di frammenti di contenuto utilizzando le variabili di query

Nell&#39;[esercitazione di GraphQL in più passaggi](../multi-step/explore-graphql-api.md) precedente hai definito e utilizzato query persistenti di base per ottenere i dati dei frammenti di contenuto. In questo caso, espandi questa conoscenza e filtra i dati dei frammenti di contenuto passando le variabili alle query persistenti.

Durante lo sviluppo di applicazioni client, in genere è necessario filtrare i frammenti di contenuto in base a argomenti dinamici. L’API GraphQL di AEM consente di trasmettere questi argomenti come variabili in una query per evitare la costruzione di stringhe sul lato client in fase di esecuzione. Per ulteriori informazioni sulle variabili di GraphQL, consulta la [documentazione di GraphQL](https://graphql.org/learn/queries/#variables).

Per questo esempio, esegui una query su tutti gli Istruttori con una particolare abilità.

1. Nell’IDE GraphiQL, incolla la seguente query nel pannello a sinistra:

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
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
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   La query `listPersonBySkill` di cui sopra accetta una variabile (`skillFilter`) che è un `String` obbligatorio. Questa query esegue una ricerca in tutti i frammenti di contenuto della persona e li filtra in base al campo `skills` e alla stringa passata in `skillFilter`.

   `listPersonBySkill` include la proprietà `contactInfo`, che è un riferimento frammento al modello Informazioni di contatto definito nei capitoli precedenti. Il modello Informazioni contatto contiene `phone` e `email` campi. Almeno uno di questi campi nella query deve essere presente per funzionare correttamente.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. Quindi, definiamo `skillFilter` e ottieni tutti gli istruttori esperti nello sci. Incolla la seguente stringa JSON nel pannello Variabili di query nell’IDE GraphiQL:

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. Esegui la query. Il risultato dovrebbe essere simile al seguente:

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

Premi il pulsante **Riproduci** nel menu principale per eseguire la query. Dovresti visualizzare i risultati dei frammenti di contenuto del capitolo precedente:

![Risultati persona per abilità](assets/explore-graphql-api/person-by-skill.png)

## Filtrare il contenuto all’interno di un riferimento a un frammento

L’API GraphQL di AEM consente di eseguire query sui frammenti di contenuto nidificati. Nel capitolo precedente sono stati aggiunti tre nuovi riferimenti di frammento a un frammento di contenuto Adventure: `location`, `instructorTeam` e `administrator`. Ora, filtriamo tutte le avventure per qualsiasi amministratore che ha un nome specifico.

>[!CAUTION]
>
>È necessario consentire un solo modello come riferimento per eseguire correttamente questa query.

1. Nell’IDE GraphiQL, incolla la seguente query nel pannello a sinistra:

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         title
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. Quindi, incolla la seguente stringa JSON nel pannello Variabili di query:

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   La query `getAdventureAdministratorDetailsByAdministratorName` filtra tutte le avventure per qualsiasi `administrator` di `fullName` &quot;Jacob Wester&quot;, restituendo informazioni da due frammenti di contenuto nidificati: Avventura e Istruttore.

1. Esegui la query. Il risultato dovrebbe essere simile al seguente:

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "title": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## Eseguire una query per i riferimenti in linea da un campo di testo su più righe {#query-rte-reference}

L’API GraphQL di AEM consente di eseguire query per contenuti e riferimenti a frammenti all’interno di campi di testo su più righe. Nel capitolo precedente, hai aggiunto entrambi i tipi di riferimento nel campo **Descrizione** del frammento di contenuto del team di Yosemite. Ora, recuperiamo questi riferimenti.

1. Nell’IDE GraphiQL, incolla la seguente query nel pannello a sinistra:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   La query `getTeamByAdventurePath` filtra tutte le avventure in base al percorso e restituisce i dati per il riferimento al frammento `instructorTeam` di una specifica avventura.

   `_references` è un campo generato dal sistema e utilizzato per visualizzare riferimenti, inclusi quelli inseriti in campi di testo su più righe.

   La query `getTeamByAdventurePath` recupera più riferimenti. In primo luogo, utilizza l&#39;oggetto predefinito `ImageRef` per recuperare `_path` e `__typename` di immagini inserite come riferimenti al contenuto nel campo di testo su più righe. Quindi, utilizza `LocationModel` per recuperare i dati del frammento di contenuto della posizione inserito nello stesso campo.

   La query include anche il campo `_metadata`. Questo consente di recuperare il nome del frammento di contenuto del team e di visualizzarlo successivamente nell’app WKND.

1. Quindi, incolla la seguente stringa JSON nel pannello Variabili di query per ottenere l’avventura di backpacking di Yosemite:

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Esegui la query. Il risultato dovrebbe essere simile al seguente:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   Il campo `_references` mostra sia l&#39;immagine del logo che il frammento di contenuto della Loggia di Yosemite Valley inserito nel campo **Descrizione**.


## Eseguire query tramite direttive

A volte, durante lo sviluppo di applicazioni client, è necessario modificare la struttura delle query in modo condizionale. In questo caso, l’API GraphQL di AEM consente di utilizzare le direttive GraphQL per modificare il comportamento delle query in base ai criteri forniti. Per ulteriori informazioni sulle direttive di GraphQL, consulta la [documentazione di GraphQL](https://graphql.org/learn/queries/#directives).

Nella [sezione precedente](#query-rte-reference), hai imparato a eseguire query per riferimenti in linea all&#39;interno di campi di testo su più righe. Il contenuto è stato recuperato dal campo `description` nel formato `plaintext`. Quindi, espandiamo la query e utilizziamo una direttiva per recuperare `description` in modo condizionale anche nel formato `json`.

1. Nell’IDE GraphiQL, incolla la seguente query nel pannello a sinistra:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   La query precedente accetta un&#39;altra variabile (`includeJson`) che è un `Boolean` obbligatorio, noto anche come direttiva della query. È possibile utilizzare una direttiva per includere in modo condizionale i dati del campo `description` nel formato `json` in base al valore booleano passato in `includeJson`.

1. Quindi, incolla la seguente stringa JSON nel pannello Variabili di query:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Esegui la query. Si dovrebbe ottenere lo stesso risultato ottenuto nella sezione precedente su [come eseguire una query per i riferimenti in linea all&#39;interno di campi di testo con più righe](#query-rte-reference).

1. Aggiornare la direttiva `includeJson` in `true` ed eseguire di nuovo la query. Il risultato dovrebbe essere simile al seguente:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Query per il tipo di contenuto Oggetto JSON

Ricorda che nel capitolo precedente sull’authoring dei frammenti di contenuto, hai aggiunto un oggetto JSON nel campo **Meteo per stagione**. Ora recuperiamo tali dati all’interno del frammento di contenuto Posizione.

1. Nell’IDE GraphiQL, incolla la seguente query nel pannello a sinistra:

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
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
     }
   }
   ```

1. Quindi, incolla la seguente stringa JSON nel pannello Variabili di query:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Esegui la query. Il risultato dovrebbe essere simile al seguente:

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   Il campo `weatherBySeason` contiene l&#39;oggetto JSON aggiunto nel capitolo precedente.

## Esegui query per tutto il contenuto contemporaneamente

Finora sono state eseguite più query per illustrare le funzionalità dell’API GraphQL di AEM.

Gli stessi dati possono essere recuperati con una sola query, che in seguito viene utilizzata nell’applicazione client per recuperare informazioni aggiuntive come posizione, nome del team e membri del team di un’avventura:

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


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## Congratulazioni.

Congratulazioni Sono state testate query avanzate per raccogliere i dati dei frammenti di contenuto creati nel capitolo precedente.

## Passaggi successivi

Nel [prossimo capitolo](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), scoprirai come rendere persistenti le query GraphQL e perché è consigliabile utilizzare le query persistenti nelle applicazioni.
