---
title: Modifica delle app React tramite Editor universale
description: Scopri come modificare il contenuto di un’app React di esempio utilizzando l’editor universale.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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
   - **AEM SDK**: fornisce i contenuti archiviati nei frammenti di contenuto per l&#39;app React utilizzando le API di GraphQL.
   - **React app**: interfaccia utente semplice che visualizza il contenuto dell&#39;AEM.
   - **Universal Editor Service**: una _copia locale del servizio Universal Editor_ che associa Universal Editor e l&#39;SDK AEM.
- Come dotare l’app React per modificare i contenuti con l’editor universale
- Come modificare i contenuti dell’app React utilizzando l’editor universale


## Panoramica dell’editor universale

L’editor universale consente ad autori e sviluppatori di contenuti (front-end e back-end), esaminiamo alcuni dei vantaggi chiave dell’editor universale:

- Progettato per modificare contenuti headless e headful in modalità &quot;ciò che vedi è ciò che ricevi&quot; (WYSIWYG).
- Offre un’esperienza di modifica dei contenuti coerente su diverse tecnologie front-end come React, Angular, Vue, ecc. Pertanto, gli autori dei contenuti possono modificarli senza preoccuparsi della tecnologia front-end sottostante.
- È necessaria una strumentazione minima per abilitare Universal Editor nell’applicazione front-end. In questo modo, massimizza la produttività dello sviluppatore e li libera di concentrarsi sulla creazione dell’esperienza.
- Separazione delle preoccupazioni tra tre ruoli: autori di contenuti, sviluppatori front-end e sviluppatori back-end, consentendo a ogni ruolo di concentrarsi sulle proprie responsabilità di base.


## App React di esempio

Questa esercitazione utilizza [**Team WKND**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) come app React di esempio. L&#39;app React **WKND Teams** visualizza un elenco dei membri del team e i relativi dettagli.

I dettagli del team come titolo, descrizione e membri del team sono archiviati come _Frammenti di contenuto del team_ in AEM. Allo stesso modo, i dettagli della persona come nome, biografia e immagine del profilo vengono memorizzati come _Frammenti di contenuto della persona_ nell&#39;AEM.

Il contenuto dell&#39;app React viene fornito dall&#39;AEM tramite le API GraphQL e l&#39;interfaccia utente viene creata utilizzando due componenti React, `Teams` e `Person`.

È disponibile un tutorial corrispondente per scoprire come creare l&#39;app React **WKND Teams**. L&#39;esercitazione [è disponibile qui](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Passaggio successivo

Scopri come [configurare l&#39;ambiente di sviluppo locale](./local-development-setup.md).
