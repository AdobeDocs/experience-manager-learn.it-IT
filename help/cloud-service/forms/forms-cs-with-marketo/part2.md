---
title: Integrare il Cloud Service AEM Forms e Marketo (Parte 2)
description: Scopri come integrare AEM Forms e Marketo utilizzando AEM Forms Form Data Model.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 3%

---

# Creazione di Data Source

Le API REST di Marketo sono autenticate con OAuth 2.0 a 2 gambe. Possiamo creare facilmente un’origine dati utilizzando il file swagger scaricato nel passaggio precedente

## Crea contenitore di configurazione

* Accedi all’AEM.
* Fare clic sul menu Strumenti e quindi su **Browser di configurazione** come illustrato di seguito

* ![menu strumenti](assets/datasource3.png)

* Fai clic su **Crea** e fornisci un nome significativo come mostrato di seguito. Accertati di selezionare l’opzione Configurazioni cloud come mostrato di seguito

* ![contenitore configurazione](assets/datasource4.png)

## Creare servizi cloud

* Passa al menu Strumenti, quindi fai clic su Cloud Services -> Origini dati

* ![servizi cloud](assets/datasource5.png)

* Seleziona il contenitore di configurazione creato nel passaggio precedente e fai clic su **Crea** per creare una nuova origine dati.Specifica un nome significativo, seleziona il servizio RESTful dall&#39;elenco a discesa Tipo di servizio, quindi fai clic su **Avanti**
* ![new-data-source](assets/datasource6.png)

* Carica il file Swagger e specifica il tipo di concessione, l’ID client, il segreto client e l’URL del token di accesso specifici per la tua istanza di Marketo, come illustrato nella schermata seguente.

* Verifica la connessione e, se questa ha esito positivo, assicurati di fare clic sul pulsante blu **Crea** per completare il processo di creazione dell&#39;origine dati.

* ![data-source-config](assets/datasource1.png)


## Passaggi successivi

[Crea modello dati modulo](./part3.md)
