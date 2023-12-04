---
title: Distribuzioni dei componenti web headless AEM
description: Scopri le considerazioni sulla distribuzione per le distribuzioni headless AEM basate su Componente web/JS pure.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 68
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 2%

---

# Distribuzioni dei componenti web headless AEM

AEM headless [Componente Web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)Le implementazioni /JS sono app JavaScript pure che vengono eseguite in un browser web e che utilizzano e interagiscono con i contenuti dell’AEM in modo headless. Le distribuzioni di Componente Web/JS sono diverse da [Distribuzioni SPA](./spa.md) in quanto non utilizzano un solido framework SPA, e ci si aspetta che siano incorporati nel contesto di qualsiasi sito web, consegnino, trasmettano contenuti dall&#39;AEM.


## Configurazioni di distribuzione

La seguente configurazione di distribuzione deve essere implementata per le distribuzioni di Componente Web/JS.

| Componente web/app JS si connette a | Autore AEM | AEM Publish | Anteprima AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri del Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Condivisione delle risorse tra le origini (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Host AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Esempio di componente Web

In questo Adobe viene illustrato un componente Web.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Componente Web" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Componente Web">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Componente Web">Componente Web</a></p>
                   <p class="is-size-6">Componente web di esempio, scritto in JavaScript puro, che utilizza contenuti delle API GraphQL headless dell’AEM.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
