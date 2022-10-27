---
title: Rendering di Interactive PDF tramite Forms Services in AEM Forms
description: Utilizzo dell’API del servizio Forms in AEM Forms per il rendering di PDF interattivi
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# Rendering di Interactive PDF tramite Forms Services in AEM Forms

Utilizzo dell’API del servizio Forms in AEM Forms per il rendering di PDF interattivi

In questo articolo daremo un&#39;occhiata al seguente servizio

* FormsService - Si tratta di un servizio molto versatile che consente di esportare/importare dati da e in file PDF e di generare pdf interattivi unendo i dati xml nel modello xdp

Il funzionario [javadoc per l’API di AEM Forms è elencato qui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Il frammento di codice seguente esegue il rendering di un file pdf interattivo utilizzando l&#39;operazione renderPDFForm di FormsService. schengen.xdp è un modello che viene utilizzato per unire i dati xml.

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

Linea 1: Posizione della cartella che contiene il modello xdp

Linea2-4: Creare PDFFormRenderOptions e impostarne le proprietà

Linea 7: Generare PDF interattivi utilizzando il funzionamento del servizio renderPDFForm di FormsService

Linea 11: Restituisce il pdf interattivo generato all&#39;applicazione chiamante

**Per testare il pacchetto di esempio sul sistema**
1. [Scarica e installa DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Scarica e installa il pacchetto di esempio DocumentServices utilizzando la console Web Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Scarica e installa il pacchetto utilizzando il gestore di pacchetti AEM](assets/downloadinteractivepdffrommobileform.zip)

1. [Accedi a configMgr](http://localhost:4502/system/console/configMgr)
1. Ricerca filtro CSRF Granite Adobe
1. Aggiungi il seguente percorso nelle sezioni escluse e salva
1. /bin/generateinteractivepdf
1. Cerca _Servizio mappatore utenti del servizio Apache Sling_ e fai clic su per aprire le proprietà
   1. Fai clic sul pulsante *+* icona (più) per aggiungere la seguente mappatura del servizio
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Fai clic su &#39; Salva &#39;
1. [Aprire il modulo mobile](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Compila un paio di campi e fai clic sul pulsante ***Scarica e compila ....*** pulsante
1. Il pdf interattivo deve essere scaricato nel sistema locale


Il pacchetto di esempio contiene il profilo personalizzato associato al modulo mobile. Esplorare [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) file. Questo jsp estrae i dati dal modulo mobile ed effettua una richiesta POST al servlet montato su ***/bin/generateinteractivepdf*** percorso. Il servlet restituisce il pdf interattivo all&#39;applicazione chiamante. Il codice in customtoolbar.jsp quindi scarica il file nel sistema locale
