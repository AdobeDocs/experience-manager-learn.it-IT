---
title: Crea servlet
description: Creare un servlet per gestire le richieste di POST per salvare i dati del modulo
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6539
thumbnail: 6539.pg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 0%

---

# Crea servlet

Il passo successivo è quello di creare un servlet che chiamerà i metodi appropriati del nostro servizio OSGi personalizzato. Il servlet ha accesso ai dati del modulo adattivo, alle informazioni sugli allegati del file. Il servlet restituisce un ID applicazione univoco che può essere utilizzato per recuperare il modulo adattivo parzialmente completato.

Questo servlet viene richiamato quando l&#39;utente fa clic sul pulsante Salva ed esce sul modulo adattivo

```java
package com.techmarketing.core.servlets;

import java.io.PrintWriter;
import javax.servlet.Servlet;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.JsonObject;
import store.and.fetch.core.*;

@Component(service = {
    Servlet.class
}, property = {
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/storeafdatawithattachments"
})
public class StoreDataInDBWithAttachmentsInfo extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(StoreDataInDBWithAttachmentsInfo.class);
    private static final long serialVersionUID = 1 L;
    @Reference(target = "(&(datasource.name=aemformstutorial))")
    private DataSource dataSource;
    @Reference
    AemFormsAndDB aemFormsAndDB;
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("#### Inside dopost of StoreDataInDBWithAttachmentsInfo ####");
        String afData = request.getParameter("data");
        String tel = request.getParameter("mobileNumber");
        log.debug("$$$The telephone number is  " + tel);
        log.debug("The request parameter " + afData);
        try {
            JSONObject fileMap = new JSONObject(request.getParameter("fileMap").toString());
            String newFileMap = aemFormsAndDB.storeAFAttachments(fileMap, request);
            String application_id = aemFormsAndDB.storeFormData(afData, newFileMap.toString(), tel);
            JsonObject jsonObject = new JsonObject();
            jsonObject.addProperty("path", application_id);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
        } catch (Exception ex) {
            log.debug(ex.getMessage());
        }
    }
}
```
