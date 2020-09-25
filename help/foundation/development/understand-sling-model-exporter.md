---
title: Comprendere l'Esportatore di modelli Sling in AEM
description: Apache Sling Models 1.3.0 introduce Sling Model Exporter, un modo elegante per esportare o serializzare oggetti Sling Model in astrazioni personalizzate. Questo articolo illustra il caso d’uso tradizionale dell’utilizzo di Sling Models per compilare gli script HTL, sfruttando il framework Sling Model Exporter per serializzare un modello Sling in JSON.
version: 6.3, 6.4, 6.5
sub-product: servizi di base, content-services
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 0%

---


# Comprendere [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduce [!DNL Sling Model Exporter], un modo elegante per esportare o serializzare [!DNL Sling Model] gli oggetti in astrazioni personalizzate. In questo articolo viene confrontato il caso d’uso tradizionale [!DNL Sling Models] per la compilazione degli script HTL, con l’utilizzo del [!DNL Sling Model Exporter] framework per la serializzazione di un [!DNL Sling Model] file JSON.

## Flusso di richieste HTTP per il modello Sling tradizionale

Il caso d&#39;uso tradizionale per [!DNL Sling Models] è quello di fornire un&#39;astrazione aziendale per una risorsa o una richiesta, che fornisce script HTL (o, in precedenza, JSP) un&#39;interfaccia per l&#39;accesso alle funzioni aziendali.

Si stanno sviluppando pattern comuni [!DNL Sling Models] che rappresentano componenti AEM o pagine e utilizzano gli [!DNL Sling Model] oggetti per alimentare gli script HTL con i dati, con un risultato finale di HTML visualizzato nel browser.

### Flusso di richieste HTTP per modello Sling

![Flusso richiesta modello Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Viene richiesta una risorsa in AEM.

   Esempio: `HTTP GET /content/my-resource.html`

1. In base alla risorsa richiesta `sling:resourceType`, viene risolto lo script appropriato.

1. Lo script adatta la richiesta o la risorsa alla richiesta [!DNL Sling Model].

1. Lo script utilizza l&#39; [!DNL Sling Model] oggetto per generare la rappresentazione HTML.

1. Il codice HTML generato dallo script viene restituito nella risposta HTTP.

Questo pattern tradizionale funziona bene nel contesto della generazione di HTML, in quanto [!DNL Sling Model] può essere facilmente sfruttato tramite HTL. La creazione di dati più strutturati come JSON o XML è un&#39;impresa molto più tediosa in quanto HTL non si presta naturalmente alla definizione di questi formati.

## [!DNL Sling Model Exporter] Flusso di richieste HTTP

Apache [!DNL Sling Model Exporter] viene fornito con un Sling fornito Jackson Exporter che serializza automaticamente un [!DNL Sling Model] oggetto &quot;ordinario&quot; in JSON. Jackson Exporter, benché configurabile, al suo centro controlla l&#39; [!DNL Sling Model] oggetto e genera JSON utilizzando qualsiasi metodo &quot;getter&quot; come chiavi JSON, e il getter restituisce i valori come valori JSON.

La serializzazione diretta di [!DNL Sling Models] consente loro di soddisfare entrambe le normali richieste Web con le risposte HTML create utilizzando il flusso di [!DNL Sling Model] richieste tradizionale (vedi sopra), ma anche di esporre le rappresentazioni JSON che possono essere utilizzate dai servizi Web o dalle applicazioni JavaScript.

![Sling Model Exporter HTTP Richiesta Flusso](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Questo flusso descrive il flusso utilizzando Jackson Exporter fornito per produrre l&#39;output JSON. L’utilizzo di esportatori personalizzati segue lo stesso flusso ma con il relativo formato di output.*

1. Richiesta GET HTTP per una risorsa in AEM con il selettore e l’estensione registrati con l’ [!DNL Sling Model]Esportatore.

   Esempio: `HTTP GET /content/my-resource.model.json`

1. Sling risolve il selettore `sling:resourceType`e l’estensione della risorsa richiesta a un Servlet Sling Exporter generato in modo dinamico, mappato sul [!DNL Sling Model] server con Exporter.
1. Il servlet Sling Exporter risolto richiama l&#39;oggetto [!DNL Sling Model Exporter] rispetto all&#39; [!DNL Sling Model] oggetto adattato dalla richiesta o risorsa (come determinato dagli adattatori Sling Models).
1. L&#39;esportatore serializza il modello [!DNL Sling Model] in base alle opzioni di esportazione e alle annotazioni del modello Sling specifiche per l&#39;esportatore e restituisce il risultato al Servlet Sling Exporter.
1. Il Servlet Sling Exporter restituisce la rappresentazione JSON dell’ [!DNL Sling Model] oggetto nella risposta HTTP.

>[!NOTE]
>
>Mentre il progetto Apache Sling fornisce Jackson Exporter che esegue la serializzazione [!DNL Sling Models] su JSON, il framework Exporter supporta anche gli Exporter personalizzati. Ad esempio, un progetto potrebbe implementare un Esportatore personalizzato che serializza un [!DNL Sling Model] file XML.

>[!NOTE]
>
>Non solo [!DNL Sling Model Exporter] serializza ** [!DNL Sling Models], ma può anche esportarli come oggetti Java. L&#39;esportazione in altri oggetti Java non ha alcun ruolo nel flusso di richieste HTTP e non viene quindi visualizzata nel diagramma precedente.

## Materiali di supporto

* [Documentazione [!DNL Sling Model Exporter] ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
