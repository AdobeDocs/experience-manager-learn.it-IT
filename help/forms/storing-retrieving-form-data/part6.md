---
title: Memorizzazione e recupero dei dati del modulo dal database MySQL - Distribuzione
description: Tutorial in più parti per illustrare i passaggi necessari per l’archiviazione e il recupero dei dati del modulo
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 47
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 1%

---

# Distribuisci sul server

>[!NOTE]
>
>Per eseguire l&#39;operazione nel sistema sono necessari i seguenti elementi
>
>* AEM Forms (versione 6.3 o successiva)
>* Database MySql

Per testare questa funzionalità nell’istanza di AEM Forms, segui i passaggi seguenti

* Scarica e distribuisci i file [Jar driver MySql](assets/mysqldriver.jar) tramite la [console Web Felix](http://localhost:4502/system/console/bundles)
* Scarica e distribuisci il bundle [OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) utilizzando la [console Web Felix](http://localhost:4502/system/console/bundles)
* Scarica e installa il [pacchetto contenente la libreria client, il modello di modulo adattivo e il componente pagina personalizzato](assets/store-and-fetch-af-with-data.zip) utilizzando [Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa il [modulo adattivo di esempio](assets/sample-adaptive-form.zip) utilizzando l&#39;interfaccia [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importare [form-data-db.sql](assets/form-data-db.sql) utilizzando MySql Workbench. In questo modo verranno creati nel database gli schemi e le tabelle necessari per il funzionamento di questa esercitazione.
* Accedi a [configMgr.](http://localhost:4502/system/console/configMgr) Ricerca &quot;Apache Sling Connection Pooled DataSource&quot;. Crea una nuova voce di origine dati in pool di connessione Apache Sling denominata **SaveAndContinue** utilizzando le seguenti proprietà:

| Nome proprietà | Valore |
| ------------------------|---------------------------------------|
| Nome origine dati | `SaveAndContinue` |
| Classe driver JDBC | `com.mysql.cj.jdbc.Driver` |
| URI connessione JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |

* Apri il [modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Immettere alcuni dettagli e fare clic sul pulsante Salva e continua più tardi.
* È necessario recuperare un URL contenente un GUID.
* Copia l’URL e incollalo in una nuova scheda del browser. **Verificare che non vi siano spazi vuoti alla fine dell&#39;URL.**
* Il modulo adattivo deve essere compilato con i dati del passaggio precedente.
