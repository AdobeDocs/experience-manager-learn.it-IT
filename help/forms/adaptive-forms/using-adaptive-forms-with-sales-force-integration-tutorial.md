---
title: Configurazione di DataSource con Salesforce in AEM Forms 6.3 e 6.4
description: Integrazione di AEM Forms con Salesforce tramite il modello dati del modulo
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
duration: 175
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Configurazione di DataSource con Salesforce in AEM Forms 6.3 e 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Prerequisiti {#prerequisites}

Questo articolo illustra come creare Data Source con Salesforce

Prerequisiti per questa esercitazione:

* Scorri fino alla parte inferiore della pagina, scarica il file swagger e salvalo sul disco rigido.
* AEM Forms con SSL abilitato

   * [Documentazione ufficiale per l&#39;abilitazione di SSL su AEM 6.3](https://helpx.adobe.com/it/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentazione ufficiale per l&#39;abilitazione di SSL su AEM 6.4](https://helpx.adobe.com/it/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Devi avere un account Salesforce
* Devi creare un’app connessa. La documentazione ufficiale di Salesforce per la creazione dell&#39;app è elencata [qui](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Fornisci ambiti OAuth appropriati per l&#39;app (ho selezionato tutti gli ambiti OAuth disponibili a scopo di test)
* Specifica l&#39;URL di callback. L&#39;URL di callback nel mio caso era

   * Se si utilizza **AEM Forms 6.3**, l&#39;URL di callback è https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. In questo URL createlead è il nome del modello dati del modulo.

   * Se utilizzi **&#x200B; AEM Forms 6.4**, l’URL di callback è https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

In questo esempio gbedekar -w7-1:6443 è il nome del server e della porta su cui è in esecuzione AEM.

Dopo aver creato l&#39;app connessa, prendere nota di **Chiave consumer e Chiave segreta**. Queste sono necessarie quando crei l’origine dati in AEM Forms.

Dopo aver creato l’app connessa, devi creare un file swagger per le operazioni da eseguire in salesforce. Un esempio di file swagger è incluso come parte delle risorse scaricabili. Questo file swagger consente di creare un oggetto &quot;Lead&quot; all’invio di un modulo adattivo. Esplora questo file Swagger.

Il passaggio successivo consiste nel creare Data Source in AEM Forms. Segui i seguenti passaggi in base alla versione di AEM Forms in uso

## AEM Forms 6.3 {#aem-forms}

* Accedi ad AEM Forms utilizzando il protocollo https
* Passa ai servizi cloud digitando in https://&lt;servername>:&lt;serverport> /etc/cloudservices.html, ad esempio https://gbedekar-w7-1:6443/etc/cloudservices.html
* Scorri verso il basso fino a &quot;Modello dati modulo&quot;.
* Fai clic su &quot;Mostra configurazioni&quot;.
* Fai clic su &quot;+&quot; per aggiungere una nuova configurazione
* Selezionare &quot;Rest Full Service&quot;. Fornisci un titolo e un nome significativi alla configurazione. Ad esempio:

   * Nome: CreateLeadInSalesForce
   * Titolo: CreateLeadInSalesForce

* Fai clic su &quot;Crea&quot;

**Nella schermata successiva &#x200B;**

* Selezionate &quot;File&quot; come opzione per il file di origine Swagger. Individua il file scaricato in precedenza
* Seleziona tipo di autenticazione come OAuth2.0
* Fornisci i valori ClientID e ClientSecret
* L&#39;URL OAuth è - **https://login.salesforce.com/services/oauth2/authorize**
* Aggiorna url token - **https://na5.salesforce.com/services/oauth2/token**
* **URL token di accesso - https://na5.salesforce.com/services/oauth2/token**
* Ambito autorizzazione: API **&#x200B;   id completo chatter_api   openid   refresh_token visualforce web**
* Gestore autenticazione: Bearer di autorizzazione
* Fai clic su &quot;Connetti a OAUTH &quot;.Se tutto va bene, non dovresti vedere alcun errore

Dopo aver creato il modello dati modulo con Salesforce, puoi creare l’integrazione dati modulo utilizzando il Source dati appena creato. La documentazione ufficiale per la creazione dell&#39;integrazione dei dati del modulo è [qui](https://helpx.adobe.com/it/aem-forms/6-3/data-integration.html).

Assicurati di configurare il modello dati modulo per includere il servizio POST per creare un oggetto Lead in SFDC.

Sarà inoltre necessario configurare il servizio di lettura e scrittura per l&#39;oggetto Lead. Fai riferimento alle schermate in fondo a questa pagina.

Dopo aver creato il modello dati modulo, puoi creare un Forms adattivo basato su questo modello e utilizzare i metodi di invio del modello dati modulo per creare un lead in SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Creazione di Data Source

   * [Passa a origini dati](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Fai clic sul pulsante &quot;Crea&quot;
   * Fornisci alcuni valori significativi

      * Nome: CreateLeadInSalesForce
      * Titolo: CreateLeadInSalesForce
      * Tipo di servizio: servizio RESTful

   * Fai clic su Successivo
   * Swagger Source: File
   * Sfoglia e seleziona il file Swagger scaricato nel passaggio precedente
   * Tipo di autenticazione: OAuth 2.0. Specifica i seguenti valori
   * Fornisci i valori ClientID e ClientSecret
   * L&#39;URL OAuth è - **https://login.salesforce.com/services/oauth2/authorize**
   * Aggiorna url token - **https://na5.salesforce.com/services/oauth2/token**
   * Token di accesso Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * Ambito autorizzazione: **&#x200B; api chatter_api id completo openid refresh_token visualforce web**
   * Gestore autenticazione: Bearer di autorizzazione
   * Fai clic sul pulsante &quot;Connetti a OAuth&quot;. In caso di errori, controlla i passaggi precedenti per assicurarti che tutte le informazioni siano state inserite correttamente.

Dopo aver creato il Data Source utilizzando Salesforce, puoi creare l’integrazione dei dati del modulo utilizzando il Data Source appena creato. Il collegamento alla documentazione per questo è [qui](https://helpx.adobe.com/it/experience-manager/6-4/forms/using/create-form-data-models.html)

Assicurati di configurare il modello dati modulo per includere il servizio POST per creare un oggetto Lead in SFDC.

Sarà inoltre necessario configurare il servizio di lettura e scrittura per l&#39;oggetto Lead. Fai riferimento alle schermate in fondo a questa pagina.

Dopo aver creato il modello dati modulo, puoi creare un Forms adattivo basato su questo modello e utilizzare i metodi di invio del modello dati modulo per creare un lead in SFDC.

>[!NOTE]
>
>Assicurati che l’URL nel file Swagger corrisponda all’area geografica. Ad esempio, l’URL nel file swagger di esempio è &quot;na46.salesforce.com&quot; in quanto l’account è stato creato in Nord America. Il modo più semplice è accedere al tuo account Salesforce e controllare l&#39;URL .

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[FileSwaggerCampione](assets/swagger-sales-force-lead.json)
