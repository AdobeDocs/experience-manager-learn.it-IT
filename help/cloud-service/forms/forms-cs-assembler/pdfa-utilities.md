---
title: Converti PDF in PDF/A.
description: Creare e convalidare file PDF/A in Forms CA utilizzando gli endpoint HTTP
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 10105
exl-id: a4955104-8a87-4add-85c7-c3e3395f5f1a
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 3%

---

# Creazione e convalida di documenti PDF/A

PDF/A è una versione standard ISO del formato portatile (PDF) specializzato nell&#39;archiviazione e nella conservazione a lungo termine dei documenti elettronici. PDF/A differisce da PDF per il divieto di funzioni non adatte all’archiviazione a lungo termine, come il collegamento dei font (anziché l’incorporazione dei font) e la crittografia.

## Converti in PDF/A

Il codice seguente è stato utilizzato per convertire PDF in PDF/A

```java
package com.aemformscs.documentservices;
import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.URL;
import org.apache.commons.io.IOUtils;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.HttpMultipartMode;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class PDFAUtilities {
  public String SAVE_LOCATION = "c:\\pdfa";
  public void toPDFA(String postURL) {

    HttpPost httpPost = new HttpPost(postURL);
    CredentialUtilites cu = new CredentialUtilites();
    String accessToken = cu.getAccessToken();
    httpPost.addHeader("Authorization", "Bearer " + accessToken);
    ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
    URL fileToConvert = classLoader.getResource("pdffiles/Address.pdf");
    MultipartEntityBuilder builder = MultipartEntityBuilder.create();

    builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);

    File pdfToConvert = new File(fileToConvert.getPath());
    builder.addBinaryBody("inDoc", pdfToConvert, ContentType.create("application/pdf"), pdfToConvert.getName());
    try {

      HttpEntity entity = builder.build();
      httpPost.setEntity(entity);
      CloseableHttpClient httpclient = HttpClients.createDefault();
      CloseableHttpResponse response = httpclient.execute(httpPost);
      if (response.getStatusLine().getStatusCode() == 200) {
        InputStream generatedPDF = response.getEntity().getContent();
        byte[] bytes = IOUtils.toByteArray(generatedPDF);
        File saveLocation = new File(SAVE_LOCATION);
        if (!saveLocation.exists()) {
          saveLocation.mkdirs();
        }
        File outputFile = new File(SAVE_LOCATION + File.separator + "pdfa.pdf");
        FileOutputStream outputStream = new FileOutputStream(outputFile);
        outputStream.write(bytes);
        outputStream.close();
        System.out.println("PDF was converted to PDFA and  saved to " + SAVE_LOCATION);

      } else {
        String json_string = EntityUtils.toString(response.getEntity());
        JsonObject responseJson = JsonParser.parseString(json_string).getAsJsonObject();
        System.out.println("Could not convert to PDF/A   - " + responseJson.get("message").getAsString());
      }

    } catch (Exception e) {
      System.out.println("The message is " + e.getMessage());
    }
  }

}
```

## Convalida PDF/A

Il codice seguente viene utilizzato per convalidare la conformità di un dato PDF per PDF/A.

```java
public void validatePDFA(String postURL) {

  HttpPost httpPost = new HttpPost(postURL);
  CredentialUtilites cu = new CredentialUtilites();
  String accessToken = cu.getAccessToken();
  httpPost.addHeader("Authorization", "Bearer " + accessToken);
  ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
  URL fileToValidate = classLoader.getResource("pdffiles/pdfa.pdf");
  MultipartEntityBuilder builder = MultipartEntityBuilder.create();

  builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
  builder.addBinaryBody("options", GetOptions.getPDFAOptions().getBytes(), ContentType.APPLICATION_JSON, "options");

  File pdfToValidate = new File(fileToValidate.getPath());
  builder.addBinaryBody("inDoc", pdfToValidate, ContentType.create("application/pdf"), pdfToValidate.getName());
  try {

    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200) {
      String json_string = EntityUtils.toString(response.getEntity());
      JsonObject responseJson = JsonParser.parseString(json_string).getAsJsonObject();
      System.out.println("Th document is    - " + responseJson.toString());

    } else {
      System.out.println(response.getStatusLine().getStatusCode());
    }

  } catch (Exception e) {
    System.out.println("The message is " + e.getMessage());
  }
}
```