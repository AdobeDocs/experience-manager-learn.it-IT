---
title: Integrare i processi di lavoro di Asset Compute con i Profili elaborazione di AEM
description: AEM as a Cloud Service si integra con i processi di lavoro di Asset Compute implementati in Adobe I/O Runtime tramite i Profili elaborazione di AEM Assets. I Profili di elaborazione sono configurati nel servizio Author in modo da elaborare specifiche risorse tramite processi di lavoro personalizzati e archiviare i file generati da tali processi di lavoro come rappresentazioni delle risorse.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 126
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# Integrare con i profili di elaborazione di AEM

Affinché i processi di lavoro di Asset Compute possano generare rappresentazioni personalizzate in AEM as a Cloud Service, è necessario registrarli nel servizio AEM as a Cloud Service Author tramite Profili di elaborazione. Tutte le risorse soggette a tale profilo di elaborazione avranno il lavoratore richiamato al momento del caricamento o della rielaborazione e avranno la rappresentazione personalizzata generata e resa disponibile tramite le rappresentazioni della risorsa.

## Definire un profilo di elaborazione

Crea innanzitutto un nuovo Profilo di elaborazione che richiamerà il processo di lavoro con i parametri configurabili.

![Elaborazione profilo](./assets/processing-profiles/new-processing-profile.png)

1. Accedi al servizio AEM as a Cloud Service Author come __amministratore AEM__. Trattandosi di un’esercitazione, consigliamo di utilizzare un ambiente di sviluppo o un ambiente in una sandbox.
1. Passa a __Strumenti > Assets > Profili elaborazione__
1. Tocca il pulsante __Crea__
1. Denomina il profilo di elaborazione, `WKND Asset Renditions`
1. Tocca la scheda __Personalizzato__ e tocca __Aggiungi nuovo__
1. Definisci il nuovo servizio
   + __Nome rappresentazione:__ `Circle`
      + Nome file della rappresentazione utilizzato per identificare la rappresentazione in AEM Assets
   + __Estensione:__ `png`
      + Estensione della rappresentazione generata. Impostato su `png` poiché questo è il formato di output supportato dal servizio Web del lavoratore e restituisce uno sfondo trasparente dietro il ritaglio del cerchio.
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + URL del processo di lavoro ottenuto tramite `aio app get-url`. Assicurati che l’URL punti nell’area di lavoro corretta in base all’ambiente AEM as a Cloud Service.
      + Assicurarsi che l&#39;URL del lavoratore punti all&#39;area di lavoro corretta. AEM as a Cloud Service Stage deve utilizzare l’URL dell’area di lavoro di staging e AEM as a Cloud Service Production l’URL dell’area di lavoro di produzione.
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
      + Coppie chiave/valore passate al processo di lavoro Asset Compute e disponibili tramite `rendition.instructions` oggetto JavaScript.
   + __Tipi MIME__
      + __Include:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Questi tipi MIME sono gli unici moduli npm del lavoratore. Questo elenco limita i valori elaborati dal lavoratore personalizzato.
      + __Esclusioni:__ `Leave blank`
         + Non elaborare mai le risorse con questi tipi MIME utilizzando questa configurazione del servizio. In questo caso, utilizziamo solo un elenco consentiti.
1. Tocca __Salva__ in alto a destra

## Applicare e richiamare un profilo di elaborazione

1. Selezionare il profilo di elaborazione appena creato, `WKND Asset Renditions`
1. Tocca __Applica profilo a cartelle__ nella barra delle azioni superiore
1. Seleziona una cartella a cui applicare il profilo di elaborazione, ad esempio `WKND`, e tocca __Applica__
1. Passa alla cartella a cui non è stato applicato il profilo di elaborazione tramite __AEM > Assets > File__ e tocca `WKND`.
1. Carica alcune nuove risorse di immagini ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) e [sample-3.jpg](../assets/samples/sample-3.jpg)) in qualsiasi cartella della cartella con il profilo di elaborazione applicato e attendi l&#39;elaborazione della risorsa caricata.
1. Tocca la risorsa per aprirne i dettagli
   + Le rappresentazioni predefinite possono essere generate e visualizzate più rapidamente in AEM rispetto alle rappresentazioni personalizzate.
1. Apri la visualizzazione __Rappresentazioni__ dalla barra laterale a sinistra
1. Tocca la risorsa denominata `Circle.png` e controlla la rappresentazione generata

   ![Rappresentazione generata](./assets/processing-profiles/rendition.png)

## Finito!

Congratulazioni Hai terminato l&#39;[esercitazione](../overview.md) su come estendere i microservizi AEM as a Cloud Service Asset Compute. Ora dovresti avere la possibilità di configurare, sviluppare, testare, eseguire il debug e distribuire processi di lavoro Asset Compute personalizzati che verranno utilizzati dal servizio AEM as a Cloud Service Author.

### Verifica il codice sorgente completo del progetto su Github

Il progetto finale di Asset Compute è disponibile su Github all’indirizzo:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene è lo stato finale del progetto, popolato completamente con il processo di lavoro e i casi di test, ma non contiene credenziali, ad esempio. `.env`, `.config.json` o `.aio`._

## Risoluzione dei problemi

+ [Rappresentazione personalizzata mancante nella risorsa in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [L’elaborazione delle risorse in AEM non riesce](../troubleshooting.md#asset-processing-fails)
