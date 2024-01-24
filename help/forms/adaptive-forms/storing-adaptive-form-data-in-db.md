---
title: Memorizzazione dei dati del modulo adattivo
description: Memorizzazione dei dati del modulo adattivo in DataBase come parte del flusso di lavoro AEM
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 3dd552da-fc7c-4fc7-97ec-f20b6cc33df0
last-substantial-update: 2020-03-20T00:00:00Z
duration: 192
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Memorizzazione di invii di moduli adattivi nel database

Esistono diversi modi per memorizzare i dati del modulo inviato nel database desiderato. È possibile utilizzare un&#39;origine dati JDBC per memorizzare direttamente i dati nel database. È possibile scrivere un bundle OSGI personalizzato per memorizzare i dati nel database. Questo articolo utilizza la fase del processo personalizzato nel flusso di lavoro AEM per memorizzare i dati.
Il caso d’uso prevede l’attivazione di un flusso di lavoro AEM all’invio di un modulo adattivo e un passaggio nel flusso di lavoro archivia i dati inviati nel database.



## Pool di connessioni JDBC

* Vai a [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * Cercare &quot;Pool di connessioni JDBC&quot;. Crea un nuovo connection pool JDBC Day Commons. Specificare le impostazioni specifiche del database.

   * ![Configurazione OSGi pool di connessioni JDBC](assets/aemformstutorial-jdbc.png)

## Specifica dettagli database

* Cerca &quot;**Specifica dettagli database**&quot;
* Specificare le proprietà specifiche del database.
   * DataSourceName: nome dell&#39;origine dati configurata in precedenza.
   * TableName: nome della tabella in cui si desidera memorizzare i dati AF
   * FormName: nome della colonna in cui inserire il nome del form
   * ColumnName: nome della colonna in cui inserire i dati AF

  ![Specificare la configurazione OSGi dei dettagli del database](assets/specify-database-details.png)



## Codice per la configurazione OSGi

```java
package com.aemforms.dbsamples.core.insertFormData;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Specify Database details", description = "Specify Database details")

public @interface InsertFormDataConfiguration {
  @AttributeDefinition(name = "DataSourceName", description = "Data Source Name configured")
  String dataSourceName() default "";
  @AttributeDefinition(name = "TableName", description = "Name of the table")
  String tableName() default "";
  @AttributeDefinition(name = "FormName", description = "Column Name for form name")
  String formName() default "";
  @AttributeDefinition(name = "columnName", description = "Column Name for form data")
  String columnName() default "";

}
```

## Leggi valori configurazione

```java
package com.aemforms.dbsamples.core.insertFormData;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(service={InsertFormDataConfigurationService.class})
@Designate(ocd=InsertFormDataConfiguration.class)

public class InsertFormDataConfigurationService {
    public String TABLE_NAME;
    public String DATA_SOURCE_NAME;
    public String COLUMN_NAME;
    public String FORM_NAME;
    @Activate      
      protected final void activate(InsertFormDataConfiguration insertFormDataConfiguration)
      {
        TABLE_NAME = insertFormDataConfiguration.tableName();
        DATA_SOURCE_NAME = insertFormDataConfiguration.dataSourceName();
        COLUMN_NAME = insertFormDataConfiguration.columnName();
        FORM_NAME = insertFormDataConfiguration.formName();
      }
    public String getTABLE_NAME()
    {
        return TABLE_NAME;
    }
    public String getDATA_SOURCE_NAME()
    {
        return DATA_SOURCE_NAME;
    }
    public String getCOLUMN_NAME()
    {
        return COLUMN_NAME;
    }
    public String getFORM_NAME()
    {
        return FORM_NAME;
    }
}
```

## Codice per implementare il passaggio del processo

```java
package com.aemforms.dbsamples.core.insertFormData;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.StandardCharsets;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.sql.DataSource;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.commons.datasource.poolservice.DataSourcePool;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Insert Form Data in Database",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Insert Form Data in Database"
})

public class InsertAfData implements WorkflowProcess {
  @Reference
  InsertFormDataConfigurationService insertFormDataConfig;
  @Reference
  DataSourcePool dataSourcePool;
  private final Logger log = LoggerFactory.getLogger(getClass());
  @Override
  public void execute(WorkItem workItem, WorkflowSession session, MetaDataMap metaDataMap) throws WorkflowException {

    String proccesArgsVals = (String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string");
    String[] values = proccesArgsVals.split(",");
    String AdaptiveFormName = values[0];
    String formDataFile = values[1];
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Session jcrSession = (Session) session.adaptTo((Class) Session.class);
    String dataFilePath = payloadPath + "/" + formDataFile + "/jcr:content";
    log.debug("The data file path is " + dataFilePath);
    PreparedStatement ps = null;
    Connection con = null;
    DataSource dbSource = null;

    try {
      dbSource = (DataSource) dataSourcePool.getDataSource(insertFormDataConfig.getDATA_SOURCE_NAME());
      log.debug("Got db source");
      con = dbSource.getConnection();

      Node xmlDataNode = jcrSession.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      StringWriter writer = new StringWriter();
      String encoding = StandardCharsets.UTF_8.name();
      IOUtils.copy(xmlDataStream, writer, encoding);
      String queryStmt = "insert into " + insertFormDataConfig.TABLE_NAME + "(" + insertFormDataConfig.COLUMN_NAME + "," + insertFormDataConfig.FORM_NAME + ") values(?,?)";
      log.debug("The query Stmt is " + queryStmt);
      ps = con.prepareStatement(queryStmt);
      ps.setString(1, writer.toString());
      ps.setString(2, AdaptiveFormName);
      ps.executeUpdate();

    } catch (Exception e) {
      log.debug("The error message is " + e.getMessage());
    } finally {
      if (ps != null) {
        try {
          ps.close();
        } catch (SQLException sqlException) {
          log.debug(sqlException.getMessage());
        }
      }
      if (con != null) {
        try {
          con.close();
        } catch (SQLException sqlException) {
          log.error("Unable to close connection to database", sqlException);
        }
      }
    }
  }

}
```

## Distribuire le risorse di esempio

* Verificare di aver configurato il pool di connessioni JDBC
* Specificare i dettagli del database utilizzando configMgr
* [Scarica il file ZIP ed estrae il suo contenuto sul disco rigido](assets/article-assets.zip)

   * Distribuisci il file jar tramite [Console web AEM](http://localhost:4502/system/console/bundles). Questo file jar contiene il codice per memorizzare i dati del modulo nel database.

   * Importa i due file zip in [AEM utilizzando Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp). Questo ti darà la [workflow di esempio](http://localhost:4502/editor.html/conf/global/settings/workflow/models/storeformdata.html) e [modulo adattivo di esempio](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) che attiverà il flusso di lavoro all’invio del modulo. Osserva gli argomenti del processo nel passaggio del flusso di lavoro. Questi argomenti indicano il nome del modulo e il nome del file di dati che conterrà i dati del modulo adattivo. Il file di dati viene memorizzato nella cartella del payload nell’archivio crx. Osserva come [modulo adattivo](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) è configurato per attivare il flusso di lavoro AEM all’invio e la configurazione del file di dati (data.xml)

   * Visualizza l’anteprima, compila il modulo e invia. Dovresti visualizzare una nuova riga creata nel database

