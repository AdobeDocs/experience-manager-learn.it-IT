---
title: Comprendere Sling Model Exporter nell’AEM
description: Apache Sling Models 1.3.0 introduce Sling Model Exporter, un modo elegante per esportare o serializzare oggetti Sling Model in astrazioni personalizzate. Questo articolo giustappone il caso d’uso tradizionale di utilizzo dei modelli Sling per popolare gli script HTL, con l’utilizzo del framework Sling Model Exporter per serializzare un modello Sling in JSON.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 170
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Comprendere [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduce [!DNL Sling Model Exporter], un modo elegante per esportare o serializzare [!DNL Sling Model] oggetti in astrazioni personalizzate. Questo articolo giustappone il caso d’uso tradizionale dell’utilizzo di [!DNL Sling Models] per popolare gli script HTL, sfruttando la [!DNL Sling Model Exporter] framework per serializzare un [!DNL Sling Model] in JSON.

## Flusso di richieste HTTP del modello Sling tradizionale

Il caso d’uso tradizionale per [!DNL Sling Models] deve fornire un’astrazione aziendale per una risorsa o una richiesta, che fornisce script HTL (o, in precedenza, JSP) un’interfaccia per accedere alle funzioni aziendali.

I pattern comuni si stanno sviluppando [!DNL Sling Models] che rappresentano i componenti o le pagine dell’AEM e che utilizzano [!DNL Sling Model] oggetti per alimentare gli script HTL con i dati, con il risultato finale di HTML visualizzato nel browser.

### Flusso richiesta HTTP modello Sling

![Flusso di richiesta modello Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Richiesta di una risorsa in AEM.

   Esempio: `HTTP GET /content/my-resource.html`

1. In base al valore della risorsa della richiesta `sling:resourceType`, viene risolto lo script appropriato.

1. Lo script adatta la richiesta o la risorsa al [!DNL Sling Model].

1. Lo script utilizza [!DNL Sling Model] per generare la rappresentazione HTML.

1. Il HTML generato dallo script viene restituito nella risposta HTTP.

Questo modello tradizionale funziona bene nel contesto della generazione di HTML come [!DNL Sling Model] può essere facilmente sfruttato tramite HTL. La creazione di dati più strutturati, come JSON o XML, è un’operazione molto più noiosa in quanto HTL non si presta naturalmente alla definizione di questi formati.

## [!DNL Sling Model Exporter] Flusso di richieste HTTP

Apache [!DNL Sling Model Exporter] viene fornito con un Sling fornito Jackson Exporter che serializza automaticamente un &quot;ordinario&quot; [!DNL Sling Model] oggetto in JSON. Il Jackson Exporter, pur essendo abbastanza configurabile, nel suo nucleo ispeziona [!DNL Sling Model] e genera JSON utilizzando qualsiasi metodo &quot;getter&quot; come chiavi JSON; il getter restituisce valori come valori JSON.

La serializzazione diretta di [!DNL Sling Models] consente loro di soddisfare entrambe le normali richieste Web con le risposte HTML create utilizzando il tradizionale [!DNL Sling Model] flusso di richieste (vedi sopra), ma anche esporre rappresentazioni JSON che possono essere utilizzate da servizi web o applicazioni JavaScript.

![Flusso richieste HTTP esportazione modello Sling](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Questo flusso descrive il flusso utilizzando il Jackson Exporter fornito per produrre output JSON. L’utilizzo di esportatori personalizzati segue lo stesso flusso ma con il relativo formato di output.*

1. La richiesta HTTP di GET viene effettuata per una risorsa nell’AEM con il selettore e l’estensione registrati con [!DNL Sling Model]Esportatore di.

   Esempio: `HTTP GET /content/my-resource.model.json`

1. Sling risolve i `sling:resourceType`, selettore ed estensione di un servlet di esportazione Sling generato in modo dinamico, mappato sul [!DNL Sling Model] con Exporter.
1. Il servlet di esportazione Sling risolto richiama [!DNL Sling Model Exporter] contro [!DNL Sling Model] oggetto adattato dalla richiesta o dalla risorsa (come determinato dagli adattabili Sling Models).
1. L&#39;esportatore serializza [!DNL Sling Model] in base alle annotazioni Opzioni di esportazione e Modello Sling specifico per l’esportatore e restituisce il risultato al servlet di esportazione Sling.
1. Il servlet di esportazione Sling restituisce il rendering JSON del [!DNL Sling Model] nella risposta HTTP.

>[!NOTE]
>
>Mentre il progetto Apache Sling fornisce il Jackson Exporter che serializza [!DNL Sling Models] Per JSON, il framework di esportazione supporta anche gli esportatori personalizzati. Ad esempio, un progetto potrebbe implementare un’utilità di esportazione personalizzata che serializza [!DNL Sling Model] in XML.

>[!NOTE]
>
>Non solo [!DNL Sling Model Exporter] *serializzare* [!DNL Sling Models], può anche esportarli come oggetti Java. L’esportazione in altri oggetti Java non svolge un ruolo nel flusso di richieste HTTP e pertanto non viene visualizzata nel diagramma precedente.

## Materiali di supporto

* [Apache [!DNL Sling Model Exporter] Documentazione framework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
