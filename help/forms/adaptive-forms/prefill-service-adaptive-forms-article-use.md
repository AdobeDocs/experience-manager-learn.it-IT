---
title: Servizio di precompilazione nei moduli adattivi
seo-title: Servizio di precompilazione nei moduli adattivi
description: Precompilare i moduli adattivi recuperando i dati dalle origini dati di backend.
seo-description: Precompilare i moduli adattivi recuperando i dati dalle origini dati di backend.
sub-product: forms
feature: Moduli adattivi
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: Sviluppo
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 1%

---


# Utilizzo del servizio Prefill nei moduli adattivi

È possibile precompilare i campi di un modulo adattivo utilizzando i dati esistenti. Quando un utente apre un modulo, i valori relativi a tali campi vengono precompilati. Esistono diversi modi per precompilare i campi dei moduli adattivi. In questo articolo, cercheremo di precompilare il modulo adattivo utilizzando il servizio di precompilazione di AEM Forms.

Per ulteriori informazioni sui vari metodi per precompilare i moduli adattivi, [segui questa documentazione](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

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
* Assicurati che le informazioni sul profilo dell&#39;utente [connesso](http://localhost:4502/libs/granite/security/content/useradmin) siano state compilate completamente. Questo è necessario per il funzionamento del campione. Nell&#39;esempio non è presente alcun controllo degli errori per rilevare le proprietà mancanti del profilo utente.
* Distribuisci il bundle utilizzando la [console web AEM](http://localhost:4502/system/console/bundles)
* Creare un modulo adattivo utilizzando XSD
* Associa &quot;Servizio di pre-compilazione personalizzato del modulo Aem&quot; come servizio di precompilazione per il modulo adattivo
* Trascina gli elementi dello schema sul modulo
* Visualizzare l’anteprima del modulo

>[!NOTE]
>
>Se il modulo adattivo è basato su XSD, assicurati che il documento XML restituito dal servizio di precompilazione corrisponda a XSD su cui si basa il modulo adattivo.
>
>Se il modulo adattivo non è basato su XSD, sarà necessario eseguire il binding manuale dei campi. Ad esempio, per associare un campo modulo adattivo a un elemento nome nei dati XML, utilizzare `/data/fname` nel riferimento Bind del campo modulo adattivo.

