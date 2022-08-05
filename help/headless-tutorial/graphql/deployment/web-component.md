---
title: Distribuzione di componenti web headless AEM
description: Scopri le considerazioni sulla distribuzione per le distribuzioni senza titolo basate su componenti web/AEM basate su JS.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---


# Distribuzione di componenti web headless AEM

AEM senza testa [Componente Web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)Le implementazioni /JS sono semplici app JavaScript eseguite in un browser web che utilizzano e interagiscono con i contenuti in AEM in modo headless. Le distribuzioni di componenti web/JS differiscono da [Implementazioni SPA](./spa.md) in quanto non utilizzano un solido framework di SPA, e si prevede che saranno incorporati nel contesto di qualsiasi sito web, consegnati, per far emergere contenuti da AEM.


## Configurazioni di distribuzione

La seguente configurazione di distribuzione deve essere in-place per le distribuzioni di componenti Web/JS.

| L’app Web Component/JS si connette a | Autore AEM | AEM Publish | Anteprima AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri del Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ↓ | ↓ |
| [Condivisione delle risorse tra le origini (CORS)](./configurations/cors.md) | ↓ | ↓ | ↓ |
| [Host AEM](./configurations/aem-hosts.md) | ↓ | ↓ | ↓ |

## Esempio di componente Web

Adobe fornisce un esempio di componente Web.

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
                   <p class="is-size-6">Un componente Web di esempio, scritto in JavaScript puro, che consuma contenuti dalle API GraphQL headless AEM.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
