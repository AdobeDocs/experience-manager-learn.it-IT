---
title: AEM Forms con schema JSON e dati[Parte2]
seo-title: AEM Forms with JSON Schema and Data[Part2]
description: Esercitazione in più parti per illustrarti i passaggi necessari per creare un modulo adattivo con schema JSON e per eseguire query sui dati inviati.
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Memorizzazione dei dati inviati nel database


>[!NOTE]
>
>Si consiglia di utilizzare MySQL 8 come database in quanto dispone del supporto per il tipo di dati JSON. Sarà inoltre necessario installare il driver appropriato per MySQL DB. Ho usato il driver disponibile in questa posizione https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12

Per memorizzare i dati inviati nel database, scriveremo un servlet per estrarre i dati associati, il nome e l’archivio del modulo. Di seguito è riportato il codice completo per gestire l&#39;invio del modulo e memorizzare i afBoundData nel database.

È stato creato un invio personalizzato per gestire l’invio del modulo. In questo post di invio personalizzato.POST.jsp inoltriamo la richiesta al nostro servlet.

Per ulteriori informazioni sull’invio personalizzato, consulta questo [articolo](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmit&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![connection pool](assets/connectionpooled.gif)

Per far funzionare questo sistema, segui i seguenti passaggi

* [Scarica e decomprimi il file zip](assets/aemformswithjson.zip)
* Crea AdaptiveForm con lo schema JSON. Puoi utilizzare lo schema JSON fornito come parte di questa risorsa articolo. Verificare che l’azione di invio del modulo sia configurata in modo appropriato. L’azione di invio deve essere configurata in &quot;CustomSubmitHelpx&quot;.
* Creare uno schema nell&#39;istanza MySQL importando il file schema.sql tramite lo strumento Workbench MySQL. Il file schema.sql viene fornito anche come parte di questa esercitazione risorse.
* Configura l&#39;origine dati in pool di connessione Apache Sling dalla console web Felix
* Assicurati di assegnare un nome alla tua origine dati &quot;aemformswithjson&quot;. Questo è il nome utilizzato dal bundle OSGi di esempio fornito all&#39;utente
* Fai riferimento all&#39;immagine precedente per le proprietà. Si presume che verrà utilizzato MySQL come database.
* Distribuisci i bundle OSGi forniti come parte delle risorse di questo articolo.
* Visualizzare l’anteprima del modulo e inviare il modulo.
* I dati JSON vengono memorizzati nel database creato al momento dell’importazione del file &quot;schema.sql&quot;.
