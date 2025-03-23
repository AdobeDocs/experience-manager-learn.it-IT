---
title: Aggiunta di elementi cartella DAM al componente gruppo di scelta
description: Aggiungi elementi al componente gruppo di scelta in modo dinamico
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 29f56d13-c2e2-4bc2-bfdc-664c848dd851
duration: 80
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---

# Aggiunta dinamica di elementi al componente gruppo di scelta

In AEM Forms 6.5 è stata introdotta la possibilità di aggiungere elementi in modo dinamico a un componente del gruppo di scelta di Forms adattivo, ad esempio CheckBox, Pulsante di scelta ed Elenco immagini. In questo articolo esamineremo il caso d’uso per compilare un componente del gruppo di scelta con il contenuto della cartella DAM. Nella schermata i 3 file si trovano nella cartella denominata newsletter.Ogni volta che una nuova newsletter viene aggiunta alla cartella, il componente del gruppo di scelta verrà aggiornato per elencare automaticamente il suo contenuto. L&#39;utente può selezionare una o più newsletter da scaricare.

![Editor di regole](assets/newsletters-download.png)

## Crea un servlet per restituire il contenuto della cartella DAM

Il seguente codice è stato scritto per restituire il contenuto della cartella DAM in formato JSON.

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

Il servlet viene richiamato da una funzione JavaScript. La funzione restituisce un oggetto array che verrà utilizzato per popolare il componente gruppo di scelta

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

Crea un modulo adattivo e associalo alla libreria client **listfolderassets**. Aggiungi al modulo un componente casella di controllo. Utilizza l’editor di regole per popolare le opzioni della casella di controllo come mostrato nella schermata
![set-options](assets/set-options-newsletter.png)

Si sta richiamando la funzione JavaScript denominata **getDAMFolderAssets** e si sta passando il percorso delle risorse della cartella DAM da elencare nel modulo.

## Passaggi successivi

[Assembla Assets selezionato](./assemble-selected-newsletters.md)
