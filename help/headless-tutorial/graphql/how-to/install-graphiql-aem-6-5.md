---
title: Installare IDE GraphiQL su AEM 6.5
description: Scopri come installare e configurare IDE GraphiQL su AEM 6.5
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 25%

---

# Installare IDE GraphiQL su AEM 6.5

In AEM 6.5 lo strumento IDE GraphiQL deve essere installato manualmente.

1. Vai a **[Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html)** > **AEM as a Cloud Service**.
1. Cerca “GraphiQL” (assicurati di includere **i** in **GraphiQL**).
1. Scarica la versione più recente del **pacchetto di contenuti GraphiQL v.x.x.x**.

   ![Scarica pacchetto GraphiQL](assets/graphiql/software-distribution.png)

   Il file zip è un pacchetto AEM che può essere installato direttamente.

1. Dal menu Start dell’AEM, vai a **Strumenti** > **Distribuzione** > **Pacchetti**.
1. Fai clic su **Carica pacchetto**, quindi seleziona il pacchetto scaricato nel passaggio precedente. Per installare il pacchetto, fai clic su **Installa**.

   ![Installare il pacchetto GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Accedi a **CRXDE Lite** > **Pannello archivio** > seleziona `/content/graphiql` nodo (ad esempio, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. In **Proprietà** valore di modifica scheda di `endpoint` proprietà a `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Modifica valore proprietà endpoint](assets/graphiql/endpoint-prop-value-change.png)

1. Accedi a **Configurazione console web** Interfaccia utente > Cerca **Filtro CSRF** configurazione (ad esempio,<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. In `Excluded Paths` aggiornamento campo nome proprietà, il percorso dell’endpoint GraphQL WKND a `/content/cq:graphql/wknd-shared/endpoint`.

![Escludi modifica valore proprietà percorsi](assets/graphiql/exclude-paths-value-change.png)

1. Accedere all’editor GraphiQL utilizzando `//HOST:PORT/content/graphiql.html`e verificare di poter creare una nuova query o eseguire una query esistente. (ad es. <http://localhost:4502/content/graphiql.html>)

![Editor GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Per supportare lo schema di GraphQL specifico per il progetto e l’esecuzione delle query, è necessario apportare le modifiche corrispondenti per `endpoint` e `Excluded Paths` valori nei passaggi precedenti.
