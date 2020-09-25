---
title: Servizio di precompilazione in Forms adattivo
seo-title: Servizio di precompilazione in Forms adattivo
description: Pre-compilazione di moduli adattivi recuperando dati da origini dati back-end.
seo-description: Pre-compilazione di moduli adattivi recuperando dati da origini dati back-end.
sub-product: forms
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Utilizzo del servizio Prefill in Forms adattivo

È possibile precompilare i campi di un modulo adattivo utilizzando i dati esistenti. Quando un utente apre un modulo, i valori di tali campi vengono precompilati. Esistono diversi modi per precompilare i campi modulo adattivo. In questo articolo verrà analizzata la precompilazione del modulo adattivo utilizzando  servizio di precompilazione AEM Forms.

Per ulteriori informazioni sui vari metodi per precompilare i moduli adattivi, [consultare la presente documentazione](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Per precompilare il modulo adattivo utilizzando il servizio precompila, sarà necessario creare una classe che implementa l&#39;interfaccia DataProvider. Il metodo getPrefillData avrà la logica per creare e restituire i dati che il modulo adattivo utilizzerà per precompilare i campi. In questo metodo, è possibile recuperare i dati da qualsiasi origine e restituire il flusso di input del documento dati. Il codice di esempio seguente recupera le informazioni del profilo utente dell&#39;utente connesso e crea un documento XML il cui flusso di input viene restituito per essere utilizzato dai moduli adattivi.

Nello snippet di codice riportato di seguito abbiamo una classe che implementa l&#39;interfaccia DataProvider. Possiamo accedere all&#39;utente che ha effettuato l&#39;accesso, quindi recuperare le informazioni del profilo dell&#39;utente che ha effettuato l&#39;accesso. Si crea quindi un documento XML con un elemento nodo principale denominato &quot;data&quot; e si aggiungono gli elementi appropriati a questo nodo di dati. Una volta costruito il documento XML, viene restituito il flusso di input del documento XML.

Questa classe viene quindi trasformata in bundle OSGi e distribuita in AEM. Una volta distribuito il bundle, questo servizio di precompilazione sarà disponibile per essere utilizzato come servizio di precompilazione del modulo adattivo.

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

Per testare questa funzionalità sul server, effettuare le seguenti operazioni

* [Scaricare ed estrarre il contenuto del file zip sul computer](assets/prefillservice.zip)
* Assicuratevi che le informazioni sul profilo [dell&#39;](http://localhost:4502/libs/granite/security/content/useradmin) utente che ha effettuato l&#39;accesso siano complete. Questo è un requisito necessario per il funzionamento del campione. Nell&#39;esempio non è presente alcun controllo degli errori per rilevare le proprietà del profilo utente mancanti.
* Distribuzione del bundle tramite la console [AEM Web](http://localhost:4502/system/console/bundles)
* Creazione di moduli adattivi con XSD
* Associate &quot;Custom Aem Form Pre Fill Service&quot; come servizio di precompilazione per il modulo adattivo
* Trascinare gli elementi dello schema sul modulo
* Visualizzare l’anteprima del modulo

>[!NOTE]
>
>Se il modulo adattivo è basato su XSD, assicurarsi che il documento XML restituito dal servizio di precompilazione corrisponda al file XSD su cui si basa il modulo adattivo.
>
>Se il modulo adattivo non è basato su XSD, sarà necessario eseguire il binding manuale dei campi. Ad esempio, per eseguire un binding di un campo modulo adattivo con un elemento nome nei dati XML, utilizzare `/data/fname` il riferimento Binding del campo modulo adattivo.

