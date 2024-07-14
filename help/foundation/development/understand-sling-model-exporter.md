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
duration: 133
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Comprendere [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduce [!DNL Sling Model Exporter], un modo elegante per esportare o serializzare [!DNL Sling Model] oggetti in astrazioni personalizzate. Questo articolo giustappone il caso d&#39;uso tradizionale di utilizzo di [!DNL Sling Models] per popolare gli script HTL, sfruttando il framework [!DNL Sling Model Exporter] per serializzare un [!DNL Sling Model] in JSON.

## Flusso di richieste HTTP del modello Sling tradizionale

Il caso d&#39;uso tradizionale per [!DNL Sling Models] consiste nel fornire un&#39;astrazione aziendale per una risorsa o una richiesta, che fornisce script HTL (o in precedenza JSP) un&#39;interfaccia per l&#39;accesso alle funzioni aziendali.

I pattern comuni sono lo sviluppo di [!DNL Sling Models] che rappresentano componenti o pagine AEM e l&#39;utilizzo di oggetti [!DNL Sling Model] per alimentare gli script HTL con i dati, con un risultato finale di HTML visualizzato nel browser.

### Flusso richiesta HTTP modello Sling

![Flusso richiesta modello Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] richiesta effettuata per una risorsa in AEM.

   Esempio: `HTTP GET /content/my-resource.html`

1. In base al valore `sling:resourceType` della risorsa della richiesta, viene risolto lo script appropriato.

1. Lo script adatta la richiesta o la risorsa al [!DNL Sling Model] desiderato.

1. Lo script utilizza l&#39;oggetto [!DNL Sling Model] per generare la rappresentazione HTML.

1. Il HTML generato dallo script viene restituito nella risposta HTTP.

Questo modello tradizionale funziona bene nel contesto della generazione di HTML, in quanto [!DNL Sling Model] può essere facilmente sfruttato tramite HTL. La creazione di dati più strutturati, come JSON o XML, è un’operazione molto più noiosa in quanto HTL non si presta naturalmente alla definizione di questi formati.

## [!DNL Sling Model Exporter] flusso di richieste HTTP

Apache [!DNL Sling Model Exporter] viene fornito con Jackson Exporter fornito da Sling che serializza automaticamente un oggetto [!DNL Sling Model] &quot;ordinario&quot; in JSON. Jackson Exporter, pur essendo configurabile, analizza l&#39;oggetto [!DNL Sling Model] e genera JSON utilizzando qualsiasi metodo &quot;getter&quot; come chiavi JSON, mentre il getter restituisce valori come valori JSON.

La serializzazione diretta di [!DNL Sling Models] consente di soddisfare entrambe le normali richieste Web con le risposte HTML create utilizzando il flusso di richieste [!DNL Sling Model] tradizionale (vedi sopra), ma anche di esporre rappresentazioni JSON che possono essere utilizzate dai servizi Web o dalle applicazioni JavaScript.

![Flusso richiesta HTTP esportazione modello Sling](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Questo flusso descrive il flusso che utilizza il programma di esportazione Jackson fornito per produrre l&#39;output JSON. L&#39;utilizzo di esportatori personalizzati segue lo stesso flusso ma con il relativo formato di output.*

1. Richiesta HTTP GET per una risorsa in AEM con il selettore e l&#39;estensione registrati con l&#39;utilità di esportazione di [!DNL Sling Model].

   Esempio: `HTTP GET /content/my-resource.model.json`

1. Sling risolve il selettore `sling:resourceType` e l&#39;estensione della risorsa richiesta in un servlet di esportazione Sling generato in modo dinamico, mappato a [!DNL Sling Model] con Exporter.
1. Il servlet di esportazione Sling risolto richiama [!DNL Sling Model Exporter] rispetto all&#39;oggetto [!DNL Sling Model] adattato dalla richiesta o dalla risorsa (come determinato dagli adattatori modelli Sling).
1. L&#39;esportatore serializza [!DNL Sling Model] in base alle annotazioni del modello Sling Exporter e alle opzioni Exporter e restituisce il risultato al servlet Sling Exporter.
1. Il servlet di esportazione Sling restituisce il rendering JSON di [!DNL Sling Model] nella risposta HTTP.

>[!NOTE]
>
>Sebbene il progetto Apache Sling fornisca il Jackson Exporter che serializza [!DNL Sling Models] in JSON, il framework Exporter supporta anche gli Exporter personalizzati. Ad esempio, un progetto potrebbe implementare un modulo di esportazione personalizzato che serializza [!DNL Sling Model] in XML.

>[!NOTE]
>
>Non solo [!DNL Sling Model Exporter] *serializza* [!DNL Sling Models], ma può anche esportarli come oggetti Java. L’esportazione in altri oggetti Java non svolge un ruolo nel flusso di richieste HTTP e pertanto non viene visualizzata nel diagramma precedente.

## Materiali di supporto

* [Apache [!DNL Sling Model Exporter] Documentazione framework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
