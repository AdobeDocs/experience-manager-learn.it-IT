---
title: Sviluppo con Output e Forms Services in  AEM Forms
seo-title: Sviluppo con Output e Forms Services in  AEM Forms
description: Utilizzo di Output e Forms Service API in  AEM Forms
seo-description: Utilizzo di Output e Forms Service API in  AEM Forms
feature: forms-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---


# Rendering di PDF interattivo tramite Forms Services in  AEM Forms

Utilizzo di Forms Service API in  AEM Forms per il rendering di PDF interattivo

In questo articolo osserveremo il seguente servizio

* FormsService - Si tratta di un servizio molto versatile che consente di esportare/importare dati da e in file PDF e di generare anche file PDF interattivi unendo i dati XML in modello xdp

Il javadoc ufficiale per  API AEM Forms è elencato [qui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Lo snippet di codice seguente esegue il rendering di un file pdf interattivo utilizzando l&#39;operazione renderingPDFForm di FormsService. schengen.xdp è un modello utilizzato per unire i dati xml.

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

Linea 7: Generazione di PDF interattivi tramite l&#39;operazione del servizio renderingPDFForm di FormsService

Linea 11: Restituisce il pdf interattivo generato all&#39;applicazione chiamante

**Per testare il pacchetto di esempio sul sistema**
1. [Scaricare e installare il pacchetto di esempio di Document Services tramite la console Web di Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Scaricate e installate il pacchetto utilizzando il gestore pacchetti AEM](assets/downloadinteractivepdffrommobileform.zip)



1. [Login a configMgr](http://localhost:4502/system/console/configMgr)
1. Ricerca  Adobe Filtro Granite CSRF
1. Aggiungi il seguente percorso nelle sezioni escluse e salva
1. /bin/generateinteractivepdf
1. [Aprire il modulo mobile](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Compila un paio di campi, quindi fai clic su ***Scarica e compila ....*** pulsante
1. Il pdf interattivo deve essere scaricato nel sistema locale


Il pacchetto di esempio contiene il profilo personalizzato associato al modulo mobile. Esplora il file [custom toolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) . Questo jsp estrae i dati dal modulo mobile ed effettua una richiesta POST al servlet montato sul percorso ***/bin/generateinteractivepdf*** . Il servlet restituisce il pdf interattivo all’applicazione chiamante. Il codice nella barra degli strumenti personalizzata.jsp quindi scarica il file nel sistema locale


