---
title: Memorizzazione dei dati del modulo adattivo
seo-title: Memorizzazione dei dati del modulo adattivo
description: Memorizzazione di dati modulo adattivi in DataBase come parte del flusso di lavoro AEM
seo-description: Memorizzazione di dati modulo adattivi in DataBase come parte del flusso di lavoro AEM
feature: adaptive-forms,workflow
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---


# Memorizzazione di invii di moduli adattivi nel database

Esistono diversi modi per memorizzare i dati del modulo inviato nel database di tua scelta. Un&#39;origine dati JDBC può essere utilizzata per archiviare direttamente i dati nel database. È possibile scrivere un bundle OSGI personalizzato per memorizzare i dati nel database. In questo articolo viene utilizzato un processo personalizzato nel flusso di lavoro AEM per memorizzare i dati.
Il caso d’uso consiste nell’attivare un flusso di lavoro AEM per l’invio di un modulo adattivo e un passaggio nel flusso di lavoro memorizza i dati inviati nel database.

**Segui i passaggi indicati di seguito per far sì che questo funzioni sul tuo sistema**

* [Scaricare il file Zip ed estrarne il contenuto sul disco rigido](assets/storeafdataindb.zip)

   * Importa StoreAFInDBWorkflow.zip in AEM utilizzando il gestore pacchetti. Il pacchetto include un flusso di lavoro di esempio che memorizza i dati AF in DB. Aprire il modello di workflow. Il flusso di lavoro ha un solo passaggio. Questo passaggio richiama il codice scritto nel bundle per memorizzare i dati AF nel database. Passo un solo argomento al processo. Si tratta del nome del modulo adattivo i cui dati vengono salvati.
   * Distribuite insertdata.core-0.0.1-SNAPSHOT.jar utilizzando la console Web di Felix. Questo bundle ha il codice per scrivere i dati del modulo inviati nel database

* Vai a [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * Cercate &quot;JDBC Connection Pool&quot;. Crea un nuovo pool di connessioni JDBC Day Commons. Specificate le impostazioni specifiche del database.

   * ![pool di connessioni jdbc](assets/jdbc-connection-pool.png)
   * Cercare &quot;**Inserisci dati modulo in DB**&quot;
   * Specificate le proprietà specifiche del database.
      * DataSourceName:nome dell&#39;origine dati configurata in precedenza.
      * TableName - Nome della tabella in cui si desidera memorizzare i dati AF
      * FormName - Nome della colonna in cui inserire il nome del modulo
      * NomeColonna - Nome colonna per contenere i dati AF

   ![insertdata](assets/insertdata.PNG)

* Creare un modulo adattivo.

* Associate il modulo adattivo a AEM Workflow (StoreAFValuesinDB) come mostrato nella schermata sottostante.

* Accertatevi di specificare &quot;data.xml&quot; nel percorso del file di dati come mostrato nella schermata sottostante

   ![submit](assets/submissionafforms.png)

* Visualizzare l&#39;anteprima del modulo e inviare

* Se tutto è andato bene, è necessario visualizzare i dati modulo memorizzati nella tabella e nella colonna specificata dall&#39;utente



