---
title: Servizio di precompilazione in Forms adattivo
description: Precompilare i moduli adattivi recuperando i dati dalle origini dati di backend.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: 381812397fa7d15f6ee34ef85ddf0aa0acc0af42
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Utilizzo del servizio Prefill in Adaptive Forms

È possibile precompilare i campi di un modulo adattivo utilizzando i dati esistenti. Quando un utente apre un modulo, i valori relativi a tali campi vengono precompilati. Esistono diversi modi per precompilare i campi dei moduli adattivi. In questo articolo, esamineremo la precompilazione del modulo adattivo utilizzando il servizio di precompilazione AEM Forms.

Per ulteriori informazioni sui vari metodi per precompilare i moduli adattivi, [seguire questa documentazione](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Per precompilare un modulo adattivo utilizzando il servizio di precompilazione, è necessario creare una classe che implementi il `com.adobe.forms.common.service.DataXMLProvider` interfaccia. Il metodo `getDataXMLForDataRef` avrà la logica per generare e restituire i dati che il modulo adattivo utilizzerà per precompilare i campi. In questo metodo, è possibile recuperare i dati da qualsiasi origine e restituire il flusso di input del documento di dati. Il codice di esempio seguente recupera le informazioni sul profilo utente dell&#39;utente connesso e crea un documento XML il cui flusso di input viene restituito per essere utilizzato dai moduli adattivi.

Nello snippet di codice sottostante abbiamo una classe che implementa l&#39;interfaccia DataXMLProvider. Otteniamo l&#39;accesso all&#39;utente connesso, quindi recuperiamo le informazioni sul profilo dell&#39;utente registrato. Quindi creiamo un documento XML con un elemento del nodo principale denominato &quot;data&quot; e aggiungiamo gli elementi appropriati a questo nodo di dati. Una volta costruito il documento XML, viene restituito il flusso di input del documento XML.

Questa classe viene quindi trasformata in bundle OSGi e distribuita in AEM. Una volta distribuito il bundle, questo servizio di precompilazione è disponibile per essere utilizzato come servizio di precompilazione del modulo adattivo.

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

Per testare questa funzionalità sul server, esegui le seguenti operazioni

* Assicurati di aver effettuato l&#39;accesso [profilo utente](http://localhost:4502/security/users.html) le informazioni sono compilate. L’esempio cerca le proprietà FirstName, LastName ed Email dell’utente connesso.
* [Scaricare ed estrarre il contenuto del file zip sul computer](assets/prefillservice.zip)
* Distribuisci il bundle prefill.core-1.0.0-SNAPSHOT utilizzando [Console web AEM](http://localhost:4502/system/console/bundles)
* Importare il modulo adattivo utilizzando il comando Crea | Caricamento di file da [Sezione FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Assicurati che [modulo](http://localhost:4502/editor.html/content/forms/af/prefill.html) utilizza **&quot;Servizio di precompilazione AEM Forms personalizzato&quot;** come servizio di precompilazione. Questo può essere verificato dalle proprietà di configurazione del **Contenitore modulo** sezione .
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). Dovresti visualizzare il modulo con i valori corretti.

>[!NOTE]
>
>Se hai abilitato il debug per com.aem.prefill.core.PrefillAdaptiveForm, il file di dati xml generato verrà scritto nella cartella di installazione del server AEM.

