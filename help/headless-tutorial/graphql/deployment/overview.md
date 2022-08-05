---
title: Implementazioni senza titolo AEM
description: Scopri le varie considerazioni sulla distribuzione per le app AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# Implementazioni senza titolo AEM

AEM le implementazioni client headless si presentano in molti modi; SPA in hosting AEM, SPA esterni, sito web, app mobile o persino processo server-to-server.

A seconda del client e della modalità di distribuzione, le distribuzioni senza intestazione AEM hanno considerazioni diverse.

## Architettura del servizio AEM

Prima di esaminare le considerazioni relative all’implementazione, è fondamentale comprendere AEM’architettura logica e la separazione e i ruoli dei livelli di servizio di AEM as a Cloud Service. AEM as a Cloud Service è composto da due servizi logici:

+ __AEM Author__ è il servizio in cui i team creano, collaborano e pubblicano frammenti di contenuto (e altre risorse).
+ __Pubblicazione AEM__ è il servizio che sono stati pubblicati Frammenti di contenuto (e altre risorse) vengono replicati per il consumo generale.
+ __Anteprima AEM__ è il servizio che imita il comportamento di AEM Publish, ma con il contenuto pubblicato a tale servizio a scopo di anteprima o revisione. AEM anteprima è destinata al pubblico interno e non alla distribuzione generale dei contenuti. L’utilizzo di Anteprima AEM è facoltativo, in base al flusso di lavoro desiderato.

![Architettura del servizio AEM](./assets/overview/aem-service-architecture.png)

Architettura di distribuzione headless as a Cloud Service tipica AEM

I client AEM headless che operano in una capacità di produzione generalmente interagiscono con AEM Publish, che contiene il contenuto approvato e pubblicato. I clienti che interagiscono con AEM Author devono prestare particolare attenzione, in quanto AEM Author è sicuro per impostazione predefinita, richiede l’autorizzazione per tutte le richieste e può contenere anche contenuti in corso di elaborazione o non approvati.

## Implementazioni client headless

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="App a pagina singola (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="App a pagina singola (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="App a pagina singola (SPA)">App a pagina singola (SPA)</a></p>
                   <p class="is-size-6">Scopri le considerazioni sulla distribuzione delle app a pagina singola (SPA).</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Componente Web/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Componente Web/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Componente Web/JS">Componente Web/JS</a></p>
               <p class="is-size-6">Scopri le considerazioni sulla distribuzione per i componenti web e i consumatori JavaScript headless basati su browser.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="App mobili" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="App mobili">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="App mobili">App mobile</a></p>
               <p class="is-size-6">Scopri le considerazioni sulla distribuzione delle app mobili.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="App server-to-server" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="App server-to-server">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="App server-to-server">App server-to-server</a></p>
               <p class="is-size-6">Informazioni sulle considerazioni di implementazione per le app server-to-server</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>