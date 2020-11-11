---
title: Creare librerie client
description: Crea clientlibrary per gestire l'evento click del pulsante "Save and Exit"
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 6%

---

# Crea lib client

Create [client lib](https://docs.adobe.com/content/help/it-IT/experience-manager-65/developing/introduction/clientlibs.html) che includerà il codice per richiamare il metodo `doAjaxSubmitWithFileAttachment` dell&#39; `guideBridge` API sull&#39;evento click del pulsante identificato dal **savebutton** della classe CSS.  Trasmettiamo i dati del modulo adattivo `fileMap`e l&#39; `mobileNumber` ascolto all&#39;endpoint `**/bin/storeafdatawithattachments`

Una volta salvati i dati del modulo, viene generato un ID applicazione univoco che viene presentato all&#39;utente in una finestra di dialogo. Quando si chiude la finestra di dialogo, l&#39;utente viene indirizzato al modulo che consente di recuperare il modulo adattivo salvato utilizzando l&#39;ID applicazione univoco.

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
> È stata utilizzata la libreria [javascript](http://bootboxjs.com/examples.html) di avvio per visualizzare la finestra di dialogo

Le librerie client utilizzate in questo esempio possono essere [scaricate da qui](assets/client-libraries.zip)
