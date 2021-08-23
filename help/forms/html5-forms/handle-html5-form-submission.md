---
title: Gestire l’invio di moduli HTML5
description: Crea handler per l’invio di moduli HTML5
feature: Forms Mobile
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 2%

---


# Gestire l’invio di moduli HTML5

I moduli HTML5 possono essere inviati al servlet ospitato in AEM. I dati inviati sono accessibili nel servlet come flusso di input. Per inviare il modulo HTML5 è necessario aggiungere il pulsante &quot;Invia per HTTP&quot; al modello di modulo utilizzando AEM Forms Designer

## Creare il gestore di invio

È possibile creare un semplice servlet per gestire l’invio del modulo HTML5. I dati inviati possono quindi essere estratti utilizzando il seguente codice. Questo [servlet](assets/html5-submit-handler.zip) è disponibile come parte di questa esercitazione. Installa il [servlet](assets/html5-submit-handler.zip) utilizzando [package manager](http://localhost:4502/crx/packmgr/index.jsp)

Il codice della riga 9 può essere utilizzato per richiamare il processo J2EE. Assicurati di aver configurato la [configurazione Adobe LiveCycle Client SDK](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) se intendi usare il codice per richiamare il processo J2EE.

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

* Tocca l’xdp e fai clic su _Proprietà_->_Avanzate_
* copia http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html e incolla questo nel campo di testo Invia URL
* Fare clic sul pulsante _SaveAndClose_ .

### Aggiungi una voce nei percorsi di esclusione

* Passa a [configMgr](http://localhost:4502/system/console/configMgr).
* Cerca _Filtro CSRF Granite Adobe_
* Aggiungi la seguente voce nella sezione Percorsi esclusi
* _/content/AemFormsSamples/handlehml5formsubmit_
* Salva le modifiche

### Verificare il modulo

* Tocca il modello xdp.
* Fare clic su _Anteprima_->Anteprima come HTML
* Immettere alcuni dati nel modulo e fare clic su Invia
* Dovresti vedere i dati inviati scritti nel file stdout.log del tuo server

### Lettura aggiuntiva

È inoltre consigliato questo [articolo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) sulla generazione di PDF dall’invio di moduli HTML5.




