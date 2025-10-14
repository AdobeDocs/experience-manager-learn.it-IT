---
title: Distribuzione di applicazioni a pagina singola per AEM GraphQL
description: Scopri le considerazioni sulla distribuzione per le distribuzioni headless di app a pagina singola (SPA) AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 5%

---

# Distribuzioni di applicazioni a pagina singola AEM headless

Le implementazioni di app AEM headless a pagina singola (SPA) coinvolgono applicazioni basate su JavaScript create utilizzando framework come React o Vue, che utilizzano e interagiscono con i contenuti in AEM in modo headless.

La distribuzione di un’applicazione a pagina singola che interagisce con AEM in modo headless comporta l’hosting dell’applicazione a pagina singola e la sua accessibilità tramite un browser web.

## Ospitare l’applicazione a pagina singola

Un&#39;applicazione a pagina singola è composta da una raccolta di risorse Web native: **HTML, CSS e JavaScript**. Queste risorse vengono generate durante il processo _build_ (ad esempio, `npm run build`) e distribuite a un host per l&#39;utilizzo da parte degli utenti finali.

Esistono varie opzioni di **hosting** a seconda dei requisiti della tua organizzazione:

1. **Provider cloud** come **Azure** o **AWS**.

2. **On-Premise** hosting in un **data center** aziendale

3. **Piattaforme di hosting front-end** quali **AWS Amplify**, **Azure App Service**, **Netlify**, **Heroku**, **Vercel** e così via.

## Configurazioni di distribuzione

Quando si ospita un’applicazione a pagina singola che interagisce con AEM headless, l’elemento principale da considerare è se l’applicazione a pagina singola è accessibile tramite il dominio (o host) di AEM o su un dominio diverso.  Il motivo è che le applicazioni a pagina singola sono applicazioni web in esecuzione nei browser web e quindi sono soggette ai criteri di sicurezza dei browser web.

### Dominio condiviso

Un’applicazione a pagina singola e AEM condividono i domini quando entrambi sono accessibili agli utenti finali dallo stesso dominio. Ad esempio:

+ Accesso ad AEM tramite: `https://wknd.site/`
+ L&#39;applicazione a pagina singola è accessibile tramite `https://wknd.site/spa`

Poiché sia AEM che l’applicazione a pagina singola sono accessibili dallo stesso dominio, i browser web consentono all’applicazione a pagina singola di effettuare XHR per gli endpoint headless di AEM senza la necessità di CORS e consentono la condivisione di cookie HTTP (come il cookie `login-token` di AEM).

Sta a te definire il modo in cui il traffico di SPA e AEM viene instradato sul dominio condiviso: CDN con più origini, server HTTP con proxy inverso, hosting dell’SPA direttamente in AEM e così via.

Di seguito sono riportate le configurazioni di distribuzione necessarie per le distribuzioni di produzione di applicazioni a pagina singola, se ospitate sullo stesso dominio di AEM.

| L’applicazione a pagina singola si connette a → | AEM Author | Pubblicazione AEM | Anteprima AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Condivisione delle risorse tra le origini (CORS) | ✘ | ✘ | ✘ |
| Host di AEM | ✘ | ✘ | ✘ |

### Domini diversi

Un’applicazione a pagina singola e AEM hanno domini diversi quando gli utenti finali del diverso dominio vi accedono. Ad esempio:

+ Accesso ad AEM tramite: `https://wknd.site/`
+ L&#39;applicazione a pagina singola è accessibile tramite `https://wknd-app.site/`

Poiché AEM e l&#39;applicazione a pagina singola sono accessibili da domini diversi, i browser Web applicano criteri di sicurezza quali [Cross-Source Resource Sharing (CORS)](./configurations/cors.md) e impediscono la condivisione di cookie HTTP (come il cookie `login-token` di AEM).

Di seguito sono riportate le configurazioni di distribuzione necessarie per le distribuzioni di produzione di applicazioni a pagina singola, se ospitate su un dominio diverso da AEM.

| L’applicazione a pagina singola si connette a → | AEM Author | Pubblicazione AEM | Anteprima AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtri Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Condivisione risorse tra origini](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Host di AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### Esempio di implementazione di applicazioni a pagina singola in domini diversi

In questo esempio, l&#39;applicazione a pagina singola viene distribuita a un dominio Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) e l&#39;applicazione a pagina singola utilizza le API AEM GraphQL dal dominio di pubblicazione AEM (`https://publish-p65804-e666805.adobeaemcloud.com`). Le schermate seguenti evidenziano il requisito CORS.

1. L’applicazione a pagina singola viene fornita da un dominio Netlify, ma effettua una chiamata XHR alle API GraphQL di AEM su un dominio diverso. Questa richiesta intersito richiede la configurazione di [CORS](./configurations/cors.md) in AEM per consentire l&#39;accesso al contenuto da parte del dominio Netlify.

   ![Richiesta SPA inviata dagli host SPA e AEM &#x200B;](assets/spa/cors-requirement.png)

2. Durante l&#39;analisi della richiesta XHR all&#39;API GraphQL di AEM, `Access-Control-Allow-Origin` è presente, indicando al browser Web che AEM consente alla richiesta di questo dominio Netlify di accedere al relativo contenuto.

   Se il [CORS](./configurations/cors.md) di AEM fosse mancante o non includesse il dominio Netlify, il browser Web non avrebbe risposto alla richiesta XHR e segnalerebbe un errore CORS.

   ![Intestazione risposta CORS API GraphQL AEM](assets/spa/cors-response-headers.png)

## Esempio di app a pagina singola

Adobe fornisce un esempio di app a pagina singola codificata in React.

<!-- CARDS 

* ../example-apps/react-app.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React App - AEM Headless Example">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../example-apps/react-app.md" title="App React - Esempio di AEM Headless" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app.png" alt="App React - Esempio di AEM Headless"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../example-apps/react-app.md" target="_blank" rel="referrer" title="App React - Esempio di AEM Headless">App React - Esempio AEM Headless</a>
                    </p>
                    <p class="is-size-6">Le applicazioni di esempio sono un ottimo modo per esplorare le funzionalità headless di Adobe Experience Manager (AEM). Questa applicazione React illustra come eseguire query sui contenuti che utilizzano le API GraphQL di AEM utilizzando query persistenti.</p>
                </div>
                <a href="../example-apps/react-app.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


