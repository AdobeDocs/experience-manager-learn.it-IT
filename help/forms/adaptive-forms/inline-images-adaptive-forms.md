---
title: Visualizzazione di immagini in linea in Forms adattivo
description: Visualizzare le immagini caricate in linea in Adaptive Forms
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# Immagini in linea in Forms adattivo

Un caso d’uso comune è la visualizzazione dell’immagine caricata come immagine in linea in Modulo adattivo. Per impostazione predefinita, l’immagine caricata viene visualizzata come collegamento e questa esperienza può essere migliorata visualizzando l’immagine in Modulo adattivo. Questo articolo illustra i passaggi necessari per visualizzare le immagini in linea.

## Aggiungi immagine segnaposto

Il primo passo è quello di anteporre un div segnaposto al componente file allegato. Nel codice sottostante il componente allegato file è identificato dal nome della classe CSS del file di caricamento foto. La funzione JavaScript fa parte della libreria client associata ai moduli adattivi. Questa funzione viene chiamata nell&#39;evento initialize del componente file allegato.

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

Dopo che l&#39;utente ha caricato l&#39;immagine, la funzione elencata di seguito viene richiamata nell&#39;evento commit del componente file allegato. La funzione riceve l&#39;oggetto file caricato come argomento.

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

### Distribuisci sul server

* Scarica e installa la [libreria client](assets/inline-image-client-library.zip) sulla tua istanza AEM utilizzando AEM package manager.
* Scarica e installa la [modulo di esempio](assets/inline-image-af.zip) sulla tua istanza AEM utilizzando AEM package manager.
* Posiziona il browser su [Aggiungi immagine in linea](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Fai clic sul pulsante &quot;Allega la tua foto&quot; per aggiungere l&#39;immagine
