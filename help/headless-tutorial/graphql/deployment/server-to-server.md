---
title: Distribuzioni AEM Headless server-to-server
description: Scopri le considerazioni sulla distribuzione per le distribuzioni AEM headless server-to-server.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---

# Distribuzioni AEM Headless server-to-server

Le implementazioni AEM Headless server-to-server coinvolgono applicazioni o processi lato server che utilizzano e interagiscono con i contenuti in AEM in modo headless.

Le distribuzioni server-to-server richiedono una configurazione minima, in quanto le connessioni HTTP alle API AEM Headless non vengono avviate nel contesto di un browser.

## Configurazioni di distribuzione

La seguente configurazione di distribuzione deve essere implementata per le distribuzioni di app da server a server.

| L’app server-to-server si connette a → | AEM Author | AEM Publish | Anteprima AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Condivisione delle risorse tra le origini (CORS) | ✘ | ✘ | ✘ |
| [Host di AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Requisiti di autorizzazione

Le richieste autorizzate alle API di AEM GraphQL si verificano in genere nel contesto di app server-to-server, poiché altri tipi di app, ad esempio [app a pagina singola](./spa.md), [dispositivi mobili](./mobile.md) o [Componenti Web](./web-component.md), in genere utilizzano l&#39;autorizzazione in quanto è difficile proteggere le credenziali.

Quando si autorizzano le richieste ad AEM as a Cloud Service, utilizzare l&#39;autenticazione token basata sulle credenziali del servizio [](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Per ulteriori informazioni sull&#39;autenticazione delle richieste ad AEM as a Cloud Service, consulta l&#39;[esercitazione sull&#39;autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Il tutorial esplora l&#39;autenticazione basata su token utilizzando [le API HTTP di AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html), ma gli stessi concetti e approcci sono applicabili alle app che interagiscono con le API GraphQL headless di AEM.

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
                   <p class="is-size-6">Un esempio di app server-to-server, scritta in Node.js, che utilizza contenuti dalle API GraphQL headless di AEM.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
