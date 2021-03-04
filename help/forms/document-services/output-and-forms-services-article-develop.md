---
title: Sviluppo con output e servizi Forms in AEM Forms
seo-title: Sviluppo con output e servizi Forms in AEM Forms
description: Utilizzo dell’API del servizio Output e Forms in AEM Forms
seo-description: Utilizzo dell’API del servizio Output e Forms in AEM Forms
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: Servizio di output
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
topic: Sviluppo
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---


# Sviluppo con output e servizi Forms in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Utilizzo dell’API del servizio Output e Forms in AEM Forms

In questo articolo daremo un&#39;occhiata a quanto segue

* Servizio di output: in genere questo servizio viene utilizzato per unire dati xml con modello xdp o pdf per generare pdf appiattiti
* FormsService - Si tratta di un servizio molto versatile che consente di esportare/importare dati da e in file PDF

Javadoc ufficiale per l&#39;API di AEM Forms è elencato [qui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Il seguente frammento di codice esporta i dati dal file PDF

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

La riga 5 ottiene il blocco di FormsService

La riga 6 esporta i dati xml dal file PDF

**Per testare il pacchetto di esempio sul sistema**

[Scarica e installa il pacchetto utilizzando il gestore di pacchetti AEM](assets/outputandformsservice.zip)




**Dopo aver installato il pacchetto, dovrai inserire i seguenti URL nel filtro CSRF di Adobe Granite.**

1. Segui i passaggi indicati di seguito per inserire i percorsi sopra indicati.
1. [Accedi a configMgr](http://localhost:4502/system/console/configMgr)
1. Ricerca filtro CSRF di Adobe Granite
1. Aggiungi i seguenti 3 percorsi nelle sezioni escluse e salva
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Cerca &quot;filtro Sling Referrer&quot;
1. Selezionare la casella di controllo &quot;Consenti vuoto&quot;. (Questa impostazione deve essere utilizzata solo a scopo di test)
Esistono diversi modi per testare il codice di esempio. La più rapida e semplice è quella di utilizzare l’app Postman. Postman ti consente di effettuare richieste POST al tuo server. Installa l&#39;app Postman sul tuo sistema.
Avvia l’app e immetti il seguente URL per testare l’API dei dati di esportazione

Accertati di aver selezionato &quot;POST&quot; dall&#39;elenco a discesa
http://localhost:4502/content/AemFormsSamples/exportdata.html
Assicurati di specificare &quot;Autorizzazione&quot; come &quot;Autenticazione di base&quot;. Specifica il nome utente e la password del server AEM
Passa alla scheda &quot;Corpo&quot; e specifica i parametri della richiesta come mostrato nell’immagine seguente
![esportazione](assets/postexport.png)
Quindi fai clic sul pulsante Invia

Il pacchetto contiene 3 campioni. Nei paragrafi seguenti viene spiegato quando utilizzare il servizio di output o il servizio Forms, l’url del servizio , i parametri di input previsti da ogni servizio

**Unisci dati e appiattisci output:**

* Utilizzare il servizio di output per unire i dati con il documento xdp o pdf per generare un pdf appiattito
* **URL** POST: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parametri di richiesta -**

   * xdp_or_pdf_file : File xdp o pdf con cui si desidera unire i dati
   * xmlfile: Il file di dati xml che verrà unito con xdp_or_pdf_file
   * saveLocation: Posizione in cui salvare il documento di cui è stato effettuato il rendering nel file system

**Importa dati in file PDF:**
* Utilizzare FormsService per importare dati in file PDF
* **URL POST**  - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parametri di richiesta:**

   * pdffile : File pdf con cui si desidera unire i dati
   * xmlfile: Il file di dati xml che verrà unito al file pdf
   * saveLocation: Posizione in cui salvare il documento di cui è stato effettuato il rendering nel file system. Ad esempio c:\\\outputsample.pdf.

**Esportare dati da file PDF**
* Utilizzare FormsService per esportare dati da file PDF
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parametri di richiesta:**

   * pdffile : Il file pdf da cui si desidera esportare i dati
   * saveLocation: Posizione in cui salvare i dati esportati nel file system

[Puoi importare questa raccolta di postman per testare l’API](assets/document-services-postman-collection.json)

