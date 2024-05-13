---
title: Modifica delle app React tramite Editor universale
description: Scopri come modificare il contenuto di un’app React di esempio utilizzando l’editor universale.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
source-git-commit: 14767141348d3d56c154704cc21d39722bb67aec
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---


# Modifica delle app React tramite Editor universale

Scopri come modificare il contenuto di un’app React di esempio utilizzando l’editor universale. I contenuti vengono memorizzati all’interno di Frammenti di contenuto in AEM e vengono recuperati utilizzando le API di GraphQL.

Questa esercitazione ti guida attraverso il processo di configurazione dell’ambiente di sviluppo locale, la strumentazione dell’app React per modificarne il contenuto e la modifica del contenuto tramite l’editor universale.

## Cosa impara

Questo tutorial tratta i seguenti argomenti:

- Breve panoramica di Universal Editor
- Configurazione dell’ambiente di sviluppo locale
   - **SDK AEM**: fornisce i contenuti memorizzati all’interno dei frammenti di contenuto per l’app React utilizzando le API di GraphQL.
   - **React app**: una semplice interfaccia utente che visualizza il contenuto dell’AEM.
   - **Servizio editor universale**: a _copia locale del servizio Universal Editor_ che associa Universal Editor e l’SDK dell’AEM.
- Come dotare l’app React per modificare i contenuti con l’editor universale
- Come modificare i contenuti dell’app React utilizzando l’editor universale


## Panoramica dell’editor universale

L’editor universale consente ad autori e sviluppatori di contenuti (front-end e back-end), esaminiamo alcuni dei vantaggi chiave dell’editor universale:

- Progettato per modificare contenuti headless e headful in modalità &quot;ciò che vedi è ciò che ricevi&quot; (WYSIWYG).
- Offre un’esperienza di modifica dei contenuti coerente su diverse tecnologie front-end come React, Angular, Vue, ecc. Pertanto, gli autori dei contenuti possono modificarli senza preoccuparsi della tecnologia front-end sottostante.
- È necessaria una strumentazione minima per abilitare Universal Editor nell’applicazione front-end. In questo modo, massimizza la produttività dello sviluppatore e li libera di concentrarsi sulla creazione dell’esperienza.
- Separazione delle preoccupazioni tra tre ruoli: autori di contenuti, sviluppatori front-end e sviluppatori back-end, consentendo a ogni ruolo di concentrarsi sulle proprie responsabilità di base.


## App React di esempio

Questa esercitazione utilizza [**Team WKND**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) come app React di esempio. Il **Team WKND** L’app React visualizza un elenco dei membri del gruppo e dei relativi dettagli.

I dettagli del team come titolo, descrizione e membri del team vengono memorizzati come _Team_ Frammenti di contenuto nell’AEM. Allo stesso modo, i dettagli della persona come nome, biografia e immagine del profilo vengono memorizzati come _Persona_ Frammenti di contenuto nell’AEM.

Il contenuto dell’app React è fornito dall’AEM utilizzando le API GraphQL e l’interfaccia utente è creata utilizzando due componenti React, `Teams` e `Person`.

È disponibile un tutorial corrispondente per scoprire come creare **Team WKND** React app. Puoi trovare l’esercitazione [qui](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Passaggio successivo

Scopri come [configurare l’ambiente di sviluppo locale](./local-development-setup.md).
