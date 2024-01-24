---
title: Sviluppo con servizi di output e Forms in AEM Forms
description: Utilizzo dell’API di servizio Output e Forms in AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
duration: 138
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---

# Sviluppo con servizi di output e Forms in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Utilizzo dell’API di servizio Output e Forms in AEM Forms

In questo articolo prenderemo in considerazione quanto segue

* Servizio di output: in genere questo servizio viene utilizzato per unire i dati xml con un modello xdp o un PDF per generare un PDF appiattito. Per ulteriori informazioni, consulta questa [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) per il servizio di output.
* FormsService: si tratta di un servizio molto versatile che consente di esportare/importare dati da e in file PDF. Per ulteriori informazioni, consulta questa [javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) per il servizio Forms.


Il frammento di codice seguente esporta dati da file PDF

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

La riga 1 estrae il file PDF dalla richiesta

Line2 estrae il file saveLocation dalla richiesta

La riga 5 acquisisce il blocco di FormsService

La riga 6 esporta i dati xml dal file PDF

**Per testare il pacchetto di esempio sul sistema**

[Scaricare e installare il pacchetto utilizzando Gestione pacchetti AEM](assets/outputandformsservice.zip)




**Dopo aver installato il pacchetto, dovrai inserire nell&#39;elenco Consentiti i seguenti URL in Adobe Granite CSRF Filter.**

1. Segui i passaggi indicati di seguito per inserire nell&#39;elenco Consentiti i percorsi menzionati in precedenza.
1. [Accedi a configMgr](http://localhost:4502/system/console/configMgr)
1. Cerca Adobe di filtro CSRF Granite
1. Aggiungi i seguenti 3 percorsi nelle sezioni escluse e salva
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Cerca &quot;Sling Referrer filter&quot; (Filtro referrer Sling)
1. Selezionare la casella di controllo Consenti vuoto. (Questa impostazione deve essere utilizzata solo a scopo di test) Esistono diversi modi per testare il codice di esempio. Il metodo più rapido e semplice consiste nell’utilizzare l’app Postman. Postman consente di effettuare richieste POST al server. Installa l’app Postman sul sistema.
Avvia l’app e immetti il seguente URL per testare l’API di esportazione dei dati

Assicurarsi di aver selezionato &quot;POST&quot; dall&#39;elenco a discesa http://localhost:4502/content/AemFormsSamples/exportdata.html Assicurarsi di specificare &quot;Autorizzazione&quot; come &quot;Autenticazione di base&quot;. Specifica il nome utente e la password del server AEM Passa alla scheda &quot;Corpo&quot; e specifica i parametri di richiesta come mostrato nell’immagine seguente
![esportare](assets/postexport.png)
Quindi fare clic sul pulsante Invia

La confezione contiene 3 campioni. Nei paragrafi seguenti viene illustrato quando utilizzare il servizio di output o il servizio Forms, l&#39;URL del servizio, i parametri di input previsti da ciascun servizio

## Unisci dati e appiana output

* Utilizza il servizio di output per unire i dati con un documento xdp o pdf per generare un PDF appiattito
* **URL POST**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parametri richiesta -**

   * **xdp_or_pdf_file** : il file xdp o pdf con cui vuoi unire i dati
   * **xmlfile**: file di dati xml unito con xdp_or_pdf_file
   * **saveLocation**: percorso in cui salvare il documento sottoposto a rendering nel file system. Ad esempio c:\\documents\\sample.pdf

### Importa dati in file PDF

* Utilizzare FormsService per importare dati in un file PDF
* **URL POST** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parametri richiesta:**

   * **pdfile** : file pdf con cui unire i dati
   * **xmlfile**: file di dati xml unito al file pdf
   * **saveLocation**: percorso in cui salvare il documento sottoposto a rendering nel file system. Esempio `c:\\outputsample.pdf`.

**Esporta dati da file PDF**
* Utilizzare FormsService per esportare dati da un file PDF
* **UR POST** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parametri richiesta:**

   * **pdfile** : file pdf da cui esportare i dati
   * **saveLocation**: percorso in cui salvare i dati esportati nel file system. Ad esempio c:\\documents\\export_data.xml

[Puoi importare questa raccolta postman per testare l’API](assets/document-services-postman-collection.json)
