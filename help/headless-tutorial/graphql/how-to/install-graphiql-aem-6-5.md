---
title: Installare IDE GraphiQL in AEM 6.5
description: Scopri come installare e configurare l’IDE GraphiQL in AEM 6.5
version: Experience Manager 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 14%

---

# Installare IDE GraphiQL in AEM 6.5

In AEM 6.5 lo strumento IDE GraphiQL deve essere installato manualmente.

1. Vai a **[Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html)** > **AEM as a Cloud Service**.
1. Cerca &quot;GraphiQL&quot; (assicurati di includere **i** in **GraphiQL**).
1. Scarica il **pacchetto di contenuti GraphiQL v.x.x.x** più recente.

   ![Scarica pacchetto GraphiQL](assets/graphiql/software-distribution.png)

   Il file zip è un pacchetto AEM che può essere installato direttamente.

1. Dal menu Start di AEM, passa a **Strumenti** > **Distribuzione** > **Pacchetti**.
1. Fai clic su **Carica pacchetto**, quindi seleziona il pacchetto scaricato nel passaggio precedente. Per installare il pacchetto, fai clic su **Installa**.

   ![Installa pacchetto GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Passa a **CRXDE Lite** > **Pannello archivio** > seleziona il nodo `/content/graphiql` (ad esempio, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. Nella scheda **Proprietà** modificare il valore della proprietà `endpoint` in `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Modifica valore proprietà endpoint](assets/graphiql/endpoint-prop-value-change.png)

1. Passa alla configurazione della **console Web** UI > Cerca **filtro CSRF** configurazione (ad esempio,<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. Nell&#39;aggiornamento del campo del nome della proprietà `Excluded Paths`, il percorso dell&#39;endpoint GraphQL WKND a `/content/cq:graphql/wknd-shared/endpoint`.

![Modifica valore proprietà percorsi di esclusione](assets/graphiql/exclude-paths-value-change.png)

1. Accedere all&#39;editor GraphiQL utilizzando `//HOST:PORT/content/graphiql.html` e verificare che sia possibile creare una nuova query o eseguire una query esistente. (esempio: <http://localhost:4502/content/graphiql.html>)

![Editor GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Per supportare lo schema GraphQL specifico del progetto e l&#39;esecuzione di query, è necessario apportare le modifiche corrispondenti ai valori `endpoint` e `Excluded Paths` nei passaggi precedenti.
