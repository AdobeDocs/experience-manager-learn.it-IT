---
title: Suggerimenti e trucchi utili per l’interfaccia
description: Documento che illustra alcuni utili suggerimenti sull’interfaccia
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
exl-id: 13b9cd28-2797-4da9-a300-218e208cd21b
last-substantial-update: 2019-07-07T00:00:00Z
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# Attiva/disattiva visibilità campi password

Un caso d’uso comune consiste nel consentire ai compilatori di moduli di attivare o disattivare la visibilità del testo immesso nel campo della password.
Per eseguire questo caso d&#39;uso, ho utilizzato l&#39;icona dell&#39;occhio della [Libreria di caratteri fantastici](https://fontawesome.com/). I file CSS e eye.svg richiesti sono inclusi nella libreria client creata per questa dimostrazione.



## Codice di esempio

Il modulo adattivo contiene un campo di tipo PasswordBox denominato **ssnField**.

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

Il seguente file CSS è stato utilizzato per posizionare l&#39;icona **occhio** nel campo password

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

* Scarica la [libreria client](assets/simple-ui-tips.zip)
* Scarica il [modulo di esempio](assets/simple-ui-tricks-form.zip)
* Importa la libreria client utilizzando l&#39;[interfaccia utente di Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
* Importa il modulo di esempio utilizzando [Forms e il documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Anteprima modulo](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)
