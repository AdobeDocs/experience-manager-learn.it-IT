---
title: Utilizzo dell’API per generare un documento di record con AEM Forms
description: Genera documento di record (DOR) a livello di programmazione
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
duration: 67
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# Utilizzo dell’API per generare un documento di record in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Genera documento di record (DOR) a livello di programmazione

In questo articolo viene illustrato l&#39;utilizzo di `com.adobe.aemds.guide.addon.dor.DoRService API` per generare **Documento di record** a livello di programmazione. [Il documento record](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html?lang=it) è una versione PDF dei dati acquisiti in un modulo adattivo.

1. Di seguito è riportato lo snippet di codice. La prima riga ottiene il servizio DOR.
1. Impostare le opzioni DoRO.
1. Richiama il metodo di rendering del servizio DoRS e passa l&#39;oggetto DoROptions al metodo di rendering

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
```

Per provare questa operazione sul sistema locale, attenersi alla seguente procedura

1. [Scaricare e installare le risorse dell’articolo tramite Gestione pacchetti](assets/dor-with-api.zip)
1. Assicurati di aver installato e avviato il bundle DevelopingWithServiceUser fornito come parte dell&#39;[articolo Crea utente del servizio](service-user-tutorial-develop.md)
1. [Accesso a configMgr](http://localhost:4502/system/console/configMgr)
1. Cerca servizio User Mapper del servizio Apache Sling
1. Assicurati di avere la seguente voce _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ nella sezione Mappature servizio
1. [Apri il modulo](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Compila il modulo e fai clic su &quot;Visualizza PDF&quot;
1. Dovresti visualizzare DOR in una nuova scheda nel browser


**Suggerimenti per la risoluzione dei problemi**

PDF non viene visualizzato nella nuova scheda del browser:

1. Assicurati di non bloccare i popup nel browser
1. Assicurati di avviare il server AEM come amministratore (almeno su Windows)
1. Assicurati che il bundle &#39;DevelopingWithServiceUser&#39; sia in *stato attivo*
1. [Assicurarsi che l&#39;utente di sistema](http://localhost:4502/useradmin) &#39; fd-service&#39; disponga delle autorizzazioni di lettura, modifica e creazione per il seguente nodo `/content/usergenerated/content/aemformsenablement`
