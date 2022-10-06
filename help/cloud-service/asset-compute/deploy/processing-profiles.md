---
title: Integrare i processi di lavoro di Asset compute con i profili di elaborazione AEM
description: AEM as a Cloud Service si integra con i processi di lavoro di Asset compute distribuiti in Adobe I/O Runtime tramite i profili di elaborazione AEM Assets. I profili di elaborazione sono configurati nel servizio Author per elaborare risorse specifiche utilizzando processi di lavoro personalizzati e archiviare i file generati dai processi di lavoro come rappresentazioni delle risorse.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 2%

---

# Integrazione con i profili di elaborazione AEM

Affinché i processi di lavoro di Asset compute possano generare rappresentazioni personalizzate in AEM as a Cloud Service, è necessario che siano registrati nel servizio Author AEM as a Cloud Service tramite Profili di elaborazione. Per tutte le risorse soggette a tale profilo di elaborazione, il processo di lavoro verrà richiamato al momento del caricamento o della rielaborazione e il rendering personalizzato verrà generato e reso disponibile tramite le rappresentazioni della risorsa.

## Definire un profilo di elaborazione

Creare innanzitutto un nuovo profilo di elaborazione che richiamerà il processo di lavoro con i parametri configurabili.

![Profilo di elaborazione](./assets/processing-profiles/new-processing-profile.png)

1. Accedi a AEM servizio Author as a Cloud Service come __Amministratore AEM__. Si tratta di un tutorial che consigliamo di utilizzare un ambiente di sviluppo o un ambiente in una sandbox.
1. Passa a __Strumenti > Risorse > Profili di elaborazione__
1. Tocca __Crea__ pulsante
1. Assegna un nome al profilo di elaborazione, `WKND Asset Renditions`
1. Tocca __Personalizzato__ e tocca __Aggiungi nuovo__
1. Definire il nuovo servizio
   + __Nome rappresentazione:__ `Circle`
      + Nome file del rendering utilizzato per identificare il rendering in AEM Assets
   + __Estensione:__ `png`
      + Estensione del rendering generato. Imposta su `png` poiché si tratta del formato di output supportato supportato dal servizio Web del processo di lavoro, e si ottiene uno sfondo trasparente dietro il cerchio tagliato.
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Questo è l’URL del processo di lavoro ottenuto tramite `aio app get-url`. Assicurati che l’URL punti nell’area di lavoro corretta in base all’ambiente as a Cloud Service AEM.
      + Assicurati che l&#39;URL del lavoratore punti all&#39;area di lavoro corretta. AEM Stage as a Cloud Service deve utilizzare l’URL dell’area di lavoro Stage e AEM Produzione as a Cloud Service deve utilizzare l’URL dell’area di lavoro Produzione.
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
      + Queste coppie chiave/valore che vengono passate al processo di lavoro Asset compute e sono disponibili tramite `rendition.instructions` Oggetto JavaScript.
   + __Tipi mime__
      + __Include:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Questi tipi MIME sono gli unici moduli npm del processo di lavoro. Questo elenco limita le operazioni elaborate dal processo di lavoro personalizzato.
      + __Escludi:__ `Leave blank`
         + Non elaborare mai le risorse con questi tipi MIME utilizzando questa configurazione del servizio. In questo caso, utilizziamo solo un elenco consentiti.
1. Tocca __Salva__ in alto a destra

## Applicare e richiamare un profilo di elaborazione

1. Seleziona il nuovo profilo di elaborazione creato, `WKND Asset Renditions`
1. Tocca __Applica profilo a cartelle__ nella barra delle azioni superiore
1. Selezionare una cartella a cui applicare il profilo di elaborazione, ad esempio `WKND` e toccare __Applica__
1. Passa alla cartella a cui non è stato applicato il profilo di elaborazione tramite __AEM > Risorse > File__ e toccare `WKND`.
1. Carica alcune nuove risorse immagini ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg)e [sample-3.jpg](../assets/samples/sample-3.jpg)) in qualsiasi cartella della cartella a cui è stato applicato il profilo di elaborazione e attendi che la risorsa caricata venga elaborata.
1. Tocca la risorsa per aprirne i dettagli
   + Le rappresentazioni predefinite possono generare e visualizzare più rapidamente in AEM rispetto alle rappresentazioni personalizzate.
1. Apri __Rendering__ vista dalla barra laterale sinistra
1. Tocca la risorsa denominata `Circle.png` e controlla il rendering generato

   ![Rendering generato](./assets/processing-profiles/rendition.png)

## Completato!

Congratulazioni! Hai finito il [tutorial](../overview.md) su come estendere AEM microservizi Asset compute as a Cloud Service! Ora dovresti avere la possibilità di configurare, sviluppare, testare, eseguire il debug e distribuire processi di lavoro di Asset compute personalizzati da utilizzare dal servizio di authoring as a Cloud Service AEM.

### Rivedi il codice sorgente completo del progetto su Github

Il progetto di Asset compute finale è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, ad esempio. `.env`, `.config.json` oppure `.aio`._

## Risoluzione dei problemi

+ [Rendering personalizzato mancante dalla risorsa in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [L’elaborazione delle risorse non riesce AEM](../troubleshooting.md#asset-processing-fails)
