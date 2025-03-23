---
title: API AEM basate su OpenAPI
description: Scopri le API AEM basate su OpenAPI, tra cui il supporto per l’autenticazione, i concetti chiave e le modalità di accesso alle API Adobe.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 0eb0054d-0c0a-4ac0-b7b2-fdaceaa6479b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 1%

---

# API AEM basate su OpenAPI

>[!IMPORTANT]
>
>Le API AEM basate su OpenAPI sono disponibili solo in AEM as a Cloud Service e non sono compatibili con AEM 6.X.

Scopri le API AEM basate su OpenAPI, tra cui il supporto per l’autenticazione, i concetti chiave e le modalità di accesso alle API Adobe.

La specifica [OpenAPI](https://swagger.io/specification/) (precedentemente nota come Swagger) è uno standard ampiamente utilizzato per la definizione delle API RESTful. AEM as a Cloud Service fornisce diverse API basate sulle specifiche OpenAPI (o semplicemente API AEM basate su OpenAPI), semplificando la creazione di applicazioni personalizzate che interagiscono con i tipi di servizi Author o Publish di AEM. Di seguito sono riportati alcuni esempi:

**Siti**

- [API Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/): API per l&#39;utilizzo di frammenti di contenuto.

**Assets**

- [API cartelle](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): API per l&#39;utilizzo di cartelle quali la creazione, l&#39;elenco e l&#39;eliminazione di cartelle.

- [API Assets Author](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): API per l&#39;utilizzo delle risorse e dei relativi metadati.

**Forms**

- [API di Forms Communications](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): API per l&#39;utilizzo di moduli e documenti.

Nelle versioni future, verranno aggiunte più API AEM basate su OpenAPI per supportare casi d’uso aggiuntivi.

>[!AVAILABILITY]
>
>Le API AEM basate su OpenAPI sono disponibili come parte di un programma di accesso anticipato. Se ti interessa accedervi, ti invitiamo a inviare un&#39;e-mail a [aem-apis@adobe.com](mailto:aem-apis@adobe.com) con una descrizione del tuo caso d&#39;uso.

## Supporto dell’autenticazione{#authentication-support}

Le API AEM basate su OpenAPI supportano l’autenticazione OAuth 2.0, inclusi i seguenti tipi di sovvenzione:

- **Credenziali server-to-server OAuth**: ideale per i servizi back-end che richiedono accesso API senza interazione da parte dell&#39;utente. Utilizza il tipo di concessione _client_credentials_, che consente la gestione degli accessi sicuri a livello di server. Per ulteriori informazioni, vedere [Credenziali server-to-server OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Credenziali app Web OAuth**: adatte per applicazioni Web con componenti front-end e _back-end_ che accedono alle API AEM per conto degli utenti. Utilizza il tipo di concessione _authorization_code_, in cui il server backend gestisce in modo sicuro segreti e token. Per ulteriori informazioni, vedere [Credenziali app Web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Credenziali app a pagina singola OAuth**: progettate per applicazioni a pagina singola in esecuzione nel browser, che devono accedere alle API per conto di un utente senza un server back-end. Utilizza il tipo di concessione _authorization_code_ e si basa su meccanismi di sicurezza lato client che utilizzano PKCE (Proof Key for Code Exchange) per proteggere il flusso del codice di autorizzazione. Per ulteriori informazioni, vedere [Credenziali app a pagina singola OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Differenza tra le credenziali server-to-server OAuth e app Web OAuth/app a pagina singola{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | Server-to-server OAuth | Autenticazione utente OAuth (web-app) |
| --- | --- | --- |
| Scopo di autenticazione | Progettato per interazioni macchina-macchina. | Progettato per interazioni guidate dall&#39;utente. |
| Comportamento del token | Emette i token di accesso che rappresentano l&#39;applicazione client stessa. | Emette i token di accesso per conto di un utente autenticato. |
| Casi d’uso | Servizi back-end che richiedono accesso API senza interazione dell’utente. | Applicazioni web con componenti front-end e back-end che accedono alle API per conto degli utenti. |
| Considerazioni sulla sicurezza | Archivia in modo sicuro le credenziali sensibili (`client_id`, `client_secret`) nei sistemi back-end. | L’utente si autentica e riceve il proprio token di accesso temporaneo. Archivia in modo sicuro le credenziali sensibili (`client_id`, `client_secret`) nei sistemi back-end. |
| Tipo di concessione | _credenziali_client_ | _codice_autorizzazione_ |

## Accesso alle API di Adobe e ai concetti correlati{#accessing-adobe-apis-and-related-concepts}

Prima di accedere alle API di Adobe, è essenziale comprendere i seguenti costrutti chiave:

- **[Adobe Developer Console](https://developer.adobe.com/)**: hub per sviluppatori per accedere a API, SDK, eventi in tempo reale, funzioni senza server di Adobe e altro ancora. Si noti che è diverso dal Developer Console _AEM_, utilizzato per il debug delle applicazioni AEM.

- **[Progetto Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/projects/)**: posizione centrale per la gestione di integrazioni API, eventi e funzioni di runtime. In questo caso puoi configurare le API, impostare l’autenticazione e generare le credenziali richieste.

- **[Profili di prodotto](https://helpx.adobe.com/it/enterprise/using/manage-product-profiles.html)**: i profili di prodotto forniscono un predefinito di autorizzazione che consente di controllare l&#39;accesso dell&#39;utente o dell&#39;applicazione a prodotti Adobe come AEM, Adobe Target, Adobe Analytics e altri. A ogni prodotto Adobe sono associati profili di prodotto predefiniti.

- **Servizi**: i servizi definiscono le autorizzazioni effettive e sono associati al profilo di prodotto. Per ridurre o aumentare il predefinito di autorizzazioni, puoi deselezionare o selezionare i servizi associati al profilo di prodotto. In questo modo, puoi controllare il livello di accesso al prodotto e alle relative API. In AEM as a Cloud Service, i servizi rappresentano gruppi di utenti con elenchi di controllo di accesso (ACL) predefiniti per i nodi dell’archivio, consentendo una gestione granulare delle autorizzazioni.

## Inizia

Scopri come configurare l’ambiente AEM as a Cloud Service e un progetto Adobe Developer Console per abilitare l’accesso alle API AEM basate su OpenAPI. Accedi anche all’API di AEM utilizzando il browser per verificare la configurazione e rivedere la richiesta e la risposta.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Configurare le API AEM basate su OpenAPI" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="Configurare le API AEM basate su OpenAPI"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Configurare le API AEM basate su OpenAPI">Configurare le API AEM basate su OpenAPI</a>
                    </p>
                    <p class="is-size-6">Scopri come configurare l’ambiente AEM as a Cloud Service per abilitare l’accesso alle API AEM basate su OpenAPI.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Esercitazioni API

Scopri come utilizzare le API AEM basate su OpenAPI utilizzando diversi metodi di autenticazione OAuth:

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="Richiama API tramite autenticazione server-to-server" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="Richiama API tramite autenticazione server-to-server"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Richiama API tramite autenticazione server-to-server">Richiama API tramite autenticazione server-to-server</a>
                    </p>
                    <p class="is-size-6">Scopri come richiamare le API AEM basate su OpenAPI da un’applicazione NodeJS personalizzata utilizzando l’autenticazione server-to-server di OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="Richiama l’API tramite l’autenticazione tramite app web" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="Richiama l’API tramite l’autenticazione tramite app web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Richiama l’API tramite l’autenticazione tramite app web">Richiama l'API tramite l'autenticazione dell'app Web</a>
                    </p>
                    <p class="is-size-6">Scopri come richiamare le API AEM basate su OpenAPI da un’applicazione web personalizzata utilizzando l’autenticazione OAuth Web App.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
