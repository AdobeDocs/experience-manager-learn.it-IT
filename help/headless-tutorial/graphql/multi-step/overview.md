---
title: Guida introduttiva all’esercitazione pratica sull’AEM - GraphQL
description: Un tutorial end-to-end che illustra come creare ed esporre contenuti utilizzando le API GraphQL dell’AEM.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
duration: 81
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 3%

---

# Guida introduttiva ad AEM Headless: GraphQL

{{aem-headless-trials-promo}}

Un tutorial end-to-end che illustra come creare ed esporre contenuti utilizzando le API GraphQL dell’AEM e utilizzati da un’app esterna, in uno scenario CMS headless.

Questo tutorial esplora come le API GraphQL dell’AEM e le funzionalità headless possono essere utilizzate per potenziare le esperienze emerse in un’app esterna.

Questo tutorial tratta i seguenti argomenti:

* Creare una configurazione di progetto
* Creare modelli per frammenti di contenuto per modellare i dati
* Crea frammenti di contenuto in base ai modelli creati in precedenza.
* Scopri come è possibile eseguire query sui frammenti di contenuto nell’AEM utilizzando lo strumento di sviluppo GraphiQL integrato.
* Per memorizzare o rendere persistenti le query GraphQL nell’AEM
* Utilizzare query GraphQL persistenti da un&#39;app React di esempio

## Prerequisiti {#prerequisites}

Per seguire questa esercitazione sono necessari i seguenti elementi:

* Competenze di base in ambito HTML e JavaScript
* I seguenti strumenti devono essere installati localmente:
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Un IDE (ad esempio, [Codice Microsoft® Visual Studio](https://code.visualstudio.com/))

### Ambiente AEM

Per completare questa esercitazione, si consiglia di accedere come amministratore AEM a un ambiente as a Cloud Service AEM. Se non hai accesso all’ambiente AEM as a Cloud Service, puoi utilizzare [SDK Quickstart as a Cloud Service per AEM locale](/help/cloud-service/local-development-environment/aem-runtime.md). Tuttavia, è importante notare che alcune schermate dell’interfaccia utente del prodotto, come la navigazione per frammenti di contenuto, sono diverse.

## Iniziamo!

Inizia l’esercitazione con [Definizione dei modelli per frammenti di contenuto](content-fragment-models.md).

## Progetto GitHub

Il codice sorgente e i pacchetti di contenuti sono disponibili nel [Guide AEM - Progetto WKND GraphQL GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

Se riscontri un problema con l’esercitazione o con il codice, lascia un [Problema GitHub](https://github.com/adobe/aem-guides-wknd-graphql/issues).
