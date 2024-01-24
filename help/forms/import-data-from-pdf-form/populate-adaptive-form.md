---
title: Compilare un modulo adattivo utilizzando il metodo setData
description: Invia il file pdf caricato per l’estrazione dei dati e compila il modulo adattivo con i dati estratti
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 38
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# Effettua chiamata Ajax

Quando l’utente carica il file pdf, dobbiamo effettuare una chiamata POST a un servlet e passare il documento PDF caricato nella richiesta POST. La richiesta POST restituisce un percorso per i dati esportati nell’archivio crx

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

Servlet installato su **_/bin/ExtractDataFromPDF_** estrae i dati dal file PDF e restituisce il percorso del nodo crx in cui vengono memorizzati i dati estratti.
Il [SetData di GuideBridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) viene quindi utilizzato per impostare i dati del modulo adattivo.

## Passaggi successivi

[Distribuire le risorse di esempio](./test-the-solution.md)
