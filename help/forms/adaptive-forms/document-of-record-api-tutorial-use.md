---
title: Utilizzo dell’API per generare un documento di record con AEM Forms
description: Genera documento di record (DOR) a livello di programmazione
feature: Moduli adattivi
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# Utilizzo dell’API per generare un documento di record in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Genera documento di record (DOR) a livello di programmazione

Questo articolo illustra l&#39;utilizzo di `com.adobe.aemds.guide.addon.dor.DoRService API` per generare **Documento di record** a livello di programmazione. [Documento di ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) registrazione: versione PDF dei dati acquisiti in modulo adattivo.

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
1. Assicurati di aver installato e avviato il bundle DevelopingWithServiceUser fornito come parte di [Crea articolo utente del servizio](service-user-tutorial-develop.md)
1. [Accedi a configMgr](http://localhost:4502/system/console/configMgr)
1. Ricerca del servizio User Mapper di Apache Sling Service
1. Assicurati di avere la seguente voce _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ nella sezione Mappature dei servizi
1. [Aprire il modulo](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Compila il modulo e fai clic su &quot;Visualizza PDF&quot;
1. Dovresti visualizzare DOR in una nuova scheda nel tuo browser


**Suggerimenti per la risoluzione dei problemi**

Il PDF non viene visualizzato nella nuova scheda del browser:

1. Assicurati di non bloccare i popup nel browser
1. Segui i passaggi descritti in questo [articolo](service-user-tutorial-develop.md)
1. Assicurati che il bundle &#39;DevelopingWithServiceUser&#39; sia in *stato attivo*
1. Assicurati che l&#39;utente di sistema &#39; dati &#39; abbia le autorizzazioni di lettura, modifica e creazione sul nodo seguente `/content/usergenerated/content/aemformsenablement`

