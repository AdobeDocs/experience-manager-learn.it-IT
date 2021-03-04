---
title: Creare librerie client
description: Crea clientlibrary per gestire l'evento click del pulsante "Save and Exit"
feature: Moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Sviluppo
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 8%

---

# Creare una libreria client

Crea [client lib](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html) che includerà il codice per richiamare il metodo `doAjaxSubmitWithFileAttachment` dell’ API `guideBridge` sull’evento clic del pulsante identificato dalla classe CSS **savebutton**.  Trasmettiamo i dati del modulo adattivo `fileMap` e il `mobileNumber` all’endpoint in ascolto su `**/bin/storeafdatawithattachments`

Una volta salvati i dati del modulo, viene generato un ID applicazione univoco che viene presentato all’utente in una finestra di dialogo. Quando si chiude la finestra di dialogo, l’utente viene portato al modulo che consente loro di recuperare il modulo adattivo salvato utilizzando l’ID applicazione univoco.

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> Abbiamo utilizzato [libreria javascript di bootbox](http://bootboxjs.com/examples.html) per visualizzare la finestra di dialogo

Le librerie client utilizzate in questo esempio possono essere [scaricate da qui](assets/client-libraries.zip)
