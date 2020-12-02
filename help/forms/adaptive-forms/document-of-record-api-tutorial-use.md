---
title: Utilizzo dell'API per generare un documento record con  AEM Forms
seo-title: Utilizzo dell'API per generare un documento record con  AEM Forms
description: Genera documento di registrazione (DOR) a livello di programmazione
seo-description: Utilizzo dell'API per generare un documento record con  AEM Forms
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Utilizzo dell&#39;API per generare il documento record in  AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Genera documento di registrazione (DOR) a livello di programmazione

Questo articolo illustra l&#39;utilizzo di `com.adobe.aemds.guide.addon.dor.DoRService API` per generare **Documento di Record** a livello di programmazione. [Il documento ](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) Registrazioni consente di ottenere una versione PDF dei dati acquisiti nel modulo adattivo.

1. Segue lo snippet di codice. La prima riga riceve il servizio DOR.
1. Impostare le opzioni DoROptions.
1. Richiamare il metodo di rendering del servizio DoRService e passare l&#39;oggetto DoROptions al metodo di rendering

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

Per provare a eseguire questa operazione sul sistema locale, procedere come segue

1. [Scaricate e installate le risorse dell&#39;articolo utilizzando il gestore pacchetti](assets/dor-with-api.zip)
1. Accertatevi di aver installato e avviato il bundle DevelopingWithServiceUser fornito come parte dell&#39; [articolo Create Service User](service-user-tutorial-develop.md)
1. [Login a configMgr](http://localhost:4502/system/console/configMgr)
1. Ricerca di Apache Sling Service User Mapper Service
1. Nella sezione Mappature dei servizi, assicurarsi che la voce seguente sia _DevelopingWithServiceUser.core:getformsresources ceresolver=fd-service_
1. [Aprire il modulo](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Compilare il modulo e fare clic su &#39; Visualizza PDF &#39;
1. Dovresti vedere DOR nella nuova scheda del browser


**Suggerimenti per la risoluzione dei problemi**

Il PDF non viene visualizzato nella nuova scheda del browser:

1. Assicuratevi di non bloccare i pop-up nel browser
1. Seguite i passaggi descritti in questo [articolo](service-user-tutorial-develop.md)
1. Assicurati che il bundle &#39;DevelopingWithServiceUser&#39; sia in *stato attivo*
1. Assicurarsi che i dati dell&#39;utente di sistema &#39; &#39; abbiano le autorizzazioni di lettura, modifica e creazione per il nodo seguente `/content/usergenerated/content/aemformsenablement`

