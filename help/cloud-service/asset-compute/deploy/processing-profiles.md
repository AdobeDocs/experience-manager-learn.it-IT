---
title: Integrare i processi di lavoro di Asset Compute con i profili di elaborazione AEM
description: AEM as a Cloud Service si integra con i processi di lavoro Asset Compute implementati in Adobe I/O Runtime tramite i profili di elaborazione di AEM Assets. I profili di elaborazione sono configurati nel servizio Author per elaborare risorse specifiche utilizzando processi di lavoro personalizzati e archiviare i file generati dai processi di lavoro come rappresentazioni delle risorse.
feature: Microservizi Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrazioni, Sviluppo
role: Developer (Sviluppatore)
level: Intermedio, esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 2%

---


# Integrazione con i profili di elaborazione AEM

Affinché i processi di lavoro Asset Compute possano generare rappresentazioni personalizzate in AEM as a Cloud Service, devono essere registrati nel servizio Author di AEM as a Cloud Service tramite Profili di elaborazione. Per tutte le risorse soggette a tale profilo di elaborazione, il processo di lavoro verrà richiamato al momento del caricamento o della rielaborazione e il rendering personalizzato verrà generato e reso disponibile tramite le rappresentazioni della risorsa.

## Definire un profilo di elaborazione

Creare innanzitutto un nuovo profilo di elaborazione che richiamerà il processo di lavoro con i parametri configurabili.

![Profilo di elaborazione](./assets/processing-profiles/new-processing-profile.png)

1. Accedi al servizio Author di AEM as a Cloud Service come __Amministratore AEM__. Si tratta di un tutorial che consigliamo di utilizzare un ambiente di sviluppo o un ambiente in una sandbox.
1. Passa a __Strumenti > Risorse > Profili di elaborazione__
1. Tocca il pulsante __Crea__
1. Denomina il profilo di elaborazione, `WKND Asset Renditions`
1. Tocca la scheda __Personalizzato__ e tocca __Aggiungi nuovo__
1. Definire il nuovo servizio
   + __Nome rappresentazione:__ `Circle`
      + Rendering del nome file che verrà utilizzato per identificare questo rendering in AEM Assets
   + __Estensione:__ `png`
      + Estensione del rendering che verrà generato. Impostato su `png` in quanto è il formato di output supportato supportato dal servizio Web del processo di lavoro e restituisce uno sfondo trasparente dietro il cerchio tagliato.
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Questo è l’URL del processo di lavoro ottenuto tramite `aio app get-url`. Assicurati che i punti URL siano nell’area di lavoro corretta in base all’ambiente AEM as a Cloud Service.
      + Assicurati che l&#39;URL del lavoratore punti all&#39;area di lavoro corretta. AEM as a Cloud Service Stage deve utilizzare l’URL dell’area di lavoro Stage e AEM as a Cloud Service Production deve utilizzare l’URL dell’area di lavoro Produzione.
   + __Parametri del servizio__
      + Tocca __Aggiungi parametro__
         + Chiave: `size`
         + Valore: `1000`
      + Tocca __Aggiungi parametro__
         + Chiave: `contrast`
         + Valore: `0.25`
      + Tocca __Aggiungi parametro__
         + Chiave: `brightness`
         + Valore: `0.10`
      + Queste coppie chiave/valore che vengono passate al processo di lavoro Asset Compute e sono disponibili tramite l’oggetto JavaScript `rendition.instructions` .
   + __Tipi mime__
      + __Include:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Questi tipi MIME sono gli unici moduli npm del processo di lavoro. Questo elenco limita le risorse che verranno elaborate dal processo di lavoro personalizzato.
      + __Escludi:__ `Leave blank`
         + Non elaborare mai le risorse con questi tipi MIME utilizzando questa configurazione del servizio. In questo caso, utilizziamo solo un elenco Consentiti.
1. Tocca __Salva__ in alto a destra

## Applicare e richiamare un profilo di elaborazione

1. Seleziona il profilo di elaborazione appena creato, `WKND Asset Renditions`
1. Tocca __Applica profilo a cartelle__ nella barra delle azioni superiore
1. Seleziona una cartella a cui applicare il profilo di elaborazione, ad esempio `WKND` e tocca __Applica__
1. Passa alla cartella a cui non è stato applicato il profilo di elaborazione tramite __AEM > Risorse > File__ e tocca `WKND`.
1. Carica alcune nuove risorse immagini ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) e [sample-3.jpg](../assets/samples/sample-3.jpg)) in qualsiasi cartella sotto la cartella con il profilo di elaborazione applicato e attendi l&#39;elaborazione della risorsa caricata.
1. Tocca la risorsa per aprirne i dettagli
   + Le rappresentazioni predefinite possono generare e visualizzare più rapidamente in AEM rispetto alle rappresentazioni personalizzate.
1. Apri la vista __Rendering__ dalla barra laterale sinistra
1. Tocca la risorsa denominata `Circle.png` e controlla il rendering generato

   ![Rendering generato](./assets/processing-profiles/rendition.png)

## Completato!

Congratulazioni! Hai terminato l’ [esercitazione](../overview.md) su come estendere i microservizi Asset Compute di AEM as a Cloud Service! Ora dovresti avere la possibilità di configurare, sviluppare, testare, eseguire il debug e distribuire processi di lavoro Asset Compute personalizzati da utilizzare dal servizio Author di AEM as a Cloud Service.

### Rivedi il codice sorgente completo del progetto su Github

Il progetto Asset Compute finale è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, ad esempio. `.env`,  `.config.json` o  `.aio`._

## Risoluzione dei problemi

+ [Rendering personalizzato mancante dalla risorsa in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [L’elaborazione delle risorse non riesce in AEM](../troubleshooting.md#asset-processing-fails)
