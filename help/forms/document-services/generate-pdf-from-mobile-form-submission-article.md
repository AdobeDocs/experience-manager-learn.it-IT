---
title: Genera PDF dall’invio di un modulo HTM5
description: Genera PDF dall’invio di un modulo mobile
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
duration: 180
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# Genera PDF dall’invio di un modulo HTM5 {#generate-pdf-from-htm-form-submission}

Questo articolo illustra i passaggi necessari per generare pdf da un modulo HTML5 (aka Mobile Forms). Questa demo illustra inoltre i passaggi necessari per aggiungere un’immagine al modulo HTML5 e unirla al pdf finale.


Per unire i dati inviati nel modello xdp, procediamo come segue

Scrivi un servlet per gestire l’invio del modulo HTML5

* All’interno di questo servlet ottieni i dati inviati
* Unisci questi dati con il modello xdp per generare il PDF
* Trasmetti il PDF all’applicazione chiamante

Di seguito è riportato il codice del servlet che estrae i dati inviati dalla richiesta. Chiama quindi il metodo documentServices .mobileFormToPDF personalizzato per ottenere il PDF.

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

Per aggiungere un’immagine al modulo mobile e visualizzarla nel pdf, abbiamo utilizzato quanto segue

Modello XDP: nel modello xdp abbiamo aggiunto un campo immagine e un pulsante denominati btnAddImage. Il codice seguente gestisce l’evento click di btnAddImage nel nostro profilo personalizzato. Come puoi vedere, attiviamo l’evento click file1. Per eseguire questo caso d’uso non è necessaria alcuna codifica nell’XDP

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Profilo personalizzato](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). L’utilizzo di un profilo personalizzato semplifica la manipolazione degli oggetti DOM HTML del modulo mobile. Un elemento file nascosto viene aggiunto a HTML.jsp. Quando l’utente fa clic su &quot;Aggiungi foto&quot;, viene attivato l’evento clic dell’elemento file. Questo consente all&#39;utente di sfogliare e selezionare la fotografia da allegare. Viene quindi utilizzato l’oggetto FileReader javascript per ottenere la stringa con codifica base64 dell’immagine. La stringa immagine base64 viene memorizzata nel campo di testo del modulo. Quando il modulo viene inviato, questo valore viene estratto e inserito nell&#39;elemento img dell&#39;XML. Questo XML viene quindi utilizzato per l’unione con xdp per generare il pdf finale.

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

Il codice riportato sopra viene eseguito quando si attiva l’evento click dell’elemento file. Riga 5 estraiamo il contenuto del file caricato come stringa base64 e lo memorizziamo nel campo di testo. Questo valore viene quindi estratto quando il modulo viene inviato al nostro servlet.

Quindi configuriamo le seguenti proprietà (avanzate) del nostro modulo mobile in AEM

* Invia URL: http://localhost:4502/bin/handlemobileformsubmission. Questo è il nostro servlet che unirà i dati inviati con il modello xdp
* Profilo rendering HTML: accertati di selezionare &quot;AddImageToMobileForm&quot;. Questo attiverà il codice per aggiungere un’immagine al modulo.

Per testare questa funzionalità sul tuo server, segui i seguenti passaggi:

* [Distribuire il bundle AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Distribuire il bundle Developing with Service User](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scarica e installa il pacchetto associato a questo articolo.](assets/pdf-from-mobile-form-submission.zip)

* Assicurati che i profili URL di invio e Rendering HTML siano impostati correttamente visualizzando la pagina delle proprietà di  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Visualizzare l’anteprima di XDP come HTML](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Aggiungi un’immagine al modulo e invia. Dovresti riprendere PDF con l&#39;immagine.
