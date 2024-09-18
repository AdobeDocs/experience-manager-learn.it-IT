---
title: Attivare il flusso di lavoro AEM all’invio del modulo HTML5 - Gestire l’invio del modulo
description: Scopri come attivare il flusso di lavoro AEM quando il modulo HTML5 viene inviato e memorizzare i dati inviati nell’archivio.
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
badgeVersions: label="AEM Forms 6.5" before-title="false"
level: Experienced
jira: kt-16215
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 1%

---


# Archivia dati inviati

Il passaggio successivo consiste nell’archiviare i dati inviati nell’archivio dell’autore dell’AEM. Il servlet installato su `/bin/startworkflow` salva i dati inviati.
Un modulo di avvio del flusso di lavoro AEM è configurato per essere attivato ogni volta che viene creata una nuova risorsa di tipo `nt:file` nel nodo &lt;node_to_store_mitted_data>. Questo flusso di lavoro creerà un PDF non interattivo o statico unendo i dati inviati con il modello xdp. Il pdf generato viene quindi assegnato a un utente per la revisione e l’approvazione.

Per memorizzare i dati inviati nel nodo &lt;node_to_store_mitted_data>, si utilizza il servizio OSGi `GetResolver`, che consente di salvare i dati inviati utilizzando l&#39;utente di sistema `fd-service` disponibile in ogni installazione di AEM Forms.

Il nodo in cui sono archiviati i dati inviati può essere configurato utilizzando ConfigMgr come spiegato in [distribuire risorse di esempio](./deploy-assets.md)

```java
package com.aemforms.mobileforms.core.servlets;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.UUID;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import com.aemforms.mobileforms.core.configuration.service.AemServerCredentialsConfigurationService;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import org.apache.sling.api.servlets.SlingAllMethodsServlet;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/startworkflow"})
public class StartWorkflow extends SlingAllMethodsServlet {

    private static Logger logger = LoggerFactory.getLogger(StartWorkflow.class);

    @Reference
    private GetResolver getResolver;
    
    @Reference
    private AemServerCredentialsConfigurationService aemServerCredentialsConfigurationService;
    
    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
    {
            String xmlData = null;
            logger.debug("in start workflow");
            response.setContentType("text/html;charset=UTF-8");
            if (request.getParameter("xmlData") != null)
                {
                    logger.debug("The form was submitted from Acrobat/Reader");
                    xmlData = request.getParameter("xmlData");
                    logger.debug("In start workflow" + xmlData);
                }
                logger.debug("Trying to get resource  "+aemServerCredentialsConfigurationService.getFolderPath());
                Resource r = getResolver.getFormsServiceResolver().getResource(aemServerCredentialsConfigurationService.getFolderPath());
                String responseMessage =null;
                if(r!= null)
                {

                    Session session = r.getResourceResolver().adaptTo(Session.class);
                    logger.debug("Got reosurce pdfsubmissions" + r.getPath());
                    UUID uidName = UUID.randomUUID();
                    Node xmlDataFilesNode = r.adaptTo(Node.class);
                    InputStream is = new ByteArrayInputStream(xmlData.getBytes());
                    Binary binary;
                    try {
                            Node xmlFileNode = xmlDataFilesNode.addNode(uidName.toString(), "nt:file");
                            logger.debug("Added nt file node");
                            Node jcrContent = xmlFileNode.addNode("jcr:content", "nt:resource");
                            logger.debug("Added jcr content");
                            binary = session.getValueFactory().createBinary(is);
                            jcrContent.setProperty("jcr:data", binary);
                            session.save();
                        } catch (RepositoryException e)
                        {
                            throw new RuntimeException(e);
                        }
                    responseMessage = "Your form was successfully submitted";
                }else
                {
                    logger.debug("The resource IS NULL");
                    responseMessage = "Error is processing your submission!!! Please contact the administrator";
                }

        try {
            response.setContentType("text/plain");
            response.getWriter().write(responseMessage);

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

}
```

## Passaggi successivi

[Avvio e flusso di lavoro del flusso di lavoro](./review-workflow.md)

