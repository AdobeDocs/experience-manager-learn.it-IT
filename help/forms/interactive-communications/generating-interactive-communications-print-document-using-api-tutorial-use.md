---
title: Generazione di documenti di comunicazione interattiva per il canale di stampa mediante il meccanismo della cartella di controllo
description: Usa cartella controllata per generare documenti del canale di stampa
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# Generazione di documenti di comunicazione interattiva per il canale di stampa mediante il meccanismo della cartella di controllo

Dopo aver progettato e testato il documento del canale di stampa, in genere è necessario generare il documento effettuando una chiamata REST o generare documenti di stampa utilizzando il meccanismo della cartella di controllo.

Questo articolo spiega il caso d’uso della generazione di documenti del canale di stampa utilizzando il meccanismo delle cartelle controllate.

Quando rilasci un file nella cartella controllata, viene eseguito uno script associato alla cartella controllata. Questo script è spiegato nell’articolo seguente.

Il file rilasciato nella cartella controllata ha la seguente struttura. Il codice genera istruzioni per tutti i numeri di conto elencati nel documento XML.

&lt;numeri account>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

L’elenco di codici riportato di seguito effettua le seguenti operazioni:

Riga 1: percorso di InteractiveCommunicationsDocument

Righe 15-20: consente di ottenere l&#39;elenco dei numeri di conto dal documento XML rilasciato nella cartella controllata

Righe 24 -25: ottenere il servizio PrintChannelService e il canale di stampa associati al documento.

Riga 30: passa il numero di account come elemento chiave al modello dati del modulo.

Righe 32-36: impostare le opzioni dati per il documento da generare.

Riga 38: eseguire il rendering del documento.

Righe 39-40 - Salva il documento generato nel file system.

L’endpoint REST del modello dati modulo prevede un ID come parametro di input. questo id è mappato su un attributo di richiesta denominato accountnumber come mostrato nella schermata seguente.

![requestattribute](assets/requestattributeprintchannel.gif)

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


**Per eseguire il test nel sistema locale, seguire le istruzioni seguenti:**

* Configurare Tomcat come descritto in questo articolo di [.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat ha il file .war che genera i dati di esempio.
* Configurare il servizio come utente di sistema come descritto in questo [articolo](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Verificare che l&#39;utente del sistema disponga delle autorizzazioni di lettura per il nodo seguente. Per concedere le autorizzazioni di accesso a [user admin](https://localhost:4502/useradmin) e cercare i &quot;dati&quot; dell&#39;utente di sistema e concedere le autorizzazioni di lettura per il nodo seguente, toccare la scheda delle autorizzazioni
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importa i seguenti pacchetti in AEM utilizzando Gestione pacchetti. Questo pacchetto contiene quanto segue:


* [Esempio di documento di comunicazione interattiva](assets/retirementstatementprint.zip)
* [Script cartella controllata](assets/printchanneldocumentusingwatchedfolder.zip)
* [Configurazione origine dati](assets/datasource.zip)

* Apri il file /etc/fd/watchfolder/scripts/PrintPDF.ecma. Verificare che il percorso del documento di comunicazione interattivo nella riga 1 punti al documento corretto da stampare

* Modificare il valore di saveLocation in base alla preferenza nella riga 2

* Crea il file accountnumbers.xml con il seguente contenuto

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


* Trascina il file accountnumbers.xml nella cartella C:\RenderPrintChannel\input.

* I file PDF generati vengono scritti in saveLocation come specificato nello script ecma.

>[!NOTE]
>
>Se si prevede di utilizzarlo in un sistema operativo non Windows, passare a
>
>/etc/fd/watchfolder /config/PrintChannelDocument e modifica il folderPath in base alle tue preferenze
