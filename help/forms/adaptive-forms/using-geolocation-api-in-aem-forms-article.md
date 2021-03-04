---
title: Utilizzo delle API di geolocalizzazione nei moduli adattivi
seo-title: Utilizzo delle API di geolocalizzazione nei moduli adattivi
description: Compilare campi indirizzo nel modulo utilizzando l’api di geolocalizzazione
seo-description: Compilare campi indirizzo nel modulo utilizzando l’api di geolocalizzazione
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: Moduli adattivi
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 1%

---


# Utilizzo delle API di geolocalizzazione nei moduli adattivi{#using-geolocation-api-s-in-adaptive-forms}

Visita la pagina [Esempi di AEM Forms](https://forms.enablementadobe.com/content/samples/samples.html?query=0) per un collegamento a una demo in tempo reale di questa funzionalità.

In questo articolo, analizzeremo l’utilizzo dell’API di geolocalizzazione di Google per popolare i campi di un modulo adattivo. Questo caso d’uso viene comunemente utilizzato quando si desidera compilare i campi dell’indirizzo correnti in un modulo.

I passaggi seguenti sono stati seguiti per utilizzare l’API di geolocalizzazione in Moduli adattivi.

1. [Ottieni API ](https://developers.google.com/maps/documentation/javascript/get-api-key) Keyda Google per utilizzare la piattaforma Google Maps. È possibile ottenere una chiave di prova valida per 1 anno.

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

Nella riga 1 usiamo l’API di geolocalizzazione HTML per ottenere la posizione corrente. Una volta ottenuta la posizione corrente passiamo la posizione corrente per mostrare la funzione Position.

Nella funzione showPosition, utilizziamo l’API di Google per recuperare i dettagli dell’indirizzo per la latitudine e la longitudine specificate.

Il JSON restituito dall’API viene quindi analizzato per impostare i campi Modulo adattivo.

>[!NOTE]
>
>A scopo di test, puoi utilizzare il protocollo HTTP con localhost nell’URL.
>
>Per il server di produzione, è necessario abilitare SSL per il server AEM per ottenere questa funzionalità.
>
>Il campione associato a questo articolo è stato testato con l’indirizzo US. Se desideri utilizzare questa funzionalità in altre posizioni geografiche, potrebbe essere necessario modificare l’analisi JSON.

Per ottenere questa funzionalità sul server, segui i seguenti passaggi

* Installa e avvia il server AEM Forms.

>!![NOTE] Questa funzionalità è stata testata su AEM Forms 6.3 e versioni successive
* [Ottieni la chiave API di Google](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importa le risorse correlate a questo articolo in AEM.](assets/geolocationapi.zip)
* [Apri il frammento Modulo adattivo in modalità di modifica.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Apri l’editor di regole per il componente Scelta immagine .
* Sostituisci &lt;your_api_key> con la chiave API di Google.
* Salva le modifiche.
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Fai clic sull&#39;icona &quot;geolocalizzazione&quot;.
* Il modulo deve essere compilato con la posizione corrente.
