---
title: Utilizzo delle API di geolocalizzazione in Adaptive Forms
description: Compilare i campi indirizzo nel modulo utilizzando le API di geolocalizzazione
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
duration: 88
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Utilizzo delle API di geolocalizzazione in Adaptive Forms{#using-geolocation-api-s-in-adaptive-forms}

In questo articolo, vedremo come utilizzare l’API di geolocalizzazione di Google per popolare i campi di un modulo adattivo. Questo caso d’uso viene comunemente utilizzato quando si desidera compilare i campi indirizzo correnti di un modulo.

Sono stati seguiti i seguenti passaggi per utilizzare l’API di geolocalizzazione in Adaptive Forms.

1. [Ottieni la chiave API](https://developers.google.com/maps/documentation/javascript/get-api-key) da Google per utilizzare la piattaforma Google Maps. È possibile ottenere una chiave di prova valida per 1 anno.

1. Il frammento di modulo adattivo è stato creato con campi che contengono l’indirizzo corrente

1. L’API di geolocalizzazione è stata richiamata all’evento click dell’oggetto immagine del modulo adattivo

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

![Campi popolati con api geoloaction](assets/capture-4.gif)

Nella riga 1 utilizziamo l’API di geolocalizzazione HTML per ottenere la posizione corrente. Una volta ottenuta la posizione corrente, questa viene passata alla funzione showPosition.

Nella funzione showPosition utilizziamo l’API Google per recuperare i dettagli dell’indirizzo per la latitudine e la longitudine specificate.

Il JSON restituito dall’API viene quindi analizzato per impostare i campi del modulo adattivo.

>[!NOTE]
>
>A scopo di test, puoi utilizzare il protocollo HTTP con localhost nell’URL.
>
>Per il server di produzione, per ottenere questa funzionalità dovrai abilitare SSL per il server AEM.
>
>Il campione associato a questo articolo è stato testato con l’indirizzo USA. Se desideri utilizzare questa funzionalità in altre posizioni geografiche, potrebbe essere necessario modificare l’analisi JSON.

Per attivare questa funzionalità sul server, attieniti alla seguente procedura

* Installa e avvia il server AEM Forms.
> Questa funzionalità è stata testata su AEM Forms 6.3 e versioni successive
>* [Ottieni chiave API Google](https://developers.google.com/maps/documentation/javascript/get-api-key).
>* [Importa le risorse correlate a questo articolo in AEM.](assets/geolocationapi.zip)
>* [Apri il frammento del modulo adattivo in modalità di modifica.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
>* Apri l’editor di regole per il componente Scelta immagine.
>* Sostituisci &lt;your_api_key> con la chiave API di Google.
>* Salva le modifiche.
>* [Anteprima modulo](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
>* Fai clic sull’icona &quot;geolocalizzazione&quot;.
>* Il modulo deve essere compilato con la posizione corrente.
