---
title: Rendering di XDP in PDF con diritti di utilizzo
description: Applica diritti di utilizzo a pdf
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---

# Rendering di XDP in PDF con diritti di utilizzo{#rendering-xdp-into-pdf-with-usage-rights}

Un caso d’uso comune è quello di eseguire il rendering di xdp in PDF e applicare le estensioni di Reader al PDF renderizzato.

Ad esempio, nel portale moduli di AEM Forms, quando un utente fa clic su XDP, possiamo eseguire il rendering XDP come PDF e il lettore estende il PDF.


Per eseguire questo caso d’uso, è necessario effettuare le seguenti operazioni.

* Aggiungi il certificato Reader estensioni all’utente &quot;fd-service&quot;. Sono elencati i passaggi per aggiungere le credenziali delle estensioni di Reader [qui](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=en)


* Puoi anche fare riferimento al video su [configurazione delle credenziali delle estensioni del Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)


* Crea un servizio OSGi personalizzato per il rendering e l’applicazione dei diritti di utilizzo. Il codice per eseguire questa operazione è elencato di seguito

## Rendering XDP e applicazione dei diritti di utilizzo {#render-xdp-and-apply-usage-rights}

* Linea 7: Utilizzando il renderPDFForm di FormsService si genera PDF dall’XDP.

* Linee 8-14: Vengono impostati i diritti di utilizzo appropriati. Questi diritti di utilizzo vengono recuperati dalle impostazioni di configurazione OSGi.

* Linea 20 : Utilizza il risolutore risorse associato al servizio fd-service utente

* Linea 24: Il metodo secureDocument di DocumentAssuranceService viene utilizzato per applicare i diritti di utilizzo

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

La schermata seguente mostra le proprietà di configurazione esposte. La maggior parte dei diritti di utilizzo comuni viene esposta tramite questa configurazione.

![](assets/configurationproperties.gif)

Il codice seguente mostra il codice utilizzato per creare le impostazioni di configurazione OSGi

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## Crea servlet per lo streaming del PDF {#create-servlet-to-stream-the-pdf}

Il passaggio successivo consiste nel creare un servlet con un metodo GET per restituire all’utente l’estensione di PDF del lettore. In questo caso, all’utente verrà richiesto di salvare PDF nel file system. Questo perché PDF viene rappresentato come PDF dinamico e i visualizzatori pdf che sono dotati dei browser non gestiscono i pdf dinamici.

Di seguito è riportato il codice del servlet. Passiamo il percorso dell&#39;XDP nell&#39;archivio CRX a questo servlet.

Chiamiamo quindi il metodo renderAndExtendXdp di com.aemformssamples.documentservices.core.DocumentServices.

Il lettore esteso PDF viene quindi inviato in streaming all&#39;applicazione chiamante

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

Per eseguire il test sul server locale, segui i seguenti passaggi
1. [Scarica e installa il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Scarica e installa il bundle AEMFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [Scarica l’HTML del modello di portale personalizzato](assets/render-and-extend-template.zip)
1. [Scarica e importa in AEM le risorse correlate a questo articolo utilizzando il gestore dei pacchetti](assets/renderandextendxdp.zip)
   * Questo pacchetto contiene il portale di esempio e il file xdp
1. Aggiungi il certificato di estensioni di Reader all’utente &quot;fd-service&quot;
1. Posiziona il browser su [pagina web del portale](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. Fai clic sull’icona pdf per eseguire il rendering dell’xdp come file pdf con diritti di utilizzo applicati.
