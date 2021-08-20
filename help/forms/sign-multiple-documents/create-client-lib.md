---
title: Crea libreria client
description: Codice libreria client per recuperare il modulo successivo da firmare
feature: Moduli adattivi
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Sviluppo
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 4%

---

# Creare una libreria client

Crea una libreria client personalizzata, clientlib per farla breve, per estrarre i parametri url che passano tali parametri nella chiamata di GET. La chiamata GET viene effettuata a un servlet montato su /bin/getnextformtosign che restituisce l’url del modulo successivo da firmare nel pacchetto.

Di seguito è riportato il codice utilizzato nella funzione javascript clientlib


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Assets

[La clientlib può essere scaricata da qui](assets/get-next-form-client-lib.zip)
