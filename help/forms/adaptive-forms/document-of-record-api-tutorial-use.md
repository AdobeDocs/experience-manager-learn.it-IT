---
title: Utilizzo dell’API per generare un documento di record con AEM Forms
description: Genera documento di record (DOR) a livello di programmazione
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 1%

---

# Utilizzo dell’API per generare un documento di record in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Genera documento di record (DOR) a livello di programmazione

Questo articolo illustra l&#39;uso del `com.adobe.aemds.guide.addon.dor.DoRService API` per generare **Documento di registrazione** a livello di programmazione. [Documento di registrazione](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) è una versione PDF dei dati acquisiti in Moduli adattivi.

1. Di seguito è riportato lo snippet di codice. La prima riga ottiene il servizio DOR.
1. Imposta le opzioni DoRO.
1. Richiamare il metodo di rendering del servizio DoRS e passare l&#39;oggetto DoROptions al metodo di rendering

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

Per provare questo sul sistema locale, segui i seguenti passaggi

1. [Scaricare e installare le risorse dell’articolo utilizzando il gestore dei pacchetti](assets/dor-with-api.zip)
1. Assicurati di aver installato e avviato il bundle DevelopingWithServiceUser fornito come parte di [Articolo utente Crea servizio](service-user-tutorial-develop.md)
1. [Accedi a configMgr](http://localhost:4502/system/console/configMgr)
1. Ricerca del servizio User Mapper di Apache Sling Service
1. Assicurati di aver inserito la voce seguente _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ nella sezione Service Mappings (Mappature dei servizi)
1. [Aprire il modulo](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Compila il modulo e fai clic su &quot;Visualizza PDF&quot;
1. Dovresti visualizzare DOR in una nuova scheda nel tuo browser


**Suggerimenti per la risoluzione dei problemi**

PDF non viene visualizzato nella nuova scheda del browser:

1. Assicurati di non bloccare i popup nel browser
1. Segui i passaggi descritti in questo [articolo](service-user-tutorial-develop.md)
1. Assicurati che il bundle &#39;DevelopingWithServiceUser&#39; sia in *stato attivo*
1. Assicurati che i dati dell&#39;utente del sistema &#39; abbiano le autorizzazioni di lettura, modifica e creazione per il nodo seguente `/content/usergenerated/content/aemformsenablement`
