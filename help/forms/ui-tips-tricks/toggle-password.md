---
title: Alcuni utili suggerimenti e trucchi per l’interfaccia utente
description: Documento che illustra alcuni utili suggerimenti sull’interfaccia utente
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 96b78ff5056bd9c2be39fb2cf21b4f92863af089
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 4%

---

# Attiva/disattiva visibilità campo password

Un caso d’uso comune consiste nell’consentire ai compilatori di passare alla visibilità del testo immesso nel campo password.
Per eseguire questo caso d’uso, ho utilizzato l’icona occhio dal [Libreria a forma di carattere](https://fontawesome.com/). I CSS richiesti e eye.svg sono inclusi nella libreria client creata per questa dimostrazione.

## Esempio live

[Questa funzionalità è disponibile per il test qui](https://forms.enablementadobe.com/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)

## Codice di esempio

Nel modulo adattivo è presente un campo di tipo PasswordBox denominato **ssnField**.

Il codice seguente viene eseguito al caricamento del modulo

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

Il seguente CSS è stato utilizzato per posizionare il **occhio** icona all’interno del campo password

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

## Distribuire l’esempio di password di attivazione/disattivazione

* Scarica la [libreria client](assets/simple-ui-tips.zip)
* Scarica la [modulo di esempio](assets/simple-ui-tricks-form.zip)
* Importa la libreria client utilizzando [interfaccia utente di gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa il modulo di esempio utilizzando [Forms e documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


