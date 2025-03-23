---
title: Distribuzioni headless di AEM
description: Scopri le varie considerazioni sulla distribuzione per le app headless di AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# Distribuzioni headless di AEM

Le implementazioni client AEM Headless possono assumere diverse forme: applicazioni a pagina singola ospitate da AEM, applicazioni a pagina singola esterne, siti web, app mobili o persino processi server-to-server.

A seconda del client e della modalità di distribuzione, le distribuzioni headless di AEM hanno considerazioni diverse.

## Architettura del servizio AEM

Prima di esaminare le considerazioni relative all’implementazione, è fondamentale comprendere l’architettura logica di AEM e la separazione e i ruoli dei livelli di servizio di AEM as a Cloud Service. AEM as a Cloud Service è costituito da due servizi logici:

+ __AEM Author__ è il servizio in cui i team creano, collaborano e pubblicano frammenti di contenuto (e altre risorse).
+ __Pubblicazione AEM__ è il servizio pubblicato I frammenti di contenuto (e altre risorse) sono replicati per l&#39;utilizzo generale.
+ __Anteprima AEM__ è il servizio che imita il comportamento di Pubblicazione AEM, ma con contenuto pubblicato a scopo di anteprima o revisione. L’anteprima di AEM è destinata al pubblico interno e non alla distribuzione generale dei contenuti. L’utilizzo di AEM Preview è facoltativo, in base al flusso di lavoro desiderato.

![Architettura del servizio AEM](./assets/overview/aem-service-architecture.png)

Architettura di distribuzione AEM as a Cloud Service headless tipica_

I client AEM Headless che operano in una capacità di produzione in genere interagiscono con AEM Publish, che contiene il contenuto approvato e pubblicato. I clienti che interagiscono con AEM Author devono prestare particolare attenzione, in quanto AEM Author è protetto per impostazione predefinita e richiede l’autorizzazione per tutte le richieste; può inoltre contenere contenuto in corso di lavorazione o non approvato.

## Distribuzioni client headless

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="App a pagina singola" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="App a pagina singola">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="App a pagina singola">App a pagina singola</a></p>
                   <p class="is-size-6">Scopri le considerazioni sulla distribuzione per le app a pagina singola (SPA).</p>
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
               <p class="is-size-6">Scopri le considerazioni sulla distribuzione per i componenti web e i consumatori headless JavaScript basati su browser.</p>
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
               <p class="is-size-6">Scopri le considerazioni sulla distribuzione per le app per dispositivi mobili.</p>
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
               <p class="is-size-6">Scopri le considerazioni sulla distribuzione per le app server-to-server</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
