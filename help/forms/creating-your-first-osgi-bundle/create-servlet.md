---
title: Creazione del primo servlet in AEM Forms
description: Crea il tuo primo servlet sling per unire i dati con il modello di modulo.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
last-substantial-update: 2021-04-23T00:00:00Z
duration: 55
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Servlet Sling

Un servlet è una classe utilizzata per estendere le funzionalità dei server che ospitano le applicazioni a cui si accede tramite un modello di programmazione richiesta-risposta. Per tali applicazioni, la tecnologia Servlet definisce classi servlet specifiche per HTTP.
Tutti i servlet devono implementare l&#39;interfaccia Servlet, che definisce i metodi del ciclo di vita.


Un servlet in AEM può essere registrato come servizio OSGi: è possibile estendere SlingSafeMethodsServlet per l’implementazione in sola lettura o SlingAllMethodsServlet per implementare tutte le operazioni RESTful.

## Codice servlet

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import java.io.File;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
    
    private static final long serialVersionUID = 1L;
    @Reference
    FormsService formsService;
     protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
      { 
         String file_path = request.getParameter("save_location");
         
         java.io.InputStream pdf_document_is = null;
         java.io.InputStream xml_is = null;
         javax.servlet.http.Part pdf_document_part = null;
         javax.servlet.http.Part xml_data_part = null;
              try
              {
                 pdf_document_part = request.getPart("pdf_file");
                 xml_data_part = request.getPart("xml_data_file");
                 pdf_document_is = pdf_document_part.getInputStream();
                 xml_is = xml_data_part.getInputStream();
                 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
                 data_merged_document.copyToFile(new File(file_path));
                 
              }
              catch(Exception e)
              {
                  response.sendError(400,e.getMessage());
              }
      }
}
```

## Creare e distribuire

Per creare il progetto, effettua le seguenti operazioni:

* Apri **finestra del prompt dei comandi**
* Passa a `c:\aemformsbundles\mysite\core`
* Esegui il comando `mvn clean install -PautoInstallBundle`
* Il comando precedente crea e distribuisce automaticamente il bundle nell’istanza AEM in esecuzione su localhost:4502

Il bundle è disponibile anche nel seguente percorso `C:\AEMFormsBundles\mysite\core\target`. Il bundle può anche essere distribuito in AEM utilizzando la [console Web Felix.](http://localhost:4502/system/console/bundles)


## Test del servlet Resolver

Puntare il browser all&#39;[URL resolver servlet](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). Questo indica il servlet richiamato per un determinato percorso come mostrato nella schermata seguente
![risolutore servlet](assets/servlet-resolver.JPG)

## Test del servlet tramite Postman

![Verifica il servlet tramite Postman](assets/test-servlet-postman.JPG)

## Passaggi successivi

[Includi file jar di terze parti](./include-third-party-jars.md)

