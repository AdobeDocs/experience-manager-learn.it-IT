---
title: Utilizzo dell’API GuideBridge per accedere ai dati del modulo
description: Accedi ai dati e agli allegati dei moduli utilizzando l’API GuideBridge per un modulo adattivo basato su un componente core.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-15286
last-substantial-update: 2024-04-05T00:00:00Z
exl-id: 099aaeaf-2514-4459-81a7-2843baa1c981
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Utilizzo dell’API GuideBridge per l’utilizzo dei dati dei moduli POST

Per &quot;salvare e riprendere&quot; un modulo è necessario consentire agli utenti di salvare l&#39;avanzamento della compilazione e di riprenderla in un secondo momento.
Per eseguire questo caso d’uso, è necessario accedere e inviare i dati del modulo utilizzando l’API GuideBridge all’endpoint REST per l’archiviazione e il recupero.

I dati del modulo vengono salvati nell’evento clic di un pulsante utilizzando l’editor di regole
![editor di regole](assets/rule-editor.png)

È stata scritta la seguente funzione JavaScript per inviare i dati all&#39;endpoint specificato

```javascript
/**
* Submits data and attachments 
* @name submitFormDataAndAttachments Submit form data and attachments to REST end point
* @param {string} endpoint in Stringformat
* @return {string} 
 */
 
 function submitFormDataAndAttachments(endpoint) {
    guideBridge.getFormDataObject({
        success: function(resultObj) {
            const afFormData = resultObj.data.data;
            const formData = new FormData();
            formData.append("dataXml", afFormData);
            resultObj.data.attachments.forEach(attachment => {
                console.log(attachment.name);
                formData.append(attachment.name, attachment.data);
            });
            fetch(endpoint, {
                method: 'POST',
                body: formData
            })
            .then(response => {
                if (response.ok) {
                    console.log("successfully saved");
                    const fld = guideBridge.resolveNode("$form.confirmation");
                    return "Form data was saved successfully";
                } else {
                    throw new Error('Failed to save form data');
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    });
}
```



## Codice lato server

Il seguente codice Java lato server è stato scritto per gestire i dati del modulo. Di seguito è riportato il servlet Java in esecuzione in AEM che viene chiamato tramite la chiamata XHR nel JavaScript precedente.

```java
package com.azuredemo.core.servlets;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import javax.servlet.Servlet;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.Serializable;
import java.util.List;
@Component(
   service = {
      Servlet.class
   }
)
@SlingServletResourceTypes(
   resourceTypes = "azure/handleFormSubmission",
   methods = "POST",
   extensions = "json"
)
public class StoreFormSubmission extends SlingAllMethodsServlet implements Serializable {
   private static final long serialVersionUID = 1 L;
   private final transient Logger log = LoggerFactory.getLogger(this.getClass());
   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      List < RequestParameter > listOfRequestParameters = request.getRequestParameterList();
      log.debug("The size of list is " + listOfRequestParameters.size());
      for (int i = 0; i < listOfRequestParameters.size(); i++) {
         RequestParameter requestParameter = listOfRequestParameters.get(i);
         log.debug("is this request parameter a form field?" + requestParameter.isFormField());
         if (!requestParameter.isFormField()) {
            Document attachmentDOC = new Document(requestParameter.getInputStream());
            attachmentDOC.copyToFile(new File(requestParameter.getName()));
         } else {
            log.debug("Not a form field " + requestParameter.getName());
            log.debug(requestParameter.getString());
         }
      }
      response.setStatus(HttpServletResponse.SC_OK);
   }
}
```
