---
title: Memorizzazione e recupero dei dati del modulo dal database MySQL
description: Esercitazione con più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Distribuisci sul server

>[!NOTE]
Per eseguire questa operazione sul sistemaAEM Forms(versione 6.3 o successiva)MYSQL Database

Per testare questa funzionalità nell&#39;istanza di AEM Forms , attenetevi alla seguente procedura

* Scaricate e decomprimete le risorse [dell’](assets/store-retrieve-form-data.zip) esercitazione nel sistema locale
* Implementare e avviare i bundle techmarketingdemos.jar e mysqldriver.jar utilizzando la console Web [Felix](http://localhost:4502/system/console/configMgr)
* Importare aemformstutorial.sql utilizzando MYSQL Workbench. Questo creerà lo schema e le tabelle necessarie nel database affinché questa esercitazione funzioni.
* Importa StoreAndRetrieve.zip tramite [AEM gestore pacchetti.](http://localhost:4502/crx/packmgr/index.jsp) Questo pacchetto contiene il modello di modulo adattivo, la libreria client per i componenti della pagina e un esempio di configurazione di modulo adattivo e origine dati.
* Accedi a [configMgr.](http://localhost:4502/system/console/configMgr) Cerca &quot;Apache Sling Connection Pooled DataSource. Aprite la voce dell&#39;origine dati associata ad aemformstutorial e immettete il nome utente e la password specifici per l&#39;istanza del database.
* Aprire il modulo [adattivo](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Compila alcuni dettagli e clicca sul pulsante &quot;Salva e continua più tardi&quot;
* È necessario recuperare un URL contenente un GUID.
* Copiate l’URL e incollatelo in una nuova scheda del browser. **Accertatevi che alla fine dell’URL non sia presente spazio vuoto**
* Il modulo adattivo deve essere compilato con i dati del passaggio precedente
