---
title: Visualizzazione di immagini DAM in linea in Adaptive Forms
description: Visualizzare immagini DAM in linea in Adaptive Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
duration: 92
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Visualizzare l’immagine DAM in Adaptive Forms

Un caso d’uso comune consiste nel visualizzare le immagini residenti nell’archivio crx in linea in un modulo adattivo.

## Aggiungi immagine segnaposto

Il primo passaggio consiste nell’anteporre un div segnaposto al componente pannello. Nel codice seguente il componente pannello è identificato dal nome della classe CSS photo-upload. La funzione JavaScript fa parte della libreria client associata ai moduli adattivi. Questa funzione viene chiamata nell’evento di inizializzazione del componente file allegato.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Visualizza immagine in linea

Dopo che l’utente ha selezionato l’immagine, nel campo nascosto NomeImmagine viene inserito il nome immagine selezionato. Questo nome di immagine viene quindi passato alla funzione damURLToFile che richiama la funzione createFile per convertire un URL in un BLOB per FileReader.readAsDataURL().

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### Distribuisci sul server

* Scarica e installa [libreria client e immagini di esempio](assets/InlineDAMImage.zip) sull’istanza AEM tramite Gestione pacchetti AEM.
* Scarica e installa [modulo di esempio](assets/FieldInspectionForm.zip) sull’istanza di AEM utilizzando Gestione pacchetti AEM.
* Puntare il browser a [ModuloIspezioneFile](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Selezionate uno degli staffaggi
* Dovresti visualizzare l’immagine nel modulo
