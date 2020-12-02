---
title: Generazione del documento di comunicazione interattiva per il canale di stampa tramite il meccanismo di gestione delle cartelle di controllo
seo-title: Generazione del documento di comunicazione interattiva per il canale di stampa tramite il meccanismo di gestione delle cartelle di controllo
description: Usa cartella esaminata per generare documenti per il canale di stampa
seo-description: Usa cartella esaminata per generare documenti per il canale di stampa
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---


# Generazione del documento di comunicazione interattiva per il canale di stampa tramite il meccanismo di gestione delle cartelle di controllo

Dopo aver progettato e testato il documento del canale di stampa, sarà in genere necessario generare il documento effettuando una chiamata REST o generando documenti di stampa utilizzando il meccanismo della cartella di controllo.

In questo articolo viene illustrato come generare documenti per il canale di stampa utilizzando il meccanismo delle cartelle controllato.

Quando rilasciate un file nella cartella esaminata, viene eseguito uno script associato alla cartella esaminata. Questo script è illustrato nell&#39;articolo riportato di seguito.

Il file rilasciato nella cartella esaminata ha la struttura seguente. Il codice genererà istruzioni per tutti i numeri di conto elencati nel documento XML.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

L&#39;elenco di codici riportato di seguito effettua le seguenti operazioni:

Linea 1 - Percorso del documento InteractiveCommunications

Linee 15-20: Ottenere l&#39;elenco dei numeri di account dal documento XML rilasciato nella cartella esaminata

Linee 24-25: Associare PrintChannelService e Print Channel al documento.

Linea 30: Passa il numero di conto come elemento chiave al modello dati modulo.

Linee 32-36: Impostare le opzioni dati per il documento da generare.

Linea 38: Eseguire il rendering del documento.

Righe 39-40 - Salva il documento generato nel file system.

L&#39;endpoint REST del modello dati modulo prevede un ID come parametro di input. questo ID viene mappato su un attributo Request denominato accountnumber come mostrato nella schermata seguente.

![requestAttribute](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**Per eseguire il test sul sistema locale, seguire le istruzioni seguenti:**

* Imposta Tomcat come descritto in questo [articolo.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat ha il file di guerra che genera i dati di esempio.
* Configurare il servizio o l&#39;utente del sistema come descritto in questo [articolo](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Assicurarsi che l&#39;utente di sistema disponga delle autorizzazioni di lettura per il nodo seguente. Per assegnare le autorizzazioni di accesso a [utente admin](https://localhost:4502/useradmin) e cercare &quot;dati&quot; dell&#39;utente di sistema e assegnare le autorizzazioni di lettura per il nodo seguente mediante il tasto di tabulazione sulla scheda delle autorizzazioni
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importate i seguenti pacchetti in AEM utilizzando il gestore pacchetti. Questo pacchetto contiene i seguenti elementi:


* [Esempio di documento di comunicazione interattiva](assets/retirementstatementprint.zip)
* [Script cartella esaminata](assets/printchanneldocumentusingwatchedfolder.zip)
* [Configurazione origine dati](assets/datasource.zip)

* Aprite il file /etc/fd/watchfolder/scripts/PrintPDF.ecma. Assicuratevi che il percorso del documento interattivoCommunicationsDocument nella riga 1 indichi il documento corretto da stampare

* Modifica saveLocation in base alle preferenze sulla riga 2

* Crea file accountnumber.xml con il contenuto seguente

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* Rilasciate il file accountnumber.xml nella cartella C:\RenderPrintChannel\input folder.

* I file PDF generati vengono scritti in saveLocation come specificato nello script ecma.

>[!NOTE]
>
>Se si intende utilizzarlo in un sistema operativo non Windows, passare a
>
>/etc/fd/watchfolder /config/PrintChannelDocument e modificare folderPath come da preferenza

