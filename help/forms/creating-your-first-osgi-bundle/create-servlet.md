---
title: Creazione del primo servlet in AEM Forms
description: Crea il tuo primo servlet sling per unire i dati con il modello di modulo.
feature: Moduli adattivi
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 2%

---


# Servlet Sling

Un Servlet è una classe utilizzata per estendere le funzionalità dei server che ospitano le applicazioni a cui si accede tramite un modello di programmazione richiesta-risposta. Per tali applicazioni, la tecnologia Servlet definisce classi di servlet specifiche per HTTP.
Tutti i servlet devono implementare l’interfaccia Servlet, che definisce i metodi del ciclo di vita.


Un servlet in AEM può essere registrato come servizio OSGi: puoi estendere SlingSafeMethodsServlet per l&#39;implementazione di sola lettura o SlingAllMethodsServlet per implementare tutte le operazioni RESTful.

## Codice servlet

```java
import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

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
* Accedi a `c:\aemformsbundles\learningaemforms\core`
* Esegui il comando `mvn clean install -PautoInstallBundle`
* Il comando precedente genera e distribuisce automaticamente il bundle nella tua istanza AEM in esecuzione su localhost:4502

Il bundle sarà disponibile anche nel seguente percorso `C:\AEMFormsBundles\learningaemforms\core\target`. Il bundle può anche essere distribuito in AEM utilizzando la [console web Felix.](http://localhost:4502/system/console/bundles)


## Test del Risolutore Servlet

Posiziona il browser sull&#39; [URL del risolutore del servlet](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). Questo ti dirà il servlet che verrà richiamato per un determinato percorso come visto nella schermata sottostante
![servlet-resolver](assets/servlet-resolver.JPG)

## Test del servlet tramite Postman

![postino del servlet di prova](assets/test-servlet-postman.JPG)
