---
title: Utilizzo dell’API usagerrights
description: Codice di esempio per applicare i diritti di utilizzo al PDF fornito
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---

# Effettua chiamata API

## Applica diritti di utilizzo

Una volta ottenuto il token di accesso, il passaggio successivo consiste nell’effettuare una richiesta API per applicare i diritti di utilizzo al PDF specificato. Ciò comporta l’inclusione del token di accesso nell’intestazione della richiesta per autenticare la chiamata, garantendo l’elaborazione sicura e autorizzata del documento.

La funzione seguente applica i diritti di utilizzo

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## Suddivisione funzione:



* **Imposta endpoint API e payload**
   * Costruisce l&#39;URL dell&#39;API utilizzando `endPoint` fornito e un `BUCKET` predefinito.
   * Definisce una stringa JSON (`usageRights`) che specifica i diritti da applicare, ad esempio:
      * Commenti
      * File incorporati
      * Compilazione modulo
      * Esportazione dati modulo

* **Carica il file PDF**
   * Recupera il file `withoutusagerights.pdf` dalla directory `pdffiles`.
   * Registra un errore e chiude se il file non viene trovato.

* **Preparare la richiesta HTTP**
   * Legge il file PDF in una matrice di byte.
   * Utilizza `MultipartEntityBuilder` per creare una richiesta in più parti contenente:
      * Il file PDF come corpo binario.
      * JSON `usageRights` come corpo del testo.
   * Imposta una richiesta HTTP `POST` con intestazioni:
      * `Authorization: Bearer <accessToken>` per autenticazione.
      * `X-Adobe-Accept-Experimental: 1` (probabilmente richiesto per compatibilità API).

* **Invia la richiesta e gestisci la risposta**
   * Esegue la richiesta HTTP utilizzando `httpClient.execute(httpPost)`.
   * Legge la risposta (che dovrebbe essere il PDF aggiornato a cui sono applicati i diritti di utilizzo).
   * Scrive il contenuto PDF ricevuto in **&quot;ReaderExtended.pdf&quot;** in `SAVE_LOCATION`.

* **Gestione e pulizia errori**
   * Rileva e registra eventuali errori `IOException`.
   * Verifica che tutte le risorse (flussi, client HTTP, risposta) siano chiuse correttamente nel blocco `finally`.

Di seguito è riportato il codice main.java che richiama la funzione applyUsageRights

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

Il metodo `main` viene inizializzato chiamando `getAccessToken()` da `AccessTokenService`, che dovrebbe restituire un token valido.

* Quindi chiama `applyUsageRights()` dalla classe `DocumentGeneration`, passando:
   * `accessToken` recuperato
   * Endpoint API per l&#39;applicazione dei diritti di utilizzo.


## Passaggi successivi

[Distribuire il progetto di esempio](sample-project.md)
