---
title: Esplora le API GraphQL - Guida introduttiva a AEM senza titolo - GraphQL
description: Guida introduttiva ad Adobe Experience Manager (AEM) e GraphQL. Esplora AEM API GraphQL utilizzando l'IDE GrapiQL integrato. Scoprite come AEM generare automaticamente uno schema GraphQL basato su un modello di frammento di contenuto. Sperimentate la creazione di query di base utilizzando la sintassi GraphQL.
sub-product: assets
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
translation-type: tm+mt
source-git-commit: bfcc9dbb70753f985a2e47f329dbb9f43f5805e2
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 0%

---


# Esplora le API GraphQL {#explore-graphql-apis}

>[!CAUTION]
>
> L&#39;API AEM GraphQL per la distribuzione dei frammenti di contenuto è disponibile su richiesta.
> Per abilitare l&#39;API per il AEM come programma di Cloud Service, contattate  supporto di Adobe.

L&#39;API GraphQL di AEM fornisce un potente linguaggio di query per esporre i dati dei frammenti di contenuto alle applicazioni a valle. I modelli di frammenti di contenuto definiscono lo schema di dati utilizzato dai frammenti di contenuto. Ogni volta che viene creato o aggiornato un modello di frammento di contenuto, lo schema viene tradotto e aggiunto al &quot;grafico&quot; che costituisce l&#39;API GraphQL.

In questo capitolo, esploreremo alcune comuni query GraphQL per raccogliere i contenuti. Integrato in AEM è un IDE denominato [GraphiQL](https://github.com/graphql/graphiql). L&#39;IDE GraphiQL consente di testare e perfezionare rapidamente le query e i dati restituiti. GraphiQL consente inoltre di accedere facilmente alla documentazione, facilitando l&#39;apprendimento e la comprensione dei metodi disponibili.

## Prerequisiti {#prerequisites}

Si tratta di un&#39;esercitazione con più parti e si presume che i passaggi descritti in [Authoring Content Fragments](./author-content-fragments.md) siano stati completati.

## Obiettivi {#objectives}

* Scopri come utilizzare lo strumento GraphiQL per creare una query utilizzando la sintassi GraphQL.
* Scoprite come eseguire una query su un elenco di frammenti di contenuto e un singolo frammento di contenuto.
* Scopri come filtrare e richiedere attributi dati specifici.
* Scoprite come eseguire una query su una variante di un frammento di contenuto.
* Scopri come partecipare a una query di più modelli di frammenti di contenuto

## Eseguire una query su un elenco di frammenti di contenuto {#query-list-cf}

Un requisito comune consiste nel richiedere più frammenti di contenuto.

1. Andate all&#39;IDE GraphiQL in [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html).
1. Incollate la seguente query nel pannello a sinistra (sotto l’elenco dei commenti):

   ```graphql
   {
     contributorList {
       items {
           _path
         }
     }
   }
   ```

1. Premere il tasto **Play** nel menu superiore per eseguire la query. Dovresti vedere i risultati dei frammenti di contenuto dei collaboratori del capitolo precedente:

   ![Risultati elenco collaboratori](assets/explore-graphql-api/contributorlist-results.png)

1. Posizionare il cursore sotto il testo `_path` e inserire **CTRL+Spazio** per attivare i suggerimenti sul codice. Aggiungete `fullName` e `occupation` alla query.

   ![Aggiorna query con hash del codice](assets/explore-graphql-api/update-query-codehinting.png)

1. Eseguire nuovamente la query premendo il pulsante **Riproduci**. I risultati devono includere le proprietà aggiuntive di `fullName` e `occupation`.

   ![Nome completo e risultati dell&#39;occupazione](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` e  `occupation` sono proprietà semplici. Ricordate dal capitolo [Defining Content Fragment Models](./content-fragment-models.md) che `fullName` e `occupation` sono i valori utilizzati per definire la **Property Name** dei rispettivi campi.

1. `pictureReference` e  `biographyText` rappresentano campi più complessi. Aggiornate la query con quanto segue per restituire i dati relativi ai campi `pictureReference` e `biographyText`.

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biographyText {
           html
         }
         pictureReference {
           ... on ImageRef {
               _path
               width
               height
               }
           }
       }
     }
   }
   ```

   `biographyText` è un campo di testo a più righe e l&#39;API GraphQL ci consente di scegliere diversi formati per i risultati come  `html`,  `markdown`o  `json`   `plaintext`.

   `pictureReference` è un riferimento di contenuto e si prevede che sia un&#39;immagine, pertanto viene utilizzato  `ImageRef` l&#39;oggetto incorporato. Questo consente di richiedere dati aggiuntivi sul riferimento dell&#39;immagine, come i `width` e `height`.

1. Quindi, provate a eseguire query per ottenere un elenco di **Avventure**. Esegui la seguente query:

   ```graphql
   {
     adventureList {
       items {
         adventureTitle
         adventureType
         adventurePrimaryImage {
           ...on ImageRef {
             _path
             mimeType
           }
         }
       }
     }
   }
   ```

   Dovrebbe essere visualizzato un elenco di **Avventure** restituite. È possibile sperimentare aggiungendo altri campi alla query.

## Filtrare un elenco di frammenti di contenuto {#filter-list-cf}

Quindi, vediamo come è possibile filtrare i risultati in un sottoinsieme di frammenti di contenuto in base a un valore di proprietà.

1. Immettete la seguente query nell’interfaccia utente di GraphiQL:

   ```graphql
   {
   contributorList(filter: {
     occupation: {
       _expressions: {
         value: "Photographer"
         }
       }
     }) {
       items {
         _path
         fullName
         occupation
       }
     }
   }
   ```

   La query di cui sopra esegue una ricerca contro tutti i collaboratori del sistema. Il filtro aggiunto all&#39;inizio della query eseguirà un confronto tra il campo `occupation` e la stringa &quot;**Fotografo**&quot;.

1. Eseguire la query. È previsto che venga restituito un solo **Collaboratore**.
1. Digitate la seguente query per eseguire una query su un elenco di **Avventure** in cui `adventureActivity` è **not** uguale a **&quot;Surfing&quot;**:

   ```graphql
   {
     adventureList(filter: {
       adventureActivity: {
           _expressions: {
               _operator: EQUALS_NOT
               value: "Surfing"
           }
       }
   }) {
       items {
       _path
       adventureTitle
       adventureActivity
       }
     }
   }
   ```

1. Eseguire la query ed esaminare i risultati. Osservate che nessuno dei risultati include un `adventureType` uguale a **&quot;Surfing&quot;**.

Ci sono molte altre opzioni per filtrare e creare query complesse, qui sopra sono solo alcuni esempi.

## Eseguire una query su un singolo frammento di contenuto {#query-single-cf}

È inoltre possibile eseguire direttamente una query su un singolo frammento di contenuto. Il contenuto in AEM è memorizzato in modo gerarchico e l&#39;identificatore univoco di un frammento è basato sul percorso del frammento. Se l&#39;obiettivo è la restituzione di dati su un singolo frammento, è preferibile utilizzare il percorso e eseguire direttamente una query sul modello. Questa sintassi indica che la complessità della query sarà molto bassa e genererà un risultato più rapido.

1. Immettere la seguente query nell&#39;editor GraphiQL:

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biography {
           html
         }
       }
     }
   }
   ```

