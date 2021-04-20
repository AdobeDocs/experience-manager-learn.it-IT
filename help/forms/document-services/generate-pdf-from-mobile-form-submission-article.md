---
title: Genera PDF dall’invio di moduli HTML5
seo-title: Genera PDF da invio modulo HTML5
description: Genera PDF dall’invio di moduli per dispositivi mobili
seo-description: Genera PDF dall’invio di moduli per dispositivi mobili
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---


# Genera PDF da invio modulo HTML5 {#generate-pdf-from-htm-form-submission}

Questo articolo illustra i passaggi necessari per generare pdf da un modulo HTML5 (aka Mobile Forms). Questa demo illustra anche i passaggi necessari per aggiungere un’immagine al modulo HTML5 e unire l’immagine al pdf finale.

Per visualizzare una dimostrazione in tempo reale di questa funzionalità, visita il [server di esempio](https://forms.enablementadobe.com/content/samples/samples.html?query=0) e cerca &quot;Modulo mobile a PDF&quot;.

Per unire i dati inviati nel modello xdp, procedere come segue

Scrivere un servlet per gestire l’invio del modulo HTML5

* All’interno di questo servlet ottenere informazioni sui dati inviati
* Unisci questi dati con il modello xdp per generare pdf
* Riporta il pdf all&#39;applicazione chiamante

Di seguito è riportato il codice del servlet che estrae i dati inviati dalla richiesta. Chiama quindi il metodo personalizzato documentServices .mobileFormToPDF per ottenere il pdf.

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

Per aggiungere un’immagine al modulo mobile e visualizzarla nel pdf, si utilizza quanto segue

Modello XDP: nel modello xdp abbiamo aggiunto un campo immagine e un pulsante denominato btnAddImage. Il codice seguente gestisce l&#39;evento click del btnAddImage nel nostro profilo personalizzato. Come puoi vedere, attiviamo l’evento click file1. Per eseguire questo caso d&#39;uso non è necessaria alcuna codifica nell&#39;xdp

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Profilo personalizzato](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). L’utilizzo di un profilo personalizzato facilita la manipolazione degli oggetti DOM HTML del modulo mobile. Un elemento di file nascosto viene aggiunto al file HTML.jsp. Quando l&#39;utente fa clic su &quot;Aggiungi foto&quot; si attiva l&#39;evento click dell&#39;elemento file. Questo consente all&#39;utente di sfogliare e selezionare la foto da allegare. Quindi usiamo l&#39;oggetto FileReader javascript per ottenere la stringa codificata base64 dell&#39;immagine. La stringa immagine base64 viene memorizzata nel campo di testo del modulo. Quando il modulo viene inviato, estraiamo questo valore e lo inseriamo nell’elemento img dell’XML. Questo XML viene quindi utilizzato per unire con l&#39;xdp per generare il pdf finale.

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

Il codice di cui sopra viene eseguito quando attiviamo l’evento click dell’elemento file . Nella riga 5 estraiamo il contenuto del file caricato come stringa base64 e lo memorizziamo nel campo di testo. Questo valore viene quindi estratto quando il modulo viene inviato al nostro servlet.

Quindi configuriamo le seguenti proprietà (avanzate) del nostro modulo mobile in AEM

* URL di invio: http://localhost:4502/bin/handlemobileformsubmission. Questo è il nostro servlet che unirà i dati inviati con il modello xdp
* Profilo di rendering HTML - Assicurati di selezionare &quot;AddImageToMobileForm&quot;. Questo attiverà il codice per aggiungere un’immagine al modulo.

Per testare questa funzionalità sul tuo server segui i seguenti passaggi:

* [Distribuzione del bundle AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Distribuzione del pacchetto di sviluppo con il pacchetto utente del servizio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Scarica e installa il pacchetto associato a questo articolo.](assets/pdf-from-mobile-form-submission.zip)

* Assicurati che l&#39;URL di invio e il profilo di rendering HTML siano impostati correttamente visualizzando la pagina delle proprietà di [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Anteprima di XDP come HTML](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Aggiungi un’immagine al modulo e invia. Dovresti recuperare il PDF con l&#39;immagine al suo interno.

