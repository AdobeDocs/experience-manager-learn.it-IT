---
title: Memorizzazione e recupero dei dati dei moduli dal database MySQL
description: Esercitazione in più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli
feature: Moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 4%

---


# Distribuisci sul server

>[!NOTE]
>
>Per eseguire questo sul sistema, è necessario quanto segue
>
>* AEM Forms (versione 6.3 o successiva)
>* Database MySql


Per testare questa funzionalità sull’istanza di AEM Forms, segui i seguenti passaggi

* Scarica e distribuisci i file [MySql Driver Jar](assets/mysqldriver.jar) utilizzando la [console Web felix](http://localhost:4502/system/console/bundles)
* Scarica e distribuisci il bundle [OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) utilizzando la [console web felix](http://localhost:4502/system/console/bundles)
* Scarica e installa il pacchetto [contenente client lib, il modello di modulo adattivo e il componente pagina personalizzato](assets/store-and-fetch-af-with-data.zip) utilizzando il [gestore pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa il [modulo adattivo di esempio](assets/sample-adaptive-form.zip) utilizzando l&#39;interfaccia [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importare [form-data-db.sql](assets/form-data-db.sql) utilizzando Workbench MySql. Questo creerà lo schema e le tabelle necessarie nel database affinché questa esercitazione funzioni.
* Accedi a [configMgr.](http://localhost:4502/system/console/configMgr) Cerca &quot;Apache Sling Connection Pooled DataSource. Crea una nuova voce dell&#39;origine dati in pool di connessione Apache Sling denominata **SaveAndContinue** utilizzando le seguenti proprietà:

| Nome proprietà | Valore |
------------------------|---------------------------------------
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


* Apri [Modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Inserisci alcuni dettagli e clicca sul pulsante &quot;Salva e continua più tardi&quot;.
* È necessario recuperare un URL contenente un GUID.
* Copia l’URL e incollalo in una nuova scheda del browser. **Assicurati che non ci sia spazio vuoto alla fine dell&#39;URL.**
* Il modulo adattivo deve essere compilato con i dati del passaggio precedente.
