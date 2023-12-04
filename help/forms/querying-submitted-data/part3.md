---
title: AEM Forms con schema JSON e dati[Part3]
description: Tutorial in più parti per illustrare i passaggi necessari per creare un modulo adattivo con schema JSON e interrogare i dati inviati.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 412eea77-3cf4-43bb-9d2f-ae860cd9d3be
duration: 163
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---

# Memorizzazione dello schema JSON nel database {#storing-json-schema-in-database}


Per poter eseguire una query sui dati inviati, è necessario memorizzare lo schema JSON associato al modulo inviato. Lo schema JSON viene utilizzato nel generatore di query per generare la query.

Quando viene inviato un modulo adattivo, verifichiamo se lo schema JSON associato si trova nel database. Se lo schema JSON non esiste, lo schema JSON viene recuperato e memorizzato nella tabella appropriata. Inoltre, associamo il nome del modulo allo schema JSON. La schermata seguente mostra la tabella in cui sono memorizzati gli schemi JSON.

![jsonschema](assets/jsonschemas.gif)

```java
public String getJSONSchema(String afPath) {
  // TODO Auto-generated method stub
  afPath = afPath.replaceAll("/content/dam/formsanddocuments/", "/content/forms/af/");
  Resource afResource = getResolver.getServiceResolver().getResource(afPath + "/jcr:content/guideContainer");
  javax.jcr.Node resNode = afResource.adaptTo(Node.class);
  String schemaNode = null;
  try {
   schemaNode = resNode.getProperty("schemaRef").getString();
  } catch (ValueFormatException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (PathNotFoundException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (RepositoryException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  if (schemaNode.startsWith("/content/dam")) {
   log.debug("The schema is in the dam");
   afResource = getResolver.getServiceResolver()
     .getResource(schemaNode + "/jcr:content/renditions/original/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }
  if (schemaNode.startsWith("/assets")) {
   afResource = getResolver.getServiceResolver()
     .getResource(afPath + "/jcr:content/guideContainer/" + schemaNode + "/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }

  return null;

 }
```

>[!NOTE]
>
>Durante la creazione di un modulo adattivo è possibile utilizzare lo schema JSON presente nell’archivio o caricare uno schema JSON. Il codice riportato sopra funziona per entrambi i casi.

Lo schema recuperato viene memorizzato nel database utilizzando le operazioni JDBC standard. Il codice seguente inserisce lo schema nel database

```java
public void insertJsonSchema(JSONObject jsonSchema, String afForm) {
  log.debug("$$$$ in insert Schema" + afForm);
  log.debug("$$$$$ The jsonSchema is  " + jsonSchema);
  Connection con = getConnection();
  log.debug("$$$$ got connection is insertJsonSchema");
  String insertTableSQL = "INSERT INTO leads.jsonschemas(jsonschema,formname) VALUES(?,?)";
  PreparedStatement pstmt = null;
  try {

   // org.json.JSONObject jsonSchemaObj = new
   // org.json.JSONObject(jsonSchema);
   pstmt = con.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonSchema.toString());
   pstmt.setString(2, afForm);
   log.debug("Executing the insert  json schema statment  " + pstmt.executeUpdate());
   con.commit();
  } catch (SQLException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } finally {
   if (con != null) {
    try {
     con.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }

 }
```

Per riassumere, finora abbiamo fatto quanto segue

* Creare un modulo adattivo basato sullo schema JSON
* Se il modulo viene inviato la prima volta, lo schema JSON associato al modulo viene memorizzato nel database.
* I dati associati del modulo adattivo vengono memorizzati nel database.

La procedura successiva consiste nell’utilizzare QueryBuilder per visualizzare i campi in cui eseguire ricerche in base allo schema JSON
