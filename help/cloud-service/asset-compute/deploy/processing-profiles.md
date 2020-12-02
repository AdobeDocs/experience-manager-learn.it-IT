---
title: Integrare  Asset compute con i profili di elaborazione AEM
description: AEM come Cloud Service si integra con  lavoratori Asset compute distribuiti in Adobe I/O Runtime tramite  profili di elaborazione AEM Assets. I profili di elaborazione sono configurati nel servizio Autore per elaborare risorse specifiche utilizzando i lavoratori personalizzati e archiviare i file generati dai lavoratori come rappresentazioni delle risorse.
feature: asset-compute, processing-profiles
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 2%

---


# Integrazione con AEM profili di elaborazione

Affinché  lavoratori di Asset compute possano generare rappresentazioni personalizzate in AEM come Cloud Service, devono essere registrati in AEM come servizio di creazione Cloud Service tramite Profili di elaborazione. Per tutte le risorse soggette a tale profilo di elaborazione, il lavoratore verrà richiamato al momento del caricamento o della rielaborazione e la rappresentazione personalizzata verrà generata e resa disponibile tramite le rappresentazioni della risorsa.

## Definire un profilo di elaborazione

Creare innanzitutto un nuovo profilo di elaborazione che richiamerà il lavoratore con i parametri configurabili.

![Profilo di elaborazione](./assets/processing-profiles/new-processing-profile.png)

1. Accedete a AEM come servizio di authoring di Cloud Service come __AEM Administrator__. Si consiglia di utilizzare un ambiente Dev o un ambiente in una sandbox.
1. Passa a __Strumenti > Risorse > Profili di elaborazione__
1. Toccare il pulsante __Crea__
1. Denominate il profilo di elaborazione, `WKND Asset Renditions`
1. Toccate la scheda __Personalizzato__, quindi toccate __Aggiungi nuovo__
1. Definire il nuovo servizio
   + __Nome rappresentazione:__ `Circle`
      + La rappresentazione del nome file che verrà utilizzata per identificare la rappresentazione in  AEM Assets
   + __Estensione:__ `png`
      + Estensione della rappresentazione che verrà generata. Impostato su `png` in quanto questo è il formato di output supportato supportato dal servizio Web del lavoratore e restituisce uno sfondo trasparente dietro il cerchio tagliato.
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Si tratta dell&#39;URL del lavoratore ottenuto tramite `aio app get-url`. Assicuratevi che l’URL punti nell’area di lavoro corretta in base al AEM come ambiente di Cloud Service.
      + Accertatevi che l’URL del lavoratore faccia riferimento all’area di lavoro corretta. AEM come fase Cloud Service deve utilizzare l’URL dell’area di lavoro dello stage e AEM come produzione Cloud Service deve utilizzare l’URL dell’area di lavoro Produzione.
   + __Parametri del servizio__
      + Toccare __Aggiungi parametro__
         + Chiave: `size`
         + Valore: `1000`
      + Toccare __Aggiungi parametro__
         + Chiave: `contrast`
         + Valore: `0.25`
      + Toccare __Aggiungi parametro__
         + Chiave: `brightness`
         + Valore: `0.10`
      + Queste coppie chiave/valore che vengono trasmesse al lavoratore del Asset compute  e sono disponibili tramite l&#39;oggetto JavaScript `rendition.instructions`.
   + __Tipi mime__
      + __Include:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Questi tipi MIME sono gli unici moduli npm del lavoratore. Questo elenco limita le risorse che verranno elaborate dal lavoratore personalizzato.
      + __Esclude:__ `Leave blank`
         + Non elaborare mai le risorse con questi tipi MIME utilizzando questa configurazione del servizio. In questo caso, utilizziamo solo un elenco consentiti .
1. Toccate __Salva__ in alto a destra

## Applicare e richiamare un profilo di elaborazione

1. Selezionare il profilo di elaborazione appena creato, `WKND Asset Renditions`
1. Toccate __Applica profilo alle cartelle__ nella barra delle azioni superiore
1. Selezionare una cartella a cui applicare il profilo di elaborazione, ad esempio `WKND` e toccare __Applica__
1. Andate alla cartella a cui non è stato applicato il profilo di elaborazione tramite __AEM > Risorse > File__ e toccate in `WKND`.
1. Caricate alcune nuove risorse di immagini ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) e [sample-3.jpg](../assets/samples/sample-3.jpg)) in qualsiasi cartella all’interno della cartella in cui è stato applicato il profilo di elaborazione e attendete l’elaborazione della risorsa caricata.
1. Toccate la risorsa per aprirne i dettagli
   + Le rappresentazioni predefinite possono generare e visualizzare più rapidamente AEM rispetto alle rappresentazioni personalizzate.
1. Aprire la visualizzazione __Rendering__ dalla barra laterale sinistra
1. Toccate la risorsa denominata `Circle.png` e controllate la rappresentazione generata

   ![Rappresentazioni generate](./assets/processing-profiles/rendition.png)

## Completato!

Congratulazioni! È stata completata l&#39; [tutorial](../overview.md) su come estendere AEM come Cloud Service  microservizi Asset compute! È ora possibile impostare, sviluppare, testare, eseguire il debug e distribuire i Asset compute di lavoro  personalizzati per l&#39;utilizzo da parte del AEM come servizio Cloud Service Autore.

### Leggi il codice sorgente completo del progetto su Github

Il progetto finale del Asset compute  è disponibile su Github all&#39;indirizzo:

+ [aem-guide-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene lo stato finale del progetto, popolato completamente con i casi di lavoro e test, ma non contiene credenziali, ad esempio. `.env`,  `.config.json` o  `.aio`._

## Risoluzione dei problemi

+ [Rendering personalizzato mancante dalla risorsa in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Elaborazione risorsa non riuscita in AEM](../troubleshooting.md#asset-processing-fails)
