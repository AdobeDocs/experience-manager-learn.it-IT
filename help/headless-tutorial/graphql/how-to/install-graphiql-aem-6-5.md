---
title: Installa GraphiQL IDE su AEM 6.5.X
description: Scopri come installare e configurare l’IDE GraphiQL nella versione 6.5.X di AEM
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 24%

---


# Installa GraphiQL IDE su AEM 6.5.X

Nella AEM 6.5 lo strumento IDE GraphiQL deve essere installato manualmente, segui i passaggi seguenti per l&#39;installazione e le configurazioni.

1. Vai a **[Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html)** > **AEM as a Cloud Service**.
1. Cerca “GraphiQL” (assicurati di includere **i** in **GraphiQL**.
1. Scarica la versione più recente del **pacchetto di contenuti GraphiQL v.x.x.x**

   ![Scarica il pacchetto GraphiQL](assets/graphiql/software-distribution.png)

   Il file zip è un pacchetto AEM che può essere installato direttamente.

1. Dal menu Start AEM, passa a **Strumenti** > **Distribuzione** > **Pacchetti**.
1. Fai clic su **Carica pacchetto**, quindi seleziona il pacchetto scaricato nel passaggio precedente. Per installare il pacchetto, fai clic su **Installa**.

   ![Installa pacchetto GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Passa a **CRXDE Lite** > **Pannello Repository** > seleziona `/content/graphiql` node (ad esempio, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. In **Proprietà** valore di modifica della scheda di `endpoint` proprietà di `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Modifica valore proprietà endpoint](assets/graphiql/endpoint-prop-value-change.png)

1. Passa a **Configurazione della console Web** Interfaccia utente > Ricerca **Filtro CSRF** configurazione (ad esempio,<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. In `Excluded Paths` aggiornamento del campo del nome della proprietà, percorso dell&#39;endpoint GraphQL WKND a `/content/cq:graphql/wknd-shared/endpoint`.
   ![Modifica del valore della proprietà Escludi percorsi](assets/graphiql/exclude-paths-value-change.png)

1. Accedi all’editor GraphiQL utilizzando `//HOST:PORT/content/graphiql.html`e verifica di poter creare una nuova query o di eseguire una query esistente. (ad es. <http://localhost:4502/content/graphiql.html>)

![Editor GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Per supportare lo schema GraphQL specifico del progetto e l’esecuzione delle query, è necessario apportare le modifiche corrispondenti per `endpoint` e `Excluded Paths` nei passaggi precedenti.
