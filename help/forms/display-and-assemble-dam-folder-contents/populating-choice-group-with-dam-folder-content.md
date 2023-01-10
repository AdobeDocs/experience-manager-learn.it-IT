---
title: Aggiunta di elementi cartella DAM al componente gruppo di scelta
description: Aggiunta dinamica di elementi al componente gruppo di scelta
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Aggiunta dinamica di elementi al componente gruppo di scelta

AEM Forms 6.5 ha introdotto la possibilità di aggiungere in modo dinamico elementi a un componente per un gruppo di scelta Forms adattivo come CheckBox, Pulsante di scelta e Elenco immagini. In questo articolo esamineremo il caso d’uso per la compilazione di un componente del gruppo di scelta con il contenuto della cartella DAM. Nella schermata scattati i 3 file si trovano nella cartella denominata newsletter.Ogni volta che una nuova newsletter viene aggiunta alla cartella, il componente gruppo di scelta viene aggiornato per elencare automaticamente il suo contenuto. L&#39;utente può selezionare una o più newsletter da scaricare.

![Editor regola](assets/newsletters-download.png)

## Crea un servlet per restituire il contenuto della cartella DAM

Il codice seguente è stato scritto per restituire il contenuto della cartella DAM in formato JSON.

```java
package com.newsletters.core.servlets;
import static com.day.cq.commons.jcr.JcrConstants.JCR_CONTENT;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.Gson;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=get",
  "sling.servlet.paths=/bin/listfoldercontents"
})
public class ListFolderContent extends SlingSafeMethodsServlet {
  private static final long serialVersionUID = 1 L;
  private static final Logger log = LoggerFactory.getLogger(ListFolderContent.class);
  protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    Resource resource = request.getResourceResolver().getResource(request.getParameter("damFolder"));
    List < JsonObject > results = new ArrayList < > ();
    resource.getChildren().forEach(child -> {
      if (!JCR_CONTENT.equals(child.getName())) {
        JsonObject asset = new JsonObject();
        log.debug("##The child name is " + child.getName());
        asset.addProperty("assetname", child.getName());
        asset.addProperty("assetpath", child.getPath());
        results.add(asset);

      }
    });
    PrintWriter out = null;
    try {
      out = response.getWriter();
    } catch (IOException e) {

      log.debug(e.getMessage());
    }
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    Gson gson = new Gson();
    out.print(gson.toJson(results));
    out.flush();
  }

}
```

## Creare una libreria client con la funzione JavaScript

Il servlet viene richiamato da una funzione JavaScript. La funzione restituisce un oggetto array che verrà utilizzato per compilare il componente gruppo di scelta

```javascript
/**
 * Populate drop down/choice group  with assets from specified folder
 * @return {string[]} 
 */
function getDAMFolderAssets(damFolder) {
   // strUrl is whatever URL you need to call
   var strUrl = '/bin/listfoldercontents?damFolder=' + damFolder;
   var documents = [];
   $.ajax({
      url: strUrl,
      success: function(jsonData) {
         for (i = 0; i < jsonData.length; i++) {
            documents.push(jsonData[i].assetpath + "=" + jsonData[i].assetname);
         }
      },
      async: false
   });
   return documents;
}
```

## Creare un modulo adattivo

Creare un modulo adattivo e associarlo alla libreria client **listfolk assets**. Aggiungi al modulo un componente casella di controllo. Utilizza l’editor di regole per popolare le opzioni della casella di controllo come mostrato nella schermata
![set-options](assets/set-options-newsletter.png)

Stiamo richiamando la funzione javascript chiamata **getDAMFolderAssets** e passare il percorso delle risorse della cartella DAM all’elenco nel modulo.