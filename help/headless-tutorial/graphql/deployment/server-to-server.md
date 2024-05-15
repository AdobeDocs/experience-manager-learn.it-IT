---
title: Distribuzioni server-to-server headless AEM
description: Scopri le considerazioni sulla distribuzione per le distribuzioni headless AEM server-to-server.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---

# Distribuzioni server-to-server headless AEM

Le implementazioni headless da server a server dell’AEM coinvolgono applicazioni o processi lato server che utilizzano e interagiscono con i contenuti dell’AEM in modo headless.

Le distribuzioni server-to-server richiedono una configurazione minima, in quanto le connessioni HTTP alle API headless dell’AEM non vengono avviate nel contesto di un browser.

## Configurazioni di distribuzione

La seguente configurazione di distribuzione deve essere implementata per le distribuzioni di app da server a server.

| L’app server-to-server si connette a | Autore AEM | Pubblicazione AEM | Anteprima AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri del Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Condivisione delle risorse tra le origini (CORS) | ✘ | ✘ | ✘ |
| [Host AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Requisiti di autorizzazione

Le richieste autorizzate alle API GraphQL dell’AEM si verificano in genere nel contesto di app server-to-server, poiché altri tipi di app, come [app a pagina singola](./spa.md), [mobile](./mobile.md), o [Componenti Web](./web-component.md), in genere si utilizza l’autorizzazione in quanto è difficile proteggere le credenziali.

Quando si autorizzano richieste a AEM as a Cloud Service, utilizzare [autenticazione token basata su credenziali del servizio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Per ulteriori informazioni sull’autenticazione delle richieste a AEM as a Cloud Service, consulta [tutorial sull’autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Il tutorial esplora l’autenticazione basata su token utilizzando [API HTTP di AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) ma gli stessi concetti e approcci sono applicabili alle app che interagiscono con le API GraphQL headless dell’AEM.

## Esempio di app server-to-server

Un Adobe fornisce un’app server-to-server codificata in Node.js.

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="App server-to-server" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="App server-to-server">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="App server-to-server">App server-to-server</a></p>
                   <p class="is-size-6">Un esempio di app server-to-server, scritta in Node.js, che utilizza contenuti dalle API GraphQL headless dell’AEM.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
