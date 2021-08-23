---
title: Comprendere l’esportatore di modelli Sling in AEM
description: Apache Sling Models 1.3.0 introduce Sling Model Exporter, un modo elegante per esportare o serializzare gli oggetti Sling Model in astrazioni personalizzate. Questo articolo sovrappone il tradizionale caso d’uso dell’utilizzo di modelli Sling per popolare gli script HTL, con l’utilizzo del framework Sling Model Exporter per serializzare un modello Sling in JSON.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: API
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Sviluppo
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%

---


# Comprendere [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduce [!DNL Sling Model Exporter], un modo elegante per esportare o serializzare gli oggetti [!DNL Sling Model] in astrazioni personalizzate. Questo articolo sovrappone il tradizionale caso d’uso dell’utilizzo di [!DNL Sling Models] per popolare gli script HTL, sfruttando il framework [!DNL Sling Model Exporter] per serializzare un [!DNL Sling Model] in JSON.

## Flusso di richieste HTTP del modello Sling tradizionale

Il caso d’uso tradizionale di [!DNL Sling Models] è quello di fornire un’astrazione aziendale per una risorsa o una richiesta, che fornisce script HTL (o, in precedenza, JSP) un’interfaccia per accedere alle funzioni aziendali.

I pattern comuni stanno sviluppando [!DNL Sling Models] che rappresentano i componenti o le pagine AEM e utilizzano gli oggetti [!DNL Sling Model] per alimentare gli script HTL con i dati, con un risultato finale di HTML visualizzato nel browser.

### Flusso di richieste HTTP per modello Sling

![Flusso di richieste modello Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] La richiesta viene effettuata per una risorsa in AEM.

   Esempio: `HTTP GET /content/my-resource.html`

1. In base al `sling:resourceType` della risorsa della richiesta, lo script appropriato viene risolto.

1. Lo script adatta la richiesta o la risorsa alla [!DNL Sling Model] desiderata.

1. Lo script utilizza l&#39;oggetto [!DNL Sling Model] per generare il rendering HTML.

1. Il codice HTML generato dallo script viene restituito nella risposta HTTP.

Questo modello tradizionale funziona bene nel contesto della generazione di HTML, in quanto il [!DNL Sling Model] può essere facilmente sfruttato tramite HTL. Creare dati più strutturati come JSON o XML è un’impresa molto più noiosa in quanto HTL non si presta naturalmente alla definizione di questi formati.

## [!DNL Sling Model Exporter] Flusso di richieste HTTP

Apache [!DNL Sling Model Exporter] viene fornito con uno Sling fornito Jackson Exporter che serializza automaticamente un oggetto &quot;ordinario&quot; [!DNL Sling Model] in JSON. L&#39;esportatore Jackson, pur essendo abbastanza configurabile, al suo centro controlla l&#39;oggetto [!DNL Sling Model] e genera JSON utilizzando qualsiasi metodo &quot;getter&quot; come chiave JSON e i valori restituiti get come valori JSON.

La serializzazione diretta di [!DNL Sling Models] consente loro di soddisfare entrambe le normali richieste web con le risposte HTML create utilizzando il flusso di richieste [!DNL Sling Model] tradizionale (vedi sopra), ma anche di esporre le rappresentazioni JSON che possono essere utilizzate da servizi web o applicazioni JavaScript.

![Flusso di richieste HTTP di Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Questo flusso descrive il flusso utilizzando l&#39;esportatore Jackson fornito per produrre l&#39;output JSON. L&#39;uso di esportatori personalizzati segue lo stesso flusso ma con il loro formato di output.*

1. HTTP GET Request viene fatto per una risorsa in AEM con il selettore e l’estensione registrati con l’esportatore di [!DNL Sling Model].

   Esempio: `HTTP GET /content/my-resource.model.json`

1. Sling risolve il selettore e l’estensione della risorsa richiesta `sling:resourceType` in un servlet Sling Exporter generato in modo dinamico, mappato su [!DNL Sling Model] con Exporter.
1. Il servlet Sling Exporter risolto richiama il [!DNL Sling Model Exporter] rispetto all&#39;oggetto [!DNL Sling Model] adattato dalla richiesta o dalla risorsa (come determinato dalle tabelle di adattamento dei modelli Sling).
1. L’esportatore serializza il [!DNL Sling Model] in base alle opzioni di esportazione e alle annotazioni del modello Sling specifiche per l’esportatore e restituisce il risultato al servlet Sling Exporter.
1. Il servlet Sling Exporter restituisce il rendering JSON del [!DNL Sling Model] nella risposta HTTP.

>[!NOTE]
>
>Mentre il progetto Apache Sling fornisce l&#39;esportatore Jackson che serializza [!DNL Sling Models] a JSON, il framework Exporter supporta anche gli esportatori personalizzati. Ad esempio, un progetto potrebbe implementare un esportatore personalizzato che serializza un [!DNL Sling Model] in XML.

>[!NOTE]
>
>Non solo [!DNL Sling Model Exporter] *serialize* [!DNL Sling Models], può anche esportarli come oggetti Java. L’esportazione in altri oggetti Java non svolge un ruolo nel flusso di richieste HTTP e quindi non viene visualizzata nel diagramma precedente.

## Materiali di supporto

* [ [!DNL Sling Model Exporter] Documentazione di ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
