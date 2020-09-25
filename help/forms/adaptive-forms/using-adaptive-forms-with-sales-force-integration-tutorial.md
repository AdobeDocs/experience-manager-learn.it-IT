---
title: Configurazione di DataSource con Salesforce in  AEM Forms 6.3 e 6.4
seo-title: Configurazione di DataSource con Salesforce in  AEM Forms 6.3 e 6.4
description: Integrazione  AEM Forms con Salesforce tramite il modello dati del modulo
seo-description: Integrazione  AEM Forms con Salesforce tramite il modello dati del modulo
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
translation-type: tm+mt
source-git-commit: cce9f5d1dae05a36b942f6b07a46c65f82eac43c
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 0%

---


# Configurazione di DataSource con Salesforce in  AEM Forms 6.3 e 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Prerequisiti {#prerequisites}

In questo articolo, verrà descritto il processo di creazione di Origini dati con Salesforce

Prerequisiti per questa esercitazione:

* Scorrete fino in fondo a questa pagina e scaricate il file swagger e salvatelo sul disco rigido.
*  AEM Forms con SSL abilitato

   * [Documentazione ufficiale per abilitare SSL su AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentazione ufficiale per abilitare SSL su AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Avrai bisogno di un account Salesforce
* Dovrai creare un&#39;app connessa. La documentazione ufficiale di Salesforce per la creazione dell&#39;app è elencata [qui](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Fornire gli ambiti OAuth appropriati per l&#39;app (ho selezionato tutti gli ambiti OAuth disponibili ai fini del test)
* Fornite l’URL di callback. Nel mio caso, l’URL di callback era

   * Se utilizzate **AEM Forms 6.3**, l&#39;URL di callback sarà https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. In questo URL createlead è il nome del modello dati del modulo.

   * Se utilizzate**  AEM Forms 6.4**, l&#39;URL di callback sarà [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

In questo esempio gbedekar -w7-1:6443 è il nome del server e la porta su cui AEM in esecuzione.

Dopo aver creato la nota dell&#39;app connessa, la nota **Consumatore e la chiave** Segreta. Ne avrete bisogno al momento della creazione dell&#39;origine dati in  AEM Forms.

Dopo aver creato l&#39;app connessa, dovrete creare un file swagger per le operazioni da eseguire in Salesforce. Un file swagger di esempio è incluso come parte delle risorse scaricabili. Questo file swagger consente di creare un oggetto &quot;Lead&quot; per l&#39;invio del modulo adattivo. Esplora questo file swagger.

Il passaggio successivo consiste nel creare un&#39;origine dati in  AEM Forms. Attenetevi alla procedura seguente in base alla versione  AEM Forms

## AEM Forms 6.3 {#aem-forms}

* Effettuate il login a  AEM Forms utilizzando il protocollo https
* Andate ai servizi cloud digitando in https://&lt;nomeserver>:&lt;porta_servizio> /etc/cloudservices.html, ad esempio https://gbedekar-w7-1:6443/etc/cloudservices.html
* Scorri verso il basso fino a &quot;Modello dati modulo&quot;.
* Fare clic su &quot;Mostra configurazioni&quot;.
* Fare clic su &quot;+&quot; per aggiungere la nuova configurazione
* Selezionare &quot;Rest Full Service&quot;. Fornire un Titolo significativo e un Nome alla configurazione. Ad esempio,

   * Nome: CreateLeadInSalesForce
   * Titolo: CreateLeadInSalesForce

* Fare clic su &quot;Crea&quot;

**Nella schermata successiva **

* Selezionate &quot;File&quot; come opzione per il file sorgente swagger. Individuate il file scaricato in precedenza
* Seleziona tipo di autenticazione come OAuth2.0
* Fornire i valori ClientID e Segreto cliente
* Url OAuth: **https://login.salesforce.com/services/oauth2/authorize**
* Aggiorna Url Token - **https://na5.salesforce.com/services/oauth2/token**
* **Url Del Token Di Accesso - https://na5.salesforce.com/services/oauth2/token**
* Ambito di autorizzazione: ** api chatter_api full id aperid refresh_token visualizzazione web***
* Gestore autenticazione: Titolare dell&#39;autorizzazione
* Fare clic su &quot;Connetti a OAUTH&quot;. Se tutto va bene, non si dovrebbero vedere errori

Dopo aver creato il modello dati del modulo con Salesforce, è possibile creare l&#39;integrazione dati del modulo utilizzando l&#39;origine dati appena creata. La documentazione ufficiale per la creazione dell&#39;integrazione dei dati del modulo è [disponibile qui](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Configurare il modello dati modulo per includere il servizio POST per creare un oggetto Lead in SFDC.

Sarà inoltre necessario configurare il servizio di lettura e scrittura per l&#39;oggetto Lead. Consultare le schermate in fondo alla pagina.

Dopo aver creato il modello dati modulo, è possibile creare un Forms adattivo basato su questo modello e utilizzare i metodi di invio del modello dati modulo per creare un lead in SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Crea origine dati

   * [Passa a Origini dati](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Fare clic sul pulsante &quot;Crea&quot;
   * Fornire alcuni valori significativi

      * Nome: CreateLeadInSalesForce
      * Titolo: CreateLeadInSalesForce
      * Tipo di servizio: Servizio RESTful
   * Fai clic su Avanti
   * Sorgente Swagger: File
   * Individuate e selezionate il file swagger scaricato nel passaggio precedente
   * Tipo di autenticazione: OAuth 2.0. Specificare i valori seguenti
   * Fornire i valori ClientID e Segreto cliente
   * Url OAuth: **https://login.salesforce.com/services/oauth2/authorize**
   * Aggiorna Url Token - **https://na5.salesforce.com/services/oauth2/token**
   * Access Token **Url - https://na5.salesforce.com/services/oauth2/token**
   * Ambito di autorizzazione: ** api chatter_api full id aperid refresh_token visualizzazione web***
   * Gestore autenticazione: Titolare dell&#39;autorizzazione
   * Fate clic sul pulsante &quot;Connetti a OAuth&quot;. In caso di errori, controlla i passaggi precedenti per verificare che tutte le informazioni siano state immesse con precisione.


Dopo aver creato l&#39;origine dati tramite SalesForce, è possibile creare l&#39;integrazione dei dati del modulo utilizzando l&#39;origine dati appena creata. Il collegamento della documentazione è [disponibile qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Configurare il modello dati modulo per includere il servizio POST per creare un oggetto Lead in SFDC.

Sarà inoltre necessario configurare il servizio di lettura e scrittura per l&#39;oggetto Lead. Consultare le schermate in fondo alla pagina.

Dopo aver creato il modello dati modulo, è possibile creare un Forms adattivo basato su questo modello e utilizzare i metodi di invio del modello dati modulo per creare un lead in SFDC.

>[!NOTE]
>
>Verificate che l&#39;URL nel file swagger corrisponda alla vostra area geografica. Ad esempio, l’URL nel file di swagger di esempio è &quot;na46.salesforce.com&quot; perché l’account è stato creato in America del Nord. Il modo più semplice è accedere al tuo account Salesforce e controllare l&#39;URL.

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
