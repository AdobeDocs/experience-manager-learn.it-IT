---
title: Rendering di XDP in PDF con diritti di utilizzo
description: Applicare i diritti di utilizzo ai PDF
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 2%

---

# Rendering di XDP in PDF con diritti di utilizzo{#rendering-xdp-into-pdf-with-usage-rights}

Un caso d’uso comune è quello di eseguire il rendering di xdp in PDF e applicare le estensioni di Reader al PDF sottoposto a rendering.

Ad esempio, nel portale Forms di AEM Forms, quando un utente fa clic su XDP, possiamo eseguire il rendering di XDP come PDF e il lettore può estendere il PDF.


Per eseguire questo caso d’uso è necessario effettuare le seguenti operazioni.

* Aggiungi il certificato Estensioni di Reader all’utente &quot;fd-service&quot;. Sono elencati i passaggi per aggiungere le credenziali delle estensioni di Reader [qui](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=it)


* È inoltre possibile fare riferimento al video su [configurazione delle credenziali delle estensioni di Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)


* Crea un servizio OSGi personalizzato che esegue il rendering e applica i diritti di utilizzo. Il codice per eseguire questa operazione è elencato di seguito

## Rendering di XDP e applicazione dei diritti di utilizzo {#render-xdp-and-apply-usage-rights}

* Linea 7: utilizzando il renderingPDFForm di FormsService, generiamo PDF da XDP.

* Righe 8-14: vengono impostati i diritti di utilizzo appropriati. Questi diritti di utilizzo vengono recuperati dalle impostazioni di configurazione OSGi.

* Riga 20 : utilizza resourceresolver associato a fd-service utente del servizio

* Riga 24: il metodo secureDocument di DocumentAssuranceService viene utilizzato per applicare i diritti di utilizzo

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

La schermata seguente mostra le proprietà di configurazione esposte. La maggior parte dei diritti di utilizzo comuni è esposta tramite questa configurazione.

![Proprietà di configurazione](assets/configurationproperties.gif)

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

## Creare un servlet per inviare in streaming il PDF {#create-servlet-to-stream-the-pdf}

Il passaggio successivo consiste nel creare un servlet con un metodo GET per restituire all’utente il reader extended PDF. In questo caso, all’utente viene richiesto di salvare il PDF nel proprio file system. Questo perché il PDF viene riprodotto come Dynamic PDF e i visualizzatori pdf forniti con i browser non gestiscono i PDF dinamici.

Di seguito è riportato il codice del servlet. Trasmettiamo a questo servlet il percorso dell’XDP nell’archivio CRX.

Viene quindi chiamato il metodo renderAndExtendXdp di com.aemformssamples.documentservices.core.DocumentServices.

Il PDF esteso del lettore viene quindi inviato in streaming all’applicazione chiamante

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

Per eseguire il test sul server locale, attieniti alla seguente procedura
1. [Scaricare e installare il bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Scaricare e installare il bundle AEMFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [Scarica il modello di portale personalizzato HTML](assets/render-and-extend-template.zip)
1. [Scarica e importa le risorse relative a questo articolo in AEM utilizzando Gestione pacchetti](assets/renderandextendxdp.zip)
   * Questo pacchetto ha un portale di esempio e un file xdp
1. Aggiungere il certificato Estensioni di Reader all&#39;utente &quot;fd-service&quot;
1. Puntare il browser a [pagina web del portale](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. Fai clic sull’icona pdf per riprodurre xdp come file pdf con diritti di utilizzo applicati.
