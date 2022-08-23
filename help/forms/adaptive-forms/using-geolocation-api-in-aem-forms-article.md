---
title: Utilizzo delle API di geolocalizzazione in Forms adattivo
description: Compilare campi indirizzo nel modulo utilizzando l’API di geolocalizzazione
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 3%

---

# Utilizzo delle API di geolocalizzazione in Forms adattivo{#using-geolocation-api-s-in-adaptive-forms}

In questo articolo, analizzeremo l’utilizzo dell’API di geolocalizzazione di Google per compilare i campi di un modulo adattivo. Questo caso d’uso viene comunemente utilizzato quando si desidera compilare i campi dell’indirizzo correnti in un modulo.

I passaggi seguenti sono stati seguiti per utilizzare l’API di geolocalizzazione in Forms adattivo.

1. [Ottieni chiave API](https://developers.google.com/maps/documentation/javascript/get-api-key) da Google per utilizzare la piattaforma Google Maps. È possibile ottenere una chiave di prova valida per 1 anno.

1. Il frammento di modulo adattivo è stato creato con campi che contengono l’indirizzo corrente

1. L&#39;API di geolocalizzazione è stata richiamata sull&#39;evento click dell&#39;oggetto immagine di Modulo adattivo

1. I dati JSON restituiti dalla chiamata API sono stati analizzati e i valori dei campi del modulo adattivo sono stati impostati di conseguenza.

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![I campi si popolano con l’api geologica](assets/capture-4.gif)

Nella riga 1, l’API di geolocalizzazione di HTML viene utilizzata per ottenere la posizione corrente. Una volta ottenuta la posizione corrente passiamo la posizione corrente per mostrare la funzione Position.

Nella funzione showPosition, utilizziamo l’API Google per recuperare i dettagli dell’indirizzo per la latitudine e la longitudine specificate.

Il JSON restituito dall’API viene quindi analizzato per impostare i campi Modulo adattivo.

>[!NOTE]
>
>A scopo di test, puoi utilizzare il protocollo HTTP con localhost nell’URL.
>
>Per il server di produzione, è necessario abilitare SSL per il server di AEM per ottenere questa funzionalità.
>
>Il campione associato a questo articolo è stato testato con l’indirizzo US. Se desideri utilizzare questa funzionalità in altre posizioni geografiche, potrebbe essere necessario modificare l’analisi JSON.

Per ottenere questa funzionalità sul server, segui i seguenti passaggi

* Installa e avvia il server AEM Forms.

>!![NOTE] Questa funzionalità è stata testata su AEM Forms 6.3 e versioni successive
* [Ottieni chiave API Google](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importa in AEM le risorse correlate a questo articolo.](assets/geolocationapi.zip)
* [Apri il frammento Modulo adattivo in modalità di modifica.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Apri l’editor di regole per il componente Scelta immagine .
* Sostituisci il &lt;your_api_key> con la chiave API di Google.
* Salva le modifiche.
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Fai clic sull&#39;icona &quot;geolocalizzazione&quot;.
* Il modulo deve essere compilato con la posizione corrente.
