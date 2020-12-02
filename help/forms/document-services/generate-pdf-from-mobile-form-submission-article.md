---
title: Genera PDF da invio modulo HTML5
seo-title: Genera PDF da invio modulo HTML5
description: Genera PDF dall'invio del modulo mobile
seo-description: Genera PDF dall'invio del modulo mobile
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Genera PDF da invio modulo HTML5 {#generate-pdf-from-htm-form-submission}

Questo articolo illustra i passaggi necessari per generare un pdf da un invio di moduli HTML5 (alias Forms Mobile). Questa demo spiega anche i passaggi necessari per aggiungere un’immagine al modulo HTML5 e unire l’immagine al PDF finale.

Per una dimostrazione live di questa funzionalità, visitare il [server di esempio](https://forms.enablementadobe.com/content/samples/samples.html?query=0) e cercare &quot;Modulo mobile in PDF&quot;.

Per unire i dati inviati nel modello xdp, eseguire le operazioni seguenti

Scrivere un servlet per gestire l’invio del modulo HTML5

* All&#39;interno di questo servlet prendere in considerazione i dati inviati
* Unisci questi dati con il modello xdp per generare pdf
* Eseguire nuovamente il streaming del pdf nell&#39;applicazione chiamante

Di seguito è riportato il codice servlet che estrae i dati inviati dalla richiesta. Viene quindi chiamato il metodo personalizzato documentServices .mobileFormToPDF per ottenere il pdf.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
   response.setContentType("application/pdf");
   response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
   response.setContentLength((int) fileInputStream.available());
   OutputStream responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    responseOutputStream.write(bytes);
   }
   responseOutputStream.flush();
   responseOutputStream.close();

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

Per aggiungere un’immagine al modulo per dispositivi mobili e visualizzarla nel PDF è stata utilizzata la seguente

Modello XDP - Nel modello xdp abbiamo aggiunto un campo immagine e un pulsante denominato btnAddImage. Il codice seguente gestisce l’evento click del btnAddImage nel nostro profilo personalizzato. Come potete vedere, attiviamo l&#39;evento click file1. Non è necessaria alcuna codifica in xdp per eseguire questo esempio di utilizzo

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Profilo](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles) personalizzato. L&#39;utilizzo di un profilo personalizzato facilita la manipolazione degli oggetti DOM HTML del modulo mobile. Un elemento file nascosto viene aggiunto a HTML.jsp. Quando l&#39;utente fa clic su &quot;Add Your Photo&quot;, viene attivato l&#39;evento click dell&#39;elemento file. Questo consente all&#39;utente di sfogliare e selezionare la foto da allegare. Quindi si utilizza l&#39;oggetto FileReader javascript per ottenere la stringa codificata base64 dell&#39;immagine. La stringa di immagine base64 è memorizzata nel campo di testo del modulo. Quando il modulo viene inviato, estraiamo questo valore e lo inseriamo nell’elemento img del codice XML. Questo XML viene quindi utilizzato per unire con il file xdp per generare il pdf finale.

Il profilo personalizzato utilizzato per questo articolo è stato reso disponibile come parte delle risorse di questo articolo.

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

Il codice riportato sopra viene eseguito quando si attiva l&#39;evento click dell&#39;elemento file. Alla riga 5 estraiamo il contenuto del file caricato come stringa base64 e lo archiviiamo nel campo di testo. Questo valore viene quindi estratto quando il modulo viene inviato al nostro servlet.

Quindi configuriamo le seguenti proprietà (avanzate) del modulo mobile in AEM

* Invia URL - http://localhost:4502/bin/handlemobileformsubmission. Questo è il nostro servlet che unirà i dati inviati con il modello xdp
* Profilo di rendering HTML - Accertatevi di selezionare &quot;AddImageToMobileForm&quot;. Questo attiverà il codice per aggiungere un&#39;immagine al modulo.

Per testare questa funzionalità sul vostro server, attenetevi alla seguente procedura:

* [Distribuzione del bundle AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Distribuzione del bundle Sviluppo con il servizio utente](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scaricate e installate il pacchetto associato a questo articolo.](assets/pdf-from-mobile-form-submission.zip)

* Assicuratevi che il profilo di invio URL e rendering HTML sia impostato correttamente visualizzando la pagina delle proprietà della [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Anteprima XDP come HTML](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Aggiungere un&#39;immagine al modulo e inviarla. Dovresti recuperare il PDF con l&#39;immagine al suo interno.

