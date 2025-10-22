---
title: Tutorial su AEM Headless OpenAPI | Consegna dei frammenti di contenuto
description: Un tutorial end-to-end che illustra come creare ed esporre contenuti utilizzando le API di distribuzione dei frammenti di contenuto basate su OpenAPI di AEM.
short-description: Un tutorial che illustra come creare ed esporre contenuti AEM utilizzando Content Fragment Delivery (Distribuzione di frammenti di contenuto) con API OpenAPI e utilizzarli in un’app esterna per scenari CMS headless.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
exl-id: 1bb7c415-58f8-4f6c-a0bc-38bdbdb521cf
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 20%

---

# Introduzione alla distribuzione dei frammenti di contenuto in AEM con API OpenAPI

Esplora questo tutorial che illustra come creare ed esporre contenuti AEM utilizzando la funzione Content Fragment Delivery di AEM con API OpenAPI e utilizzati da un’app esterna, in uno scenario CMS headless. Questo esplora questi concetti camminando ha pensato la creazione di un’app React che mostra i team WKND e i dettagli dei membri associati. I team e i membri vengono modellati utilizzando i modelli per frammenti di contenuto di AEM e utilizzati dall’app React tramite la Distribuzione di frammenti di contenuto di AEM con API OpenAPI.

![App team WKND](./assets/overview/main.png)

Questo tutorial tratta i seguenti argomenti:

* Creare una configurazione del progetto
* Creare modelli per frammenti di contenuto per modellare i dati
* Creare frammenti di contenuto basati sui modelli creati in precedenza
* Scopri come eseguire query sui frammenti di contenuto in AEM utilizzando la funzione &quot;Try It&quot; (Prova) della documentazione sulla distribuzione di frammenti di contenuto di AEM con API OpenAPI
* Consumare dati dei frammenti di contenuto tramite AEM Content Fragment Delivery con chiamate API OpenAPI da un’app React di esempio
* Migliorare l’app React per renderla modificabile nell’editor universale

## Prerequisiti {#prerequisites}

Per seguire questo tutorial sono necessari i seguenti elementi:

* AEM Sites as a Cloud Service
* Competenze di base di HTML e JavaScript
* È necessario installare localmente i seguenti strumenti:
   * [Node.js v22+](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Un IDE (ad esempio, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### Ambiente AEM as a Cloud Service

Per completare questa esercitazione, ti consigliamo di avere l&#39;accesso **Amministratore AEM** a un ambiente AEM as a Cloud Service. È inoltre possibile utilizzare un ambiente **Sviluppo**, **Ambiente di sviluppo rapido** o un ambiente in un **Programma sandbox**.

## Iniziamo.

Inizia il tutorial con la sezione [Definizione dei modelli per frammenti di contenuto](1-content-fragment-models.md).

## Progetto GitHub

Il codice sorgente e i pacchetti di contenuti sono disponibili nelle [esercitazioni AEM Headless](https://github.com/adobe/aem-tutorials) archivio GitHub.

Il ramo [`main` contiene il codice sorgente finale](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) per questa esercitazione.
Le istantanee del codice alla fine di ciascun passaggio sono disponibili come tag Git.

* Inizio del capitolo 4 - App di React: [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* Fine del capitolo 4 - App React: [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* Fine del capitolo 5 - Editor universale: [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

Se riscontri un problema con il tutorial o con il codice, crea un [problema GitHub](https://github.com/adobe/aem-tutorials/issues).
