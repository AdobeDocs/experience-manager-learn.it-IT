---
title: Crea libreria client
description: Codice libreria client per recuperare il modulo successivo da firmare
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
duration: 42
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# Creare una libreria client

Crea una libreria client personalizzata, clientlib in breve, per estrarre i parametri URL e trasmetterli nella chiamata GET. La chiamata GET viene effettuata a un servlet montato su /bin/getnextformtosign che restituisce l’URL del modulo successivo per l’accesso al pacchetto.

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

## Risorse

[La libreria client può essere scaricata da qui](assets/get-next-form-client-lib.zip)

## Passaggi successivi

[Crea un modello di modulo personalizzato per questo caso d’uso](./create-af-template.md)