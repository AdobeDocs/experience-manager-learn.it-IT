---
title: Configurazione di DataSource con Salesforce in AEM Forms 6.3 e 6.4
description: Integrazione di AEM Forms con Salesforce tramite il modello dati del modulo
feature: Adaptive Forms, Form Data Model
topics: integrations
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '900'
ht-degree: 0%

---


# Configurazione di DataSource con Salesforce in AEM Forms 6.3 e 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Prerequisiti {#prerequisites}

In questo articolo, passeremo attraverso il processo di creazione di Origine dati con Salesforce

Prerequisiti per questa esercitazione:

* Scorri fino alla parte inferiore della pagina e scarica il file swagger e salvalo il disco rigido.
* AEM Forms con SSL abilitato

   * [Documentazione ufficiale per abilitare SSL al AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentazione ufficiale per abilitare SSL al AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Avrai bisogno dell’account Salesforce
* Sarà necessario creare un’app connessa. La documentazione ufficiale di Salesforce per la creazione dell&#39;app è elencata [qui](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Fornire ambiti OAuth appropriati per l’app (ho selezionato tutti gli ambiti OAuth disponibili ai fini del test)
* Specifica l&#39;URL di callback. Nel mio caso, l’URL di callback era

   * Se utilizzi **AEM Forms 6.3**, l&#39;URL di callback sarà https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. In questo URL createlead è il nome del mio modello di dati del modulo.

   * Se utilizzi** AEM Forms 6.4**, l’URL di callback sarà https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

In questo esempio gbedekar -w7-1:6443 è il nome del mio server e la porta su cui AEM in esecuzione.

Una volta creata la nota dell&#39;app connessa, la **Chiave consumer e Chiave segreta**. Queste funzioni sono necessarie al momento della creazione dell’origine dati in AEM Forms.

Dopo aver creato l’app connessa, dovrai creare un file swagger per le operazioni da eseguire in salesforce. Un file swagger di esempio è incluso come parte delle risorse scaricabili. Questo file swagger consente di creare un oggetto &quot;Lead&quot; durante l’invio del modulo adattivo. Esplorare questo file swagger.

Il passaggio successivo consiste nel creare un’origine dati in AEM Forms. Segui i seguenti passaggi in base alla tua versione di AEM Forms

## AEM Forms 6.3 {#aem-forms}

* Accedi ad AEM Forms utilizzando il protocollo https
* Passa a servizi cloud digitando in https://&lt;servername>:&lt;serverport> /etc/cloudservices.html, ad esempio https://gbedekar-w7-1:6443/etc/cloudservices.html
* Scorri verso il basso fino a &quot;Modello dati modulo&quot;.
* Fai clic su &quot;Mostra configurazioni&quot;.
* Fai clic su &quot;+&quot; per aggiungere una nuova configurazione
* Selezionare &quot;Rest Full Service&quot;. Fornisci un titolo e un nome significativi alla configurazione. Ad esempio,

   * Nome: CreateLeadInSalesForce
   * Titolo: CreateLeadInSalesForce

* Fai clic su &quot;Crea&quot;

**Nella schermata successiva **

* Selezionare &quot;File&quot; come opzione per il file di origine swagger. Sfoglia il file scaricato in precedenza
* Seleziona il tipo di autenticazione come OAuth2.0
* Fornire i valori ClientID e Segreto client
* L&#39;URL OAuth è - **https://login.salesforce.com/services/oauth2/authorize**
* Aggiorna URL token - **https://na5.salesforce.com/services/oauth2/token**
* **Url Del Token Di Accesso - https://na5.salesforce.com/services/oauth2/token**
* Ambito dell&#39;autorizzazione: ** api   chatter_api full id   openid   refresh_token visualforce web**
* Gestore autenticazione: Titolare dell’autorizzazione
* Clicca su &quot;Connetti a OAUTH&quot;.Se tutto va bene, non dovresti vedere errori

Dopo aver creato il modello dati del modulo utilizzando Salesforce, è possibile creare l’integrazione dati del modulo utilizzando l’origine dati appena creata. La documentazione ufficiale per la creazione dell’integrazione dei dati del modulo è [qui](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Assicurati di configurare il Modello dati modulo in modo da includere il servizio POST per creare un oggetto Lead in SFDC.

Sarà inoltre necessario configurare il servizio di lettura e scrittura per l’oggetto Lead. Fai riferimento alle schermate in fondo a questa pagina.

Dopo aver creato il modello dati del modulo, è possibile creare il modello Adattivo Forms basato su questo modello e utilizzare i metodi di invio del modello dati del modulo per creare il lead in SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Crea origine dati

   * [Passa a Origini dati](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Fai clic sul pulsante &quot;Crea&quot;
   * Fornire alcuni valori significativi

      * Nome: CreateLeadInSalesForce
      * Titolo: CreateLeadInSalesForce
      * Tipo di servizio: Servizio RESTful
   * Fai clic su Avanti
   * Sorgente Swagger: File
   * Sfoglia e seleziona il file swagger scaricato nel passaggio precedente
   * Tipo di autenticazione: OAuth 2.0. Specifica i seguenti valori
   * Fornire i valori ClientID e Segreto client
   * L&#39;URL OAuth è - **https://login.salesforce.com/services/oauth2/authorize**
   * Aggiorna URL token - **https://na5.salesforce.com/services/oauth2/token**
   * Access Token Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * Ambito dell&#39;autorizzazione: ** api chatter_api full id openid refresh_token visualforce web***
   * Gestore autenticazione: Titolare dell’autorizzazione
   * Fai clic sul pulsante &quot;Connetti a OAuth&quot;. In caso di errori, controlla i passaggi precedenti per assicurarsi che tutte le informazioni siano state inserite con precisione.


Dopo aver creato l&#39;origine dati utilizzando SalesForce, è possibile creare l&#39;integrazione dei dati del modulo utilizzando l&#39;origine dati appena creata. Il collegamento alla documentazione relativo a questo percorso è [qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Assicurati di configurare il Modello dati modulo in modo da includere il servizio POST per creare un oggetto Lead in SFDC.

Sarà inoltre necessario configurare il servizio di lettura e scrittura per l’oggetto Lead. Fai riferimento alle schermate in fondo a questa pagina.

Dopo aver creato il modello dati del modulo, è possibile creare il modello Adattivo Forms basato su questo modello e utilizzare i metodi di invio del modello dati del modulo per creare il lead in SFDC.

>[!NOTE]
>
>Assicurati che l&#39;url nel file swagger corrisponda alla tua area geografica. Ad esempio, l’url nel file di swagger di esempio è &quot;na46.salesforce.com&quot;, in quanto l’account è stato creato in Nord America. Il modo più semplice è quello di accedere al tuo account Salesforce e controllare l&#39;url .

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
