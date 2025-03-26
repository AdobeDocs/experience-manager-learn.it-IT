---
title: Visualizzazione di immagini in linea in Adaptive Forms
description: Visualizzare le immagini caricate in linea in Adaptive Forms
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 58
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Immagini in linea in Adaptive Forms

Un caso d’uso comune consiste nel visualizzare l’immagine caricata come immagine in linea in Modulo adattivo. Per impostazione predefinita, l’immagine caricata viene visualizzata come collegamento e questa esperienza può essere migliorata visualizzando l’immagine in Modulo adattivo. Questo articolo illustra i passaggi necessari per visualizzare le immagini in linea.

## Aggiungi immagine segnaposto

Il primo passaggio consiste nell’anteporre un div segnaposto al componente allegato. Nel codice seguente il componente file allegato è identificato dal nome della classe CSS photo-upload. La funzione JavaScript fa parte della libreria client associata ai moduli adattivi. Questa funzione viene chiamata nell’evento di inizializzazione del componente file allegato.

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

Dopo che l’utente ha caricato l’immagine, la funzione indicata di seguito viene richiamata in caso di commit del componente file allegato. La funzione riceve l’oggetto file caricato come argomento.

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

* Scarica e installa la [libreria client](assets/inline-image-client-library.zip) nell&#39;istanza di AEM tramite Gestione pacchetti di AEM.
* Scarica e installa il [modulo di esempio](assets/inline-image-af.zip) nella tua istanza di AEM utilizzando Gestione pacchetti di AEM.
* Puntare il browser a [Aggiungi immagine in linea](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Fai clic sul pulsante &quot;Allega la tua foto&quot; per aggiungere un’immagine
