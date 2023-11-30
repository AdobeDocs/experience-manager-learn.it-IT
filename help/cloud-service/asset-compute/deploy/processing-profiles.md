---
title: Integrare i lavoratori Asset compute con i profili di elaborazione AEM
description: AEM as a Cloud Service si integra con i processi di lavoro Asset compute implementati in Adobe I/O Runtime tramite i Profili elaborazione AEM Assets. I Profili di elaborazione sono configurati nel servizio Author in modo da elaborare specifiche risorse tramite processi di lavoro personalizzati e archiviare i file generati da tali processi di lavoro come rappresentazioni delle risorse.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 2%

---

# Integrare con i profili di elaborazione dell’AEM

Ad Asset compute, per generare rappresentazioni personalizzate in AEM as a Cloud Service, i processi di lavoro devono essere registrati nel servizio AEM as a Cloud Service Author tramite Profili di elaborazione. Tutte le risorse soggette a tale profilo di elaborazione avranno il lavoratore richiamato al momento del caricamento o della rielaborazione e avranno la rappresentazione personalizzata generata e resa disponibile tramite le rappresentazioni della risorsa.

## Definire un profilo di elaborazione

Crea innanzitutto un nuovo Profilo di elaborazione che richiamerà il processo di lavoro con i parametri configurabili.

![Profilo di elaborazione](./assets/processing-profiles/new-processing-profile.png)

1. Accedi al servizio di authoring as a Cloud Service dell’AEM come __Amministratore AEM__. Trattandosi di un’esercitazione, consigliamo di utilizzare un ambiente di sviluppo o un ambiente in una sandbox.
1. Accedi a __Strumenti > Risorse > Profili elaborazione__
1. Tocca __Crea__ pulsante
1. Denomina il profilo di elaborazione, `WKND Asset Renditions`
1. Tocca il __Personalizzato__ , quindi tocca __Aggiungi nuovo__
1. Definisci il nuovo servizio
   + __Nome rappresentazione:__ `Circle`
      + Nome file della rappresentazione utilizzato per identificare la rappresentazione in AEM Assets
   + __Estensione:__ `png`
      + Estensione della rappresentazione generata. Imposta su `png` poiché questo è il formato di output supportato dal servizio web del lavoratore e risulta in uno sfondo trasparente dietro il ritaglio del cerchio.
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Questo è l’URL del lavoratore ottenuto tramite `aio app get-url`. Assicurati che l’URL punti nell’area di lavoro corretta in base all’ambiente as a Cloud Service dall’AEM.
      + Assicurarsi che l&#39;URL del lavoratore punti all&#39;area di lavoro corretta. Lo stage as a Cloud Service da AEM deve utilizzare l’URL dell’area di lavoro dello stage e la produzione as a Cloud Service da AEM deve utilizzare l’URL dell’area di lavoro di produzione.
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
      + Queste coppie chiave/valore vengono passate nel processo di lavoro Asset compute e sono disponibili tramite `rendition.instructions` Oggetto JavaScript.
   + __Tipi mime__
      + __Include:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Questi tipi MIME sono gli unici moduli npm del lavoratore. Questo elenco limita i valori elaborati dal lavoratore personalizzato.
      + __Esclusi:__ `Leave blank`
         + Non elaborare mai le risorse con questi tipi MIME utilizzando questa configurazione del servizio. In questo caso, utilizziamo solo un elenco consentiti.
1. Tocca __Salva__ in alto a destra

## Applicare e richiamare un profilo di elaborazione

1. Seleziona il profilo di elaborazione appena creato, `WKND Asset Renditions`
1. Tocca __Applica profilo a cartelle__ nella barra delle azioni superiore
1. Seleziona una cartella a cui applicare il profilo di elaborazione, ad esempio `WKND` e tocca __Applica__
1. Passa alla cartella a cui non è stato applicato il profilo di elaborazione tramite __AEM > Risorse > File__ e tocca in `WKND`.
1. Carica alcune nuove risorse immagini ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg), e [sample-3.jpg](../assets/samples/sample-3.jpg)) in qualsiasi cartella della cartella in cui è applicato il profilo di elaborazione e attendi l’elaborazione della risorsa caricata.
1. Tocca la risorsa per aprirne i dettagli
   + Le rappresentazioni predefinite possono essere generate e visualizzate più rapidamente in AEM rispetto alle rappresentazioni personalizzate.
1. Apri __Rappresentazioni__ vista dalla barra laterale a sinistra
1. Tocca la risorsa denominata `Circle.png` e rivedere la rappresentazione generata

   ![Rappresentazione generata](./assets/processing-profiles/rendition.png)

## Completato!

Congratulazioni. Hai terminato il [esercitazione](../overview.md) su come estendere i microservizi Asset compute as a Cloud Service AEM! Ora dovresti avere la possibilità di configurare, sviluppare, testare, eseguire il debug e distribuire processi di lavoro di Asset compute personalizzati per l’utilizzo da parte del servizio di authoring as a Cloud Service dell’AEM.

### Verifica il codice sorgente completo del progetto su Github

L’ultimo progetto di Asset compute è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains è lo stato finale del progetto, completamente popolato con i casi di lavoro e test, ma non contiene credenziali, ad esempio. `.env`, `.config.json` oppure `.aio`._

## Risoluzione dei problemi

+ [Rappresentazione personalizzata mancante dalla risorsa in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Elaborazione delle risorse non riuscita in AEM](../troubleshooting.md#asset-processing-fails)
