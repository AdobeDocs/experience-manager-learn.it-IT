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
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---


# Distribuisci sul server

>[!NOTE]
>
>Per eseguire questa operazione sul sistema, sono necessari i seguenti elementi:
>
>*  AEM Forms(versione 6.3 o successiva)
>* Database MySql


Per testare questa funzionalità nell&#39;istanza di AEM Forms , attenetevi alla seguente procedura

* Scaricare e distribuire i file [MySql Driver Jar](assets/mysqldriver.jar) utilizzando la console Web [felix](http://localhost:4502/system/console/bundles)
* Scaricate e distribuite il pacchetto [OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) utilizzando la [console Web del file ](http://localhost:4502/system/console/bundles)
* Scaricate e installate il pacchetto [contenente la libreria client, il modello di modulo adattivo e il componente pagina personalizzato](assets/store-and-fetch-af-with-data.zip) utilizzando il gestore [pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importare il [modulo adattivo di esempio](assets/sample-adaptive-form.zip) utilizzando l&#39;interfaccia [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importare il [form-data-db.sql](assets/form-data-db.sql) utilizzando MySql Workbench. Questo creerà lo schema e le tabelle necessarie nel database affinché questa esercitazione funzioni.
* Accedi a [configMgr.](http://localhost:4502/system/console/configMgr) Cerca &quot;Apache Sling Connection Pooled DataSource. Create una nuova voce dell&#39;origine dati pool di connessioni Apache Sling denominata **SaveAndContinue** utilizzando le seguenti proprietà:

| Nome proprietà | Valore |
------------------------|---------------------------------------
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


* Aprire il [modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Compila alcuni dettagli e fai clic sul pulsante &quot;Salva e continua più tardi&quot;.
* È necessario recuperare un URL contenente un GUID.
* Copiate l’URL e incollatelo in una nuova scheda del browser. **Accertatevi che alla fine dell’URL non sia presente spazio vuoto.**
* Il modulo adattivo deve essere compilato con i dati del passaggio precedente.
