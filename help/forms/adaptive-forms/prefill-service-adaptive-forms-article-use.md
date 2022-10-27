---
title: Servizio di precompilazione in Forms adattivo
description: Precompilare i moduli adattivi recuperando i dati dalle origini dati di backend.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# Utilizzo del servizio Prefill in Adaptive Forms

È possibile precompilare i campi di un modulo adattivo utilizzando i dati esistenti. Quando un utente apre un modulo, i valori relativi a tali campi vengono precompilati. Esistono diversi modi per precompilare i campi dei moduli adattivi. In questo articolo, esamineremo la precompilazione del modulo adattivo utilizzando il servizio di precompilazione AEM Forms.

Per ulteriori informazioni sui vari metodi per precompilare i moduli adattivi, [seguire questa documentazione](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Per precompilare il modulo adattivo utilizzando il servizio di precompilazione, è necessario creare una classe che implementi l&#39;interfaccia DataProvider. Il metodo getPrefillData avrà la logica per generare e restituire i dati che il modulo adattivo utilizzerà per precompilare i campi. In questo metodo, è possibile recuperare i dati da qualsiasi origine e restituire il flusso di input del documento di dati. Il codice di esempio seguente recupera le informazioni del profilo utente dell’utente connesso e crea un documento XML il cui flusso di input viene restituito per essere utilizzato dai moduli adattivi.

Nello snippet di codice sottostante abbiamo una classe che implementa l&#39;interfaccia DataProvider. Otteniamo l&#39;accesso all&#39;utente connesso, quindi recuperiamo le informazioni sul profilo dell&#39;utente registrato. Quindi creiamo un documento XML con un elemento del nodo principale denominato &quot;data&quot; e aggiungiamo gli elementi appropriati a questo nodo di dati. Una volta costruito il documento XML, viene restituito il flusso di input del documento XML.

Questa classe viene quindi trasformata in bundle OSGi e distribuita in AEM. Una volta distribuito il bundle, questo servizio di precompilazione è disponibile per essere utilizzato come servizio di precompilazione del modulo adattivo.

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

Per testare questa funzionalità sul server, esegui le seguenti operazioni

* [Scaricare ed estrarre il contenuto del file zip sul computer](assets/prefillservice.zip)
* Assicurati di aver effettuato l&#39;accesso [profilo utente](http://localhost:4502/libs/granite/security/content/useradmin) le informazioni sono compilate completamente. Questo è necessario per il funzionamento del campione. Nell&#39;esempio non è presente alcun controllo degli errori per rilevare le proprietà mancanti del profilo utente.
* Distribuisci il bundle utilizzando il [Console web AEM](http://localhost:4502/system/console/bundles)
* Creare un modulo adattivo utilizzando XSD
* Associa &quot;Servizio di pre-compilazione personalizzato del modulo Aem&quot; come servizio di precompilazione per il modulo adattivo
* Trascina gli elementi dello schema sul modulo
* Visualizzare l’anteprima del modulo

>[!NOTE]
>
>Se il modulo adattivo è basato su XSD, assicurati che il documento XML restituito dal servizio di precompilazione corrisponda a XSD su cui si basa il modulo adattivo.
>
>Se il modulo adattivo non è basato su XSD, sarà necessario eseguire il binding manuale dei campi. Ad esempio, per associare un campo modulo adattivo a un elemento nome nei dati XML che si utilizza `/data/fname`  nel riferimento Bind del campo modulo adattivo.
