---
title: Distribuzioni dei componenti web headless di AEM
description: Scopri le considerazioni sulla distribuzione per le distribuzioni headless di AEM basate su Componente web/JS puro.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 7%

---

# Distribuzioni dei componenti web headless di AEM

Le distribuzioni del [componente Web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS headless AEM sono app JavaScript pure eseguite in un browser web che utilizzano e interagiscono con i contenuti in AEM in modo headless. Le distribuzioni di componenti Web/JS sono diverse dalle [distribuzioni di applicazioni a pagina singola](./spa.md) in quanto non utilizzano un framework di applicazioni a pagina singola affidabile e devono essere incorporate nel contesto di qualsiasi sito Web, distribuzione o contenuto di AEM.


## Configurazioni di distribuzione

La seguente configurazione di distribuzione deve essere implementata per le distribuzioni di Componente Web/JS.

| Componente web/app JS si connette a → | AEM Author | Pubblicazione AEM | Anteprima AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Condivisione risorse tra origini](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Host di AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

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
                   <p class="is-size-6">Componente web di esempio, scritto in puro JavaScript, che utilizza contenuti delle API GraphQL headless di AEM.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
