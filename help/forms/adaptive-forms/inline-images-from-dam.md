---
title: Visualizzazione di immagini in linea in Forms adattivo
description: Visualizzare le immagini caricate in linea in Adaptive Forms
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
kt: kt-11307
source-git-commit: 853c4fedd4b8db594aa0b53fd2d27d996811f14e
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Visualizza l&#39;immagine DAM in Forms adattivo

Un caso d&#39;uso comune è quello di visualizzare le immagini che risiedono nell&#39;archivio crx in linea in un modulo adattivo.

## Aggiungi immagine segnaposto

Il primo passaggio consiste nell’anteporre un div segnaposto al componente pannello. Nel codice sottostante il componente pannello è identificato dal nome della classe CSS del foto-upload. La funzione JavaScript fa parte della libreria client associata ai moduli adattivi. Questa funzione viene chiamata nell&#39;evento initialize del componente file allegato.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Visualizza immagine in linea

Dopo che l’utente ha selezionato l’immagine, il campo nascosto ImageName viene popolato con il nome immagine selezionato. Questo nome immagine viene quindi passato alla funzione damURLToFile che richiama la funzione createFile per convertire un URL in un BLOB per FileReader.readAsDataURL().

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

* Scarica e installa la [libreria client e immagini di esempio](assets/InlineDAMImage.zip) nell’istanza di AEM utilizzando AEM Package Manager.
* Scarica e installa la [modulo di esempio](assets/FieldInspectionForm.zip) sulla tua istanza AEM utilizzando AEM package manager.
* Posiziona il browser su [ModuloIspezioneFile](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Selezionare una delle staffe
* Dovresti visualizzare l’immagine visualizzata nel modulo