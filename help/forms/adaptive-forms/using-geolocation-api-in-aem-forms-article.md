---
title: Utilizzo delle API di geolocalizzazione in Forms adattivo
seo-title: Utilizzo delle API di geolocalizzazione in Forms adattivo
description: Compilare campi indirizzo nel modulo utilizzando l'API di geolocalizzazione
seo-description: Compilare campi indirizzo nel modulo utilizzando l'API di geolocalizzazione
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: adaptive-forms,
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 0%

---


# Utilizzo delle API di geolocalizzazione in Forms adattivo{#using-geolocation-api-s-in-adaptive-forms}

Visitate la [pagina di esempi](https://forms.enablementadobe.com/content/samples/samples.html?query=0) AEM Forms per un collegamento a una dimostrazione live di questa funzionalità.

In questo articolo verrà illustrato come utilizzare l&#39;API di geolocalizzazione di Google per compilare i campi di un modulo adattivo. Questo caso d’uso è comunemente utilizzato per compilare i campi indirizzo correnti in un modulo.

Sono stati seguiti i passaggi seguenti per utilizzare l&#39;API Geolocation in Forms adattivo.

1. [Ottenere la chiave](https://developers.google.com/maps/documentation/javascript/get-api-key) API da Google per utilizzare la piattaforma Google Maps. È possibile ottenere una chiave di prova valida per 1 anno.

1. Il frammento di modulo adattivo è stato creato con campi che contengono l&#39;indirizzo corrente

1. L&#39;API Geolocation è stata richiamata sull&#39;evento click dell&#39;oggetto immagine del modulo adattivo

1. I dati JSON restituiti dalla chiamata API sono stati analizzati e i valori dei campi modulo adattivo sono stati impostati di conseguenza.

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

![Campi popolati con API geoloaction](assets/capture-4.gif)

Nella riga 1 utilizziamo l&#39;API di geolocalizzazione HTML per ottenere la posizione corrente. Una volta ottenuta la posizione corrente, trasmettiamo la posizione corrente alla funzione showPosition.

Nella funzione showPosition, utilizziamo l&#39;API di Google per recuperare i dettagli dell&#39;indirizzo per la latitudine e la longitudine date.

Il JSON restituito dall&#39;API viene quindi analizzato per impostare i campi modulo adattivo.

>[!NOTE]
>
>A scopo di test, potete utilizzare il protocollo HTTP con localhost nell’URL.
>
>Per il server di produzione, è necessario abilitare SSL per il server AEM per ottenere questa funzionalità.
>
>Il campione associato a questo articolo è stato testato con l&#39;indirizzo US. Se desiderate utilizzare questa funzionalità in altre aree geografiche, potreste dover modificare l&#39;analisi JSON.

Per scaricare questa funzionalità sul server, attenetevi alla seguente procedura

* Installate e avviate  server AEM Forms.

>!![NOTE] Questa funzionalità è stata testata su  AEM Forms 6.3 e versioni successive
* [Ottieni chiave](https://developers.google.com/maps/documentation/javascript/get-api-key)API Google.
* [Importa in AEM le risorse correlate a questo articolo.](assets/geolocationapi.zip)
* [Aprire il frammento Modulo adattivo in modalità di modifica.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Aprite l’editor delle regole per il componente Scelta immagine.
* Sostituite &lt;your_api_key> con la chiave API di Google.
* Salvare le modifiche.
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Fare clic sull&#39;icona &quot;geolocalizzazione&quot;.
* Il modulo deve essere compilato con la posizione corrente.
