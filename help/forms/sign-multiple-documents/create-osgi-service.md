---
title: Crea servizio OSGi
description: Crea servizio OSGi per archiviare i moduli da firmare
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
thumbnail: 6886.jpg
jira: KT-6886
topic: Development
role: Developer
level: Experienced
exl-id: 49e7bd65-33fb-44d4-aaa2-50832dffffb0
duration: 150
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# Crea servizio OSGi

Il codice seguente è stato scritto per memorizzare i moduli da firmare. Ogni modulo da firmare è associato a un GUID univoco e a un ID cliente. Pertanto, uno o più moduli possono essere associati allo stesso ID cliente, ma al modulo verrà assegnato un GUID univoco.

## Interfaccia

Di seguito è riportata la dichiarazione di interfaccia utilizzata

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## Inserisci dati

Il metodo insert data inserisce una riga nel database identificato dall&#39;origine dati. Ogni riga del database corrisponde a un modulo ed è identificata in modo univoco da un GUID e da un ID cliente. Anche i dati del modulo e l’URL del modulo vengono memorizzati in questa riga. La colonna di stato indica se il modulo è stato compilato e firmato. Il valore 0 indica che il modulo deve ancora essere firmato.

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## Ottieni dati modulo

Il codice seguente viene utilizzato per recuperare i dati del modulo adattivo associati al GUID specificato. I dati del modulo vengono quindi utilizzati per precompilare il modulo adattivo.

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## Aggiorna stato firma

Il completamento della cerimonia di firma attiva un flusso di lavoro AEM associato al modulo. Il primo passaggio del flusso di lavoro è un passaggio del processo che aggiorna lo stato nel database per la riga identificata dal GUID e dall’ID cliente. Il valore dell&#39;elemento signed nei dati modulo viene inoltre impostato su Y per indicare che il modulo è stato compilato e firmato. Il modulo adattivo viene compilato con tali dati e il valore dell’elemento dati firmato nei dati xml viene utilizzato per visualizzare il messaggio appropriato. Il codice updateSignatureStatus viene richiamato dal passaggio del processo personalizzato.


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## Ottieni modulo successivo da firmare

Il seguente codice è stato utilizzato per ottenere il modulo successivo per la firma per un determinato customerID con stato 0. Se la query SQL non restituisce alcuna riga, verrà restituita la stringa **&quot;AllDone&quot;** che indica che non sono presenti altri moduli per la firma per l&#39;ID cliente specificato.

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## Risorse

Il bundle OSGi con i servizi sopra menzionati può essere [scaricato da qui](assets/sign-multiple-forms.jar)

## Passaggi successivi

[Creare un flusso di lavoro principale per gestire l’invio del modulo iniziale](./create-main-workflow.md)
