---
title: Funzionalità di completamento automatico in AEM Forms
description: Consente agli utenti di trovare e selezionare rapidamente da un elenco precompilato di valori durante la digitazione, sfruttando le funzionalità di ricerca e filtro.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
duration: 74
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# Implementazione del completamento automatico

Implementare la funzionalità di completamento automatico nei moduli AEM utilizzando la funzione di completamento automatico di jquery.
L’esempio incluso in questo articolo utilizza diverse origini di dati (array statico, array dinamico popolato da una risposta API REST) per compilare i suggerimenti quando l’utente inizia a digitare nel campo di testo.

Il codice utilizzato per eseguire la funzionalità di completamento automatico è associato all’evento di inizializzazione del campo.

## Suggerimento per l&#39;indirizzo

![country-suggestions](assets/auto-complete2.png)



Di seguito è riportato il codice utilizzato per fornire suggerimenti sugli indirizzi stradali

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





## Suggerimenti con le emoji

![country-suggestions](assets/auto-complete3.png)

Il seguente codice è stato utilizzato per visualizzare le emoji nell’elenco dei suggerimenti

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

Il [il modulo di esempio può essere scaricato](assets/auto-complete-form.zip) da qui. Assicurati di fornire un nome utente/chiave API personalizzato utilizzando l’editor di codice per il codice per effettuare chiamate REST riuscite.

>[!NOTE]
>
> Affinché il completamento automatico funzioni, assicurati che il modulo utilizzi la seguente libreria client **cq.jquery.ui**. Questa libreria client viene fornita con AEM.