1. Eseguire la query e osservare che viene restituito il singolo risultato per il frammento **Stacey Roswells**.

   Nell&#39;esercizio precedente, è stato utilizzato un filtro per restringere un elenco di risultati. È possibile utilizzare una sintassi simile per filtrare in base al percorso, tuttavia la sintassi di cui sopra è preferita per motivi di prestazioni.

1. Ricordate nel capitolo [Authoring Content Fragments](./author-content-fragments.md) che è stata creata una variante **Summary** per **Stacey Roswells**. Aggiornate la query per restituire la variante **Summary**:

   ```graphql
   {
   contributorByPath
   (
       _path: "/content/dam/wknd/en/contributors/stacey-roswells"
       variation: "summary"
   ) {
       item {
         _path
         fullName
         biography {
           html
         }
       }
     }
   }
   ```

   Anche se la variante è stata denominata **Summary**, le varianti sono persistenti in lettere minuscole e quindi viene utilizzato `summary`.

1. Eseguire la query e osservare che il campo `biography` contiene un risultato molto più breve `html`.

## Query per più modelli di frammenti di contenuto {#query-multiple-models}

È inoltre possibile combinare query separate in una singola query. Questo è utile per ridurre al minimo il numero di richieste HTTP necessarie per alimentare l&#39;applicazione. Ad esempio, la vista *Home* di un&#39;applicazione può visualizzare contenuto basato su **due** diversi modelli di frammenti di contenuto. Invece di eseguire **due** query separate, possiamo combinare le query in una singola richiesta.

1. Immettere la seguente query nell&#39;editor GraphiQL:

   ```graphql
   {
     adventureList {
       items {
         _path
         adventureTitle
       }
     }
     contributorList {
       items {
         _path
         fullName
       }
     }
   }
   ```

1. Esegui la query e osserva che il set di risultati contiene i dati di **Avventure** e **Contributor**:

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
          "adventureTitle": "Bali Surf Camp"
        },
        {
          "_path": "/content/dam/wknd/en/adventures/beervana-portland/beervana-in-portland",
          "adventureTitle": "Beervana in Portland"
        },
        ...
      ]
    },
    "contributorList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/contributors/jacob-wester",
          "fullName": "Jacob Wester"
        },
        {
          "_path": "/content/dam/wknd/en/contributors/stacey-roswells",
          "fullName": "Stacey Roswells"
        }
      ]
    }
  }
}
```

## Risorse aggiuntive

Per molti altri esempi di query GraphQL, vedete: [Imparare a utilizzare GraphQL con AEM - Contenuto di esempio e query](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## Congratulazioni! {#congratulations}

Congratulazioni, avete appena creato ed eseguito diverse query GraphQL!

## Passaggi successivi {#next-steps}

Nel capitolo successivo, [AEM di query da un&#39;app React](./graphql-and-external-app.md), verrà illustrato come un&#39;applicazione esterna può eseguire query AEM endpoint GraphQL. L&#39;app esterna che modifica l&#39;app di esempio WKND GraphQL React per aggiungere filtri alle query GraphQL, consentendo all&#39;utente dell&#39;app di filtrare le avventure in base all&#39;attività. Verranno inoltre introdotti alcuni metodi di base per la gestione degli errori.
