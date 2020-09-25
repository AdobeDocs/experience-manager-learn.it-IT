---
title: Sviluppo con Output e Forms Services in  AEM Forms
seo-title: Sviluppo con Output e Forms Services in  AEM Forms
description: Utilizzo di Output e Forms Service API in  AEM Forms
seo-description: Utilizzo di Output e Forms Service API in  AEM Forms
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---


# Sviluppo con Output e Forms Services in  AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Utilizzo di Output e Forms Service API in  AEM Forms

In questo articolo osserveremo quanto segue

* Servizio di output: in genere questo servizio viene utilizzato per unire i dati xml con il modello xdp o con un pdf per generare un pdf appiattito
* FormsService - È un servizio molto versatile che consente di esportare/importare dati da e in file PDF

Il javadoc ufficiale per  API AEM Forms è elencato [qui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Il frammento di codice seguente esporta i dati dal file PDF

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

La riga 1 estrae il file pdf dalla richiesta

Line2 estrae saveLocation dalla richiesta

La riga 5 diventa parte integrante di FormsService

La riga 6 esporta i dati xml dal file PDF

**Per testare il pacchetto di esempio sul sistema**

[Scaricate e installate il pacchetto utilizzando il gestore pacchetti AEM](assets/outputandformsservice.zip)




**Dopo aver installato il pacchetto, dovrete  inserire nell&#39;elenco Consentiti i seguenti URL  filtro Granite CSRF Adobe.**

1. Seguire i passaggi indicati di seguito per  inserire nell&#39;elenco Consentiti i percorsi indicati sopra.
1. [Login a configMgr](http://localhost:4502/system/console/configMgr)
1. Ricerca  Adobe Filtro Granite CSRF
1. Aggiungere i seguenti 3 percorsi nelle sezioni escluse e salvare
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Cerca &quot;Filtro Sling Referrer&quot;
1. Selezionare la casella di controllo &quot;Consenti valori nulli&quot;. (Questa impostazione deve essere utilizzata solo a scopo di test)Esistono diversi modi per testare il codice di esempio. La più rapida e semplice è utilizzare l&#39;app Postman. Postman consente di effettuare richieste POST al server. Installate l&#39;app Postman nel sistema.
Avviate l&#39;app e immettete il seguente URL per verificare l&#39;API dei dati di esportazione

Accertatevi di aver selezionato &quot;POST&quot; dall’elenco a discesahttp://localhost:4502/content/AemFormsSamples/exportdata.htmlAccertatevi di specificare &quot;Autorizzazione&quot; come &quot;Autenticazione di base&quot;. Specificare il nome utente AEM server e la passwordPassare alla scheda &quot;Body&quot; e specificare i parametri di richiesta come mostrato nell&#39;immagine sotto![export](assets/postexport.png)Quindi fare clic sul pulsante Invia

Il pacchetto contiene 3 campioni. I paragrafi seguenti spiegano quando utilizzare il servizio di output o Forms Service, l&#39;url del servizio, i parametri di input previsti da ogni servizio

**Unisci dati e Unisci output:**

* Utilizzare Output Service per unire i dati a un documento xdp o pdf per generare un pdf appiattito
* **URL** POST: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parametri richiesta -**

   * xdp_or_pdf_file : Il file xdp o pdf con cui si desidera unire i dati
   * xmlfile: Il file di dati xml che verrà unito con xdp_or_pdf_file
   * saveLocation: Posizione in cui salvare il documento di cui è stato effettuato il rendering nel file system

**Importa dati in file PDF:**
* Uso di FormsService per importare dati in un file PDF
* **URL** POST - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parametri richiesta:**

   * pdffile : Il file pdf con cui si desidera unire i dati
   * xmlfile: Il file di dati xml che verrà unito al file pdf
   * saveLocation: Posizione in cui salvare il documento di cui è stato effettuato il rendering nel file system. Ad esempio c:\\\outputsample.pdf.

**Esporta dati da file PDF**
* Uso di FormsService per esportare dati da file PDF
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parametri richiesta:**

   * pdffile : Il file pdf da cui si desidera esportare i dati
   * saveLocation: Posizione in cui salvare i dati esportati nel file system

[Potete importare questa raccolta postman per testare l&#39;API](assets/document-services-postman-collection.json)

