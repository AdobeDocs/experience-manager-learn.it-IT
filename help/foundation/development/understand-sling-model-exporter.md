---
title: Comprendere l’esportatore di modelli Sling in AEM
description: Apache Sling Models 1.3.0 introduce Sling Model Exporter, un modo elegante per esportare o serializzare gli oggetti Sling Model in astrazioni personalizzate. Questo articolo sovrappone il tradizionale caso d’uso dell’utilizzo di modelli Sling per popolare gli script HTL, con l’utilizzo del framework Sling Model Exporter per serializzare un modello Sling in JSON.
version: 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 1%

---

# Comprendere [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduce [!DNL Sling Model Exporter], un modo elegante di esportare o serializzare [!DNL Sling Model] oggetti in astrazioni personalizzate. Il presente articolo è corredato del caso d&#39;uso tradizionale di [!DNL Sling Models] per popolare gli script HTL, utilizzando [!DNL Sling Model Exporter] per serializzare un [!DNL Sling Model] in JSON.

## Flusso di richieste HTTP del modello Sling tradizionale

L&#39;uso tradizionale per [!DNL Sling Models] fornisce un&#39;astrazione aziendale per una risorsa o una richiesta, che fornisce script HTL (o, in precedenza, JSP) un&#39;interfaccia per l&#39;accesso alle funzioni aziendali.

Si sviluppano modelli comuni [!DNL Sling Models] che rappresentano AEM componenti o pagine e utilizzano [!DNL Sling Model] oggetti per alimentare gli script HTL con i dati, con il risultato finale di HTML visualizzato nel browser.

### Flusso di richieste HTTP per modello Sling

![Flusso di richieste modello Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] La richiesta viene effettuata per una risorsa in AEM.

   Esempio: `HTTP GET /content/my-resource.html`

1. In base alla risorsa della richiesta `sling:resourceType`, lo script appropriato viene risolto.

1. Lo script adatta la richiesta o la risorsa alla richiesta desiderata [!DNL Sling Model].

1. Lo script utilizza il [!DNL Sling Model] oggetto per generare il rendering di HTML.

1. Il HTML generato dallo script viene restituito nella risposta HTTP.

Questo modello tradizionale funziona bene nel contesto della generazione di HTML come [!DNL Sling Model] può essere facilmente sfruttato tramite HTL. Creare dati più strutturati come JSON o XML è un’impresa molto più noiosa in quanto HTL non si presta naturalmente alla definizione di questi formati.

## [!DNL Sling Model Exporter] Flusso di richieste HTTP

Apache [!DNL Sling Model Exporter] viene fornito con uno Sling fornito Jackson Exporter che serializza automaticamente un &quot;ordinario&quot; [!DNL Sling Model] in JSON. L&#39;Esportatore Jackson, pur essendo abbastanza configurabile, al suo nucleo ispeziona il [!DNL Sling Model] e genera JSON utilizzando qualsiasi metodo &quot;getter&quot; come chiave JSON e i valori get restituiti come valori JSON.

La serializzazione diretta dei [!DNL Sling Models] consente loro di soddisfare entrambe le normali richieste Web con le risposte HTML create utilizzando [!DNL Sling Model] flusso di richieste (vedi sopra), ma espone anche le rappresentazioni JSON che possono essere utilizzate dai servizi web o dalle applicazioni JavaScript.

![Flusso di richieste HTTP di Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Questo flusso descrive il flusso utilizzando l&#39;esportatore Jackson fornito per produrre l&#39;output JSON. L’uso di esportatori personalizzati segue lo stesso flusso ma con il loro formato di output.*

1. HTTP GET Request viene fatto per una risorsa in AEM con il selettore e l’estensione registrati con [!DNL Sling Model]Esportatore.

   Esempio: `HTTP GET /content/my-resource.model.json`

1. Sling risolve la risorsa richiesta `sling:resourceType`, selettore ed estensione di un servlet Sling Exporter generato dinamicamente, mappato su [!DNL Sling Model] con Esportatore.
1. Il servlet Sling Exporter risolto richiama il [!DNL Sling Model Exporter] contro [!DNL Sling Model] oggetto adattato dalla richiesta o dalla risorsa (come determinato dalle adattabili Sling Models).
1. L&#39;esportatore serializza il [!DNL Sling Model] in base alle annotazioni Opzioni esportazione e Modello Sling specifico per l&#39;esportatore e restituisce il risultato al servlet Sling Exporter.
1. Il servlet Sling Exporter restituisce il rendering JSON del [!DNL Sling Model] nella risposta HTTP.

>[!NOTE]
>
>Mentre il progetto Apache Sling fornisce l&#39;esportatore Jackson che serializza [!DNL Sling Models] per JSON, il framework Exporter supporta anche gli esportatori personalizzati. Ad esempio, un progetto potrebbe implementare un esportatore personalizzato che serializza un [!DNL Sling Model] in XML.

>[!NOTE]
>
>Non solo [!DNL Sling Model Exporter] *serializzare* [!DNL Sling Models], può anche esportarli come oggetti Java. L’esportazione in altri oggetti Java non svolge un ruolo nel flusso di richieste HTTP e quindi non viene visualizzata nel diagramma precedente.

## Materiali di supporto

* [Apache [!DNL Sling Model Exporter] Documentazione del framework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
