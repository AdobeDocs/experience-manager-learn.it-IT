---
title: Suggerimenti e trucchi utili per l’interfaccia
description: Documento che illustra alcuni utili suggerimenti sull’interfaccia
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
exl-id: 13b9cd28-2797-4da9-a300-218e208cd21b
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# Attiva/disattiva visibilità campi password

Un caso d’uso comune consiste nel consentire ai compilatori di moduli di attivare o disattivare la visibilità del testo immesso nel campo della password.
Per eseguire questo caso d’uso, ho utilizzato l’icona dell’occhio di [Libreria eccezionale di caratteri](https://fontawesome.com/). I file CSS e eye.svg richiesti sono inclusi nella libreria client creata per questa dimostrazione.



## Codice di esempio

Il modulo adattivo dispone di un campo di tipo PasswordBox denominato **ssnField**.

Il seguente codice viene eseguito al caricamento del modulo

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

Il seguente CSS è stato utilizzato per posizionare **occhio** icona nel campo della password

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## Esempio di implementazione dell’interruttore della password

* Scarica il file [libreria client](assets/simple-ui-tips.zip)
* Scarica il file [modulo di esempio](assets/simple-ui-tricks-form.zip)
* Importare la libreria client utilizzando [interfaccia utente gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importare il modulo di esempio utilizzando [Forms e documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)
