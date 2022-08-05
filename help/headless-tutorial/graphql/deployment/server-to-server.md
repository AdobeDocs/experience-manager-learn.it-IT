---
title: Implementazioni senza intestazione server-to-server AEM
description: Scopri le considerazioni sulla distribuzione per le distribuzioni senza intestazione AEM server-to-server.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Implementazioni senza intestazione server-to-server AEM

Le implementazioni server-to-server headless di AEM includono applicazioni o processi lato server che utilizzano e interagiscono con i contenuti in modo AEM headless.

Le implementazioni server-to-server richiedono una configurazione minima, in quanto le connessioni HTTP a AEM API headless non vengono avviate nel contesto di un browser.

## Configurazioni di distribuzione

La seguente configurazione di distribuzione deve essere in-place per le distribuzioni da server a server delle app.

| L’app da server a server si connette a | Autore AEM | AEM Publish | Anteprima AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri del Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ↓ | ↓ |
| Condivisione delle risorse tra le origini (CORS) | ✘ | ✘ | ✘ |
| [Host AEM](./configurations/aem-hosts.md) | ↓ | ↓ | ↓ |

## Requisiti di autorizzazione

Le richieste autorizzate a AEM API GraphQL si verificano in genere nel contesto di app da server a server, a partire da altri tipi di app, come ad esempio [app a pagina singola](./spa.md), [mobile](./mobile.md)oppure [Componenti web](./web-component.md), in genere l’autorizzazione viene utilizzata in quanto è difficile proteggere le credenziali .

Quando autorizzi le richieste a AEM as a Cloud Service, utilizza [autenticazione token basata sulle credenziali del servizio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Per ulteriori informazioni sull’autenticazione delle richieste a AEM as a Cloud Service, consulta la sezione [esercitazione sull&#39;autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Il tutorial esplora l’autenticazione basata su token utilizzando [API HTTP AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) ma gli stessi concetti e approcci sono applicabili alle app che interagiscono con AEM API GraphQL headless.

## Esempio di app server-to-server

Adobe fornisce un esempio di app server-to-server codificata in Node.js.

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
                   <p class="is-size-6">Un’app server-to-server di esempio, scritta in Node.js, che consuma contenuti da AEM API GraphQL headless.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>