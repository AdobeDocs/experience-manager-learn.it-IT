---
title: Modifica delle app React tramite l’editor universale
description: Scopri come modificare i contenuti di un’app React di esempio utilizzando l’editor universale.
short-description: Scopri come modificare i contenuti di un’app React di esempio utilizzando l’editor universale. In AEM, i contenuti vengono memorizzati all’interno dei Frammenti di contenuto e recuperati utilizzando le API di GraphQL.
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 100%

---

# Modifica delle app React tramite l’editor universale

Scopri come modificare i contenuti di un’app React di esempio utilizzando l’editor universale. In AEM, i contenuti vengono memorizzati all’interno dei Frammenti di contenuto e recuperati utilizzando le API di GraphQL.

Questo tutorial ti guida attraverso il processo di configurazione dell’ambiente di sviluppo locale, la preparazione dell’app React per modificarne il contenuto e la modifica del contenuto tramite l’editor universale.

## Argomenti trattati

Questo tutorial tratta i seguenti argomenti:

- Una breve panoramica sull’editor universale
- Configurazione dell’ambiente di sviluppo locale
   - **AEM SDK**: fornisce i contenuti archiviati nei Frammenti di contenuto per l’app React utilizzando le API di GraphQL.
   - **App React**: un’interfaccia utente semplice che visualizza i contenuti di AEM.
   - **Servizio editor universale**: una _copia locale del servizio editor universale_ che associa tale editor all’SDK di AEM.
- Come preparare l’app React per modificare i contenuti con l’editor universale
- Come modificare i contenuti dell’app React utilizzando l’editor universale


## Panoramica dell’editor universale

L’editor universale potenzia il lavoro di autori e sviluppatori di contenuti (front-end e back-end). Ecco alcuni dei vantaggi chiave dell’editor universale:

- Progettato per modificare contenuti headless e headful in modalità WYSIWYG (what-you-see-is-what-you-get).
- Offre un’esperienza di modifica dei contenuti coerente su diverse tecnologie front-end come React, Angular, Vue, ecc. In questo modo, gli autori dei contenuti possono modificarli senza alcuna preoccupazione relativa alla tecnologia front-end sottostante.
- L’abilitazione dell’editor universale nell’applicazione front-end necessita di una preparazione minima. Di conseguenza, la produttività degli sviluppatori verrà ottimizzata, consentendo loro di concentrarsi sulla creazione dell’esperienza.
- Separazione delle competenze fra tre ruoli: autori di contenuti, sviluppatori front-end e sviluppatori back-end, consentendo a ciascun ruolo di concentrarsi sulle proprie responsabilità di base.


## App React di esempio

Questo tutorial utilizza [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) come app React di esempio. L’app React **WKND Teams** visualizza un elenco dei membri del team e i relativi dettagli.

I dettagli relativi al team come titolo, descrizione e membri del team sono memorizzati come Frammenti di contenuto _team_ in AEM. Allo stesso modo, i dettagli della persona come nome, biografia e immagine del profilo vengono memorizzati come Frammenti di contenuto _persona_ in AEM.

Il contenuto per l’app React viene fornito da AEM tramite le API GraphQL e l’interfaccia utente viene creata utilizzando due componenti React, `Teams` e `Person`.

Per scoprire come creare l’app React **WKND Teams**, è disponibile un tutorial corrispondente. Il tutorial è disponibile [qui](https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Passaggio successivo

Scopri come [configurare l’ambiente di sviluppo locale](./local-development-setup.md).
