---
title: Funzionalità di completamento automatico in AEM Forms
description: Consente agli utenti di trovare e selezionare rapidamente da un elenco prepopolato di valori durante la digitazione, sfruttando le funzioni di ricerca e filtro.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Implementazione automatica completata

Implementa la funzionalità di completamento automatico nei moduli AEM utilizzando la funzione di completamento automatico di jquery.
L’esempio incluso in questo articolo utilizza una varietà di origini dati (array statico, array dinamico popolato da una risposta REST API) per popolare i suggerimenti man mano che l’utente inizia a digitare nel campo di testo.

Il codice utilizzato per eseguire la funzionalità di completamento automatico è associato all’evento initialize del campo .


## Suggerimenti per il nome del paese

![suggerimenti per il paese](assets/auto-complete1.png)

## Suggerimento per l&#39;indirizzo

![suggerimenti per il paese](assets/auto-complete2.png)

Di seguito è riportato il codice utilizzato per fornire suggerimenti sull&#39;indirizzo della strada

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```

## Suggerimenti con emoji

![suggerimenti per il paese](assets/auto-complete3.png)

Il codice seguente è stato utilizzato per visualizzare le emoji nell&#39;elenco dei suggerimenti

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

La [è possibile scaricare il modulo di esempio](assets/auto-complete-form.zip) da qui. Assicurati di fornire il tuo nome utente/chiave API utilizzando l&#39;editor di codice per il codice per effettuare chiamate REST con successo.

>[!NOTE]
>
> Per il completamento automatico, assicurati che il modulo utilizzi la seguente libreria client **cq.jquery.ui**. Questa libreria client viene fornita con AEM.
