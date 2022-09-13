---
title: Distribuzione di SPA per AEM GraphQL
description: Scopri le considerazioni sulla distribuzione per le distribuzioni di app a pagina singola (SPA) AEM senza intestazione.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: b2bf2a8e454d7ccd09819f2a38e58f7c303cb066
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 1%

---


# Implementazioni di AEM Headless SPA

Le implementazioni delle app (SPA) a pagina singola AEM headless coinvolgono applicazioni basate su JavaScript create utilizzando framework come React o Vue, che utilizzano e interagiscono con i contenuti in AEM in modo headless.

La distribuzione di un SPA che interagisce AEM in modo headless comporta l’hosting del SPA e la sua accessibilità tramite un browser web.

## Host del SPA

Un SPA è composto da una raccolta di risorse web native: **HTML, CSS e JavaScript**. Queste risorse vengono generate durante la _build_ processo (ad esempio, `npm run build`) e distribuita in un host per il consumo da parte degli utenti finali.

Ci sono varie **hosting** a seconda dei requisiti aziendali:

1. **Provider cloud** quali **Azure** o **AWS**.

2. **On-Premise** hosting in un&#39;azienda **centro dati**

3. **Piattaforme di hosting front-end** quali **AWS Amplifica**, **Servizio app di Azure**, **Netliare**, **Heroku**, **Vercel**, ecc.

## Configurazioni di distribuzione

La considerazione principale quando si ospita un SPA che interagisce con AEM headless, è se l’accesso al SPA avviene tramite dominio (o host) AEM o su un dominio diverso.  Il motivo è SPA le applicazioni web in esecuzione nei browser web e sono quindi soggette ai criteri di sicurezza dei browser web.

### Dominio condiviso

Un SPA e AEM condividono i domini quando entrambi gli utenti finali hanno accesso dallo stesso dominio. Esempio:

+ AEM è accessibile tramite: `https://wknd.site/`
+ SPA è accessibile tramite `https://wknd.site/spa`

Poiché sia AEM che SPA sono accessibili dallo stesso dominio, i browser web consentono al SPA di creare endpoint XHR per AEM Headless senza la necessità di CORS e consentono la condivisione di cookie HTTP (come AEM `login-token` cookie).

Il modo in cui il traffico SPA e AEM viene instradato sul dominio condiviso, dipende da te: CDN con origini multiple, server HTTP con proxy inverso, hosting del SPA direttamente in AEM e così via.

Di seguito sono riportate le configurazioni di distribuzione necessarie per le distribuzioni di produzione SPA, se ospitate sullo stesso dominio di AEM.

| SPA si connette a | Autore AEM | AEM Publish | Anteprima AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri del Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ↓ | ↓ |
| Condivisione delle risorse tra le origini (CORS) | ✘ | ✘ | ✘ |
| Host AEM | ✘ | ✘ | ✘ |

### Domini diversi

Un SPA e AEM hanno domini diversi quando gli utenti finali accedono da un dominio diverso. Esempio:

+ AEM è accessibile tramite: `https://wknd.site/`
+ SPA è accessibile tramite `https://wknd-app.site/`

Poiché l’accesso a AEM e SPA da domini diversi, i browser web applicano criteri di sicurezza come [condivisione delle risorse tra origini (CORS)](./configurations/cors.md)e impedisce la condivisione di cookie HTTP (come AEM `login-token` cookie).

Di seguito sono riportate le configurazioni di distribuzione necessarie per SPA distribuzioni di produzione, quando ospitate su un dominio diverso da AEM.

| SPA si connette a | Autore AEM | Pubblicazione AEM | Anteprima AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri del Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ↓ | ↓ |
| [Condivisione delle risorse tra le origini (CORS)](./configurations/cors.md) | ↓ | ↓ | ↓ |
| [Host AEM](./configurations/aem-hosts.md) | ↓ | ↓ | ↓ |

#### Esempio SPA distribuzione su domini diversi

In questo esempio, il SPA viene distribuito su un dominio Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) e il SPA utilizza AEM API GraphQL dal dominio di pubblicazione AEM (`https://publish-p65804-e666805.adobeaemcloud.com`). Le seguenti schermate evidenziano i requisiti CORS.

1. Il SPA viene servito da un dominio Netlify, ma effettua una chiamata XHR a AEM API GraphQL su un dominio diverso. Questa richiesta intersito richiede [CORS](./configurations/cors.md) da configurare su AEM per consentire la richiesta del dominio Netlify di accedere al relativo contenuto.

   ![Richiesta SPA servita da host SPA e AEM ](assets/spa/cors-requirement.png)

2. Ispezione della richiesta XHR all’API GraphQL AEM, la `Access-Control-Allow-Origin` è presente, indicando al browser Web che AEM consente la richiesta da questo dominio Netlify di accedere al relativo contenuto.

   Se il AEM [CORS](./configurations/cors.md) Mancava o non includeva il dominio Netlify, il browser web avrebbe fallito la richiesta XHR e segnalato un errore CORS.

   ![API GraphQL AEM intestazione di risposta CORS](assets/spa/cors-response-headers.png)

## Esempio di app a pagina singola

Adobe fornisce un esempio di app a pagina singola codificata in React.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="Reagisce all'app" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="Reagisce all'app">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="Reagisce all'app">Reagisce all'app</a></p>
               <p class="is-size-6">Un’app a pagina singola di esempio, scritta in React, che consuma contenuti dalle API GraphQL AEM headless.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Next.js app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Next.js app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/next-js.md" title="App Next.js" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/next-js/next-js-card.png" alt="App Next.js">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/next-js.md" title="App Next.js">App Next.js</a></p>
               <p class="is-size-6">Un’app a pagina singola di esempio, scritta in Next.js, che consuma contenuti dalle API GraphQL AEM headless.</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Visualizza esempio</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
