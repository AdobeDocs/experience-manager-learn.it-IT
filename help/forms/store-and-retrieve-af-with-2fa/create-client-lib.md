---
title: Creare librerie client
description: Crea una libreria client per gestire l’evento clic del pulsante "Salva ed esci"
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 6%

---

# Crea libreria client

Crea [libreria client](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=it) che includerà il codice per richiamare il metodo `doAjaxSubmitWithFileAttachment` del `guideBridge` API sull’evento clic del pulsante identificato dalla classe CSS **pulsante salva**.  Trasmettiamo i dati del modulo adattivo, `fileMap`e `mobileNumber` all’endpoint in ascolto su `**/bin/storeafdatawithattachments`

Dopo il salvataggio dei dati del modulo, viene generato un ID applicazione univoco che viene presentato all&#39;utente in una finestra di dialogo. Quando si chiude la finestra di dialogo, l’utente viene reindirizzato al modulo che consente di recuperare il modulo adattivo salvato utilizzando l’ID applicazione univoco.

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
                  x.data.applicationID +
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
> Abbiamo utilizzato [libreria JavaScript di avvio](https://bootboxjs.com/examples.html) per visualizzare la finestra di dialogo

Le librerie client utilizzate in questo esempio possono essere [scaricato da qui.](assets/store-af-with-attachments-client-lib.zip)

## Passaggi successivi

[Verificare gli utenti con il servizio OTP](./verify-users-with-otp.md)
