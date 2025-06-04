---
title: Distribuzioni di AEM Headless
description: Scopri di più sulle varie considerazioni sulla distribuzione per le app di AEM Headless.
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
workflow-type: ht
source-wordcount: '315'
ht-degree: 100%

---

# Distribuzioni di AEM Headless

Le distribuzioni del client AEM Headless possono assumere diverse forme: SPA ospitate da AEM, SPA esterne, siti web, app per dispositivi mobili o persino processi da server a server.

A seconda del client e della modalità di distribuzione, le considerazioni sulle distribuzioni di AEM Headless sono diverse.

## Architettura del servizio AEM

Prima di esaminare le considerazioni sulla distribuzione, è fondamentale comprendere l’architettura logica di AEM e la separazione e i ruoli dei livelli di servizio di AEM as a Cloud Service. AEM as a Cloud Service è costituito da due servizi logici:

+ __Authoring AEM__ è il servizio in cui i team creano, collaborano e pubblicano frammenti di contenuto (e altre risorse).
+ __Pubblicazione AEM__ è il servizio in cui i frammenti di contenuto (e altre risorse) pubblicati, sono replicati per l’utilizzo generale.
+ __Anteprima AEM__ è il servizio che imita il comportamento di Pubblicazione AEM, ma con il contenuto pubblicato a scopo di anteprima o revisione. L’anteprima AEM è destinata ai tipi di pubblico interni e non alla distribuzione generale dei contenuti. L’utilizzo di Anteprima AEM è facoltativo ed è basato sul flusso di lavoro desiderato.

![Architettura del servizio AEM](./assets/overview/aem-service-architecture.png)

Architettura di distribuzione tipica di AEM as a Cloud Service headless_

I client AEM Headless che operano in una capacità di produzione in genere interagiscono con la Pubblicazione AEM, che contiene il contenuto approvato e pubblicato. I client che interagiscono con l’Authoring AEM devono prestare particolare attenzione, in quanto l’Authoring AEM è protetto per impostazione predefinita e richiede l’autorizzazione per tutte le richieste; può inoltre contenere contenuto in corso di elaborazione o non approvato.

## Distribuzioni client headless

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="Applicazioni a pagina singola (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="Applicazioni a pagina singola (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="Applicazioni a pagina singola (SPA)">Applicazioni a pagina singola (SPA)</a></p>
                   <p class="is-size-6">Scopri di più sulle considerazioni sulla distribuzione per le applicazioni a pagina singola (SPA).</p>
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
               <a href="./web-component.md" title="Componente web/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Componente web/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Componente web/JS">Componente web/JS</a></p>
               <p class="is-size-6">Scopri di più sulle considerazioni sulla distribuzione per i componenti web e i consumatori di JavaScript headless basato su browser.</p>
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
               <a href="./mobile.md" title="App per dispositivi mobili" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="App per dispositivi mobili">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="App per dispositivi mobili">App mobile</a></p>
               <p class="is-size-6">Scopri di più sulle considerazioni sulla distribuzione per le app per dispositivi mobili.</p>
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
               <a href="./server-to-server.md" title="App da server a server" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="App da server a server">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="App da server a server">App da server a server</a></p>
               <p class="is-size-6">Scopri di più sulle considerazioni sulla distribuzione per le app da server a server</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Scopri</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
