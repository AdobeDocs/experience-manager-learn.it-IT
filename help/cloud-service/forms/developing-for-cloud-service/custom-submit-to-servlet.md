---
title: Creazione di un gestore di azioni di invio personalizzato
description: Invio di un modulo adattivo a un gestore di invio personalizzato
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Creare un servlet per elaborare i dati inviati

Avvia il progetto di aem-banking in IntelliJ.
Crea un semplice servlet per inviare i dati inviati al file di registro. Assicurati che il codice sia nel progetto di base, come illustrato nella schermata seguente
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## Crea gestore di invio personalizzato

Creare l&#39;azione di invio personalizzata nella cartella `apps/bankingapplication` nello stesso modo in cui si creerebbe nelle [versioni precedenti di AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=it). Ai fini di questa esercitazione, creo una cartella denominata SubmitToAEMServlet nel nodo `apps/bankingapplication` nell&#39;archivio CRX.

Il codice seguente nel file post.POST.jsp inoltra semplicemente la richiesta al servlet installato in /bin/formstutorial. Questo è lo stesso servlet creato nel passaggio precedente

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

Nel progetto AEM in IntelliJ, fare clic con il pulsante destro del mouse sulla cartella `apps/bankingapplication` e selezionare Nuovo | Creare un pacchetto e digitare SubmitToAEMServlet dopo apps.bankingapplication nella finestra di dialogo Nuovo pacchetto. Fai clic con il pulsante destro del mouse sul nodo SubmitToAEMServlet e seleziona archivio | Ottieni il comando per sincronizzare il progetto AEM con l’archivio del server AEM.


## Configurare un modulo adattivo

Ora puoi configurare qualsiasi modulo adattivo da inviare a questo gestore di invio personalizzato denominato **Invia ad AEM Servlet**

## Passaggi successivi

[Registra servlet utilizzando il tipo di risorsa](./registering-servlet-using-resourcetype.md)
