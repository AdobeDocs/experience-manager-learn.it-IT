---
title: Rendering di Interactive PDF tramite i servizi Forms in AEM Forms
description: Utilizzo dell’API del servizio Forms in AEM Forms per il rendering di PDF interattivi
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 94
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Rendering di Interactive PDF tramite i servizi Forms in AEM Forms

Utilizzo dell’API del servizio Forms in AEM Forms per il rendering di PDF interattivi

In questo articolo esamineremo il seguente servizio

* FormsService: si tratta di un servizio molto versatile che consente di esportare/importare dati da e in file PDF e anche generare PDF interattivi unendo dati xml in un modello XDP

Il funzionario [javadoc per API AEM Forms è elencato qui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Il seguente snippet di codice esegue il rendering del PDF interattivo utilizzando l’operazione renderPDFForm di FormsService. Il file schengen.xdp è un modello utilizzato per unire i dati xml.

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

Riga 1: posizione della cartella che contiene il modello xdp

Line2-4: creare PDFFormRenderOptions e impostarne le proprietà

Riga 7: generare Interactive PDF utilizzando il servizio renderPDFForm di FormsService

Riga 11: restituisce il PDF interattivo generato all’applicazione chiamante

**Per testare il pacchetto di esempio sul sistema**
1. [Scarica e installa DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Scarica e installa il pacchetto di esempio DocumentServices tramite la console Web Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Scaricare e installare il pacchetto utilizzando Gestione pacchetti AEM](assets/downloadinteractivepdffrommobileform.zip)

1. [Accedi a configMgr](http://localhost:4502/system/console/configMgr)
1. Cerca Adobe di filtro CSRF Granite
1. Aggiungi il seguente percorso nelle sezioni escluse e salva
1. /bin/generateinteractivepdf
1. Cerca _Servizio User Mapper di Apache Sling_ e fai clic su per aprire le proprietà
   1. Fai clic su *+* (più) per aggiungere la seguente mappatura del servizio
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Fai clic su Salva
1. [Apri il modulo mobile](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Compila un paio di campi e fai clic su ***Scarica e compila ....*** pulsante
1. Il pdf interattivo deve essere scaricato nel sistema locale


Il pacchetto di esempio contiene il profilo personalizzato associato al modulo mobile. Esplora la [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) file. Questo jsp estrae i dati dal modulo mobile e invia una richiesta POST al servlet installato su ***/bin/generateinteractivepdf*** percorso. Il servlet restituisce il pdf interattivo all’applicazione chiamante. Il codice in customtoolbar.jsp quindi scarica il file nel sistema locale
