---
title: Memorizzazione e recupero dei dati del modulo dal database MySQL - Servlet per memorizzare i dati del modulo
description: Esercitazione in più parti per illustrare i passaggi necessari per memorizzare e recuperare i dati dei moduli
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: dd82f309-dd4e-42ce-8856-e51c898024f5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 0%

---

# Servlet per memorizzare i dati del modulo

Il passaggio successivo consiste nel creare un servlet che inserisca o aggiorni i dati del modulo. Il servlet chiama i metodi appropriati del servizio OSGi per inserire o aggiornare il database. I dati del modulo adattivo memorizzato sono associati a un GUID. Lo stesso GUID viene quindi utilizzato per aggiornare i dati del modulo. Questo servlet viene chiamato quando si fa clic sul pulsante &quot;SaveAndContinueLater&quot; (Salva e continua).

```java
package com.aemforms.saveandcontinue.core.servlets;

import java.io.IOException;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.aemforms.saveandcontinue.core.FetchStoredFormData;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
},
property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/storeafdata"
})
public class StoreDataInDB extends SlingAllMethodsServlet {
  private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
  private static final long serialVersionUID = 1L;
  @Reference
  FetchStoredFormData fetchStoredFormData;
  protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    log.debug("Inside my save af data servlet");
    if (request.getParameter("operation").equalsIgnoreCase("update")) {
      log.debug("The operation is update");
      log.debug("The data I got was " + request.getParameter("formdata"));
      String guid = fetchStoredFormData.updateData(request.getParameter("guid"), request.getParameter("formdata"));
      log.debug("The guid I got was  " + guid);
      JsonObject jsonResponse = new JsonObject();
      try {
        jsonResponse.addProperty("guid", guid);
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(jsonResponse.toString());

      } catch(IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }
    }

    if (request.getParameter("operation").equalsIgnoreCase("insert")) {
      log.debug("The data I got was " + request.getParameter("formdata"));
      String guid = fetchStoredFormData.storeFormData(request.getParameter("formdata"));
      log.debug("The guid on inserting data  " + guid);
      JsonObject jsonResponse = new JsonObject();
      try {
        jsonResponse.addProperty("guid", guid);
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(jsonResponse.toString());

      } catch(IOException e) {
        // TODO Auto-generated catch block
        log.debug("error in writing response  " + e.getMessage());
      }
    }

  }

}
```
