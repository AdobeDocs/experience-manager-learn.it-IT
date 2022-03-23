---
title: Memorizzazione e recupero dei dati del modulo dal database MySQL - Distribuzione
description: Esercitazione in più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.3,6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 2%

---

# Distribuisci sul server

>[!NOTE]
>
>Per eseguire questo sul sistema, è necessario quanto segue
>
>* AEM Forms(versione 6.3 o successiva)
>* Database MySql


Per testare questa funzionalità sulla tua istanza di AEM Forms, segui i seguenti passaggi

* Scarica e distribuisci [Jar driver MySql](assets/mysqldriver.jar) file che utilizzano [console web felix](http://localhost:4502/system/console/bundles)
* Scarica e distribuisci [Bundle OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) utilizzando [console web felix](http://localhost:4502/system/console/bundles)
* Scarica e installa la [pacchetto contenente la libreria client, il modello di modulo adattivo e il componente pagina personalizzato](assets/store-and-fetch-af-with-data.zip) utilizzando [gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa [modulo adattivo di esempio](assets/sample-adaptive-form.zip) utilizzando [Interfaccia FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importa [form-data-db.sql](assets/form-data-db.sql) utilizzo di Workbench MySql. Questo creerà lo schema e le tabelle necessarie nel database affinché questa esercitazione funzioni.
* Accedi a [configMgr.](http://localhost:4502/system/console/configMgr) Cerca &quot;Apache Sling Connection Pooled DataSource. Crea una nuova voce di origine dati in pool di connessione Apache Sling denominata **SaveAndContinue** utilizzando le seguenti proprietà:

| Nome proprietà | Valore |
| ------------------------|---------------------------------------|
| Nome origine dati | SaveAndContinue |
| Classe driver JDBC | com.mysql.cj.jdbc.Driver |
| URI di connessione JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

* Apri [Modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Inserisci alcuni dettagli e clicca sul pulsante &quot;Salva e continua più tardi&quot;.
* È necessario recuperare un URL contenente un GUID.
* Copia l’URL e incollalo in una nuova scheda del browser. **Assicurati che non ci sia spazio vuoto alla fine dell&#39;URL.**
* Il modulo adattivo deve essere compilato con i dati del passaggio precedente.
