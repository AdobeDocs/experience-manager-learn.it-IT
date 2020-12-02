---
title: Visualizzazione di immagini in linea in Forms adattivo
seo-title: Visualizzazione di immagini in linea in Forms adattivo
description: Visualizzare le immagini caricate in linea in Forms adattivo
seo-description: Visualizzare le immagini caricate in linea in Forms adattivo
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# Immagini in linea in Forms adattivo

Un caso d’uso comune consiste nel visualizzare l’immagine caricata come immagine in linea nel modulo adattivo. Per impostazione predefinita, l&#39;immagine caricata è visualizzata come collegamento e questa esperienza può essere migliorata visualizzando l&#39;immagine in Modulo adattivo. Questo articolo illustra i passaggi necessari per visualizzare le immagini in linea.

## Aggiungi immagine segnaposto

Il primo passaggio consiste nell’anteporre un div segnaposto al componente di allegato del file. Nel codice sotto il componente allegato file è identificato dal nome della classe CSS del file di caricamento foto. La funzione JavaScript fa parte della libreria client associata ai moduli adattivi. Questa funzione viene chiamata nell&#39;evento initialize del componente file allegato.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### Visualizza immagine in linea

Dopo che l’utente ha caricato l’immagine, la funzione elencata di seguito viene richiamata in corrispondenza dell’evento commit del componente file allegato. La funzione riceve l&#39;oggetto file caricato come argomento.

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### Implementazione sul server

* Scaricate e installate la [libreria client](assets/inline-image-client-library.zip) nell&#39;istanza AEM utilizzando AEM gestore pacchetti.
* Scaricate e installate il [modulo di esempio](assets/inline-image-af.zip) nell&#39;istanza AEM utilizzando AEM gestore pacchetti.
* Posizionare il browser su [Aggiungi immagine in linea](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Fate clic sul pulsante &quot;Allega la foto&quot; per aggiungere l&#39;immagine