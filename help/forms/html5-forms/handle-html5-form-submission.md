---
title: Gestire l’invio di moduli HTML5
description: Crea gestore di invio modulo di HTML5
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 78
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# Gestire l’invio di moduli HTML5

I moduli HTML5 possono essere inviati a un servlet ospitato nell’AEM. I dati inviati sono accessibili nel servlet come flusso di input. Per inviare il modulo HTML5 è necessario aggiungere &quot;HTTP Submit Button&quot; al modello di modulo tramite AEM Forms Designer

## Creare il gestore di invio

È possibile creare un semplice servlet per gestire l’invio del modulo HTML5. I dati inviati possono quindi essere estratti utilizzando il seguente codice. Questo [servlet](assets/html5-submit-handler.zip) è reso disponibile come parte di questa esercitazione. Installare [servlet](assets/html5-submit-handler.zip) utilizzo [gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)

Il codice della riga 9 può essere utilizzato per richiamare il processo J2EE. Assicurati di aver configurato [Adobe configurazione dell’SDK del client del LiveCycle](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) se si intende utilizzare il codice per richiamare il processo J2EE.

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## Configurare l’URL di invio del modulo HTML5

![submit-url](assets/submit-url.PNG)

* Tocca l’XDP e fai clic su _Proprietà_->_Avanzate_
* copia http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html e incollalo nel campo di testo Invia URL
* Clic _SaveAndClose_ pulsante.

### Aggiungi voce in Exclude Paths (Percorsi di esclusione)

* Accedi a [configMgr](http://localhost:4502/system/console/configMgr).
* Cerca _Filtro CSRF Adobe Granite_
* Aggiungi la seguente voce nella sezione Percorsi esclusi
* _/content/AemFormsSamples/handlehml5formsubmit_
* Salva le modifiche

### Testare il modulo

* Tocca il modello xdp.
* Fai clic su _Anteprima_->Anteprima come HTML
* Immetti alcuni dati nel modulo e fai clic su Invia
* Dovresti visualizzare i dati inviati scritti nel file stdout.log del server

### Letture aggiuntive

Questo [articolo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) durante la generazione di PDF dall’invio del modulo HTML5 è inoltre consigliato.
