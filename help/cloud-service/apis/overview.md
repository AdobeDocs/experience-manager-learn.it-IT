---
title: Panoramica delle API AEM
description: Scopri i diversi tipi di API in Adobe Experience Manager (AEM) e ottieni una panoramica delle API basate sulle specifiche OpenAPI, comunemente note come API AEM basate su OpenAPI.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
source-git-commit: 6b8a8dc5cdcddfa2d8572bfd195bc67906882f67
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 1%

---


# Panoramica delle API AEM{#aem-apis-overview}

Scopri i diversi tipi di API in Adobe Experience Manager (AEM as a Cloud Service) e ottieni una panoramica delle API AEM basate su [OpenAPI Specification (OAS)](https://swagger.io/specification/), comunemente note come API AEM basate su OpenAPI.

AEM as a Cloud Service fornisce un’ampia gamma di API per la creazione, la lettura, l’aggiornamento e l’eliminazione di contenuti, risorse e moduli. Queste API consentono agli sviluppatori di creare applicazioni personalizzate che interagiscono con l’AEM.

Esaminiamo i diversi tipi di API in AEM e comprendiamo i concetti chiave dell’accesso alle API Adobe.

## Tipi di API AEM{#types-of-aem-apis}

AEM offre API legacy e moderna per interagire con i suoi tipi di servizi di authoring e pubblicazione.

- **API legacy**: introdotte nelle versioni precedenti dell&#39;AEM, le API legacy sono ancora supportate per la compatibilità con le versioni precedenti.

- **API moderne**: basate sulle specifiche REST e OpenAPI, queste API seguono le best practice correnti per la progettazione API e sono consigliate per le nuove integrazioni.


| Tipo di API AEM | Specifiche | Disponibilità | Caso d’uso | Esempio |
| --- | --- | --- | --- | --- |
| API tradizionali (non RESTful) | Servlet Sling | AEM 6.X, AEM as a Cloud Service | Integrazioni legacy, compatibilità con le versioni precedenti | [API Query Builder](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) e altri |
| API RESTful | HTTP, JSON | AEM 6.X, AEM as a Cloud Service | Operazioni CRUD, applicazioni moderne | [API HTTP Assets](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [API REST flusso di lavoro](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [Esportatore JSON per Content Services](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) e altri |
| API di GraphQL | GraphQL | AEM 6.X, AEM as a Cloud Service | CMS headless, SPA, app per dispositivi mobili | [API GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| API AEM basate su OpenAPI | REST, OpenAPI | **Solo AEM as a Cloud Service** | Sviluppo API-first, applicazioni moderne | [API Autore Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [API Cartelle](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [API AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Servizi Forms Acrobat](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) e altri |

>[!IMPORTANT]
>
>Le API AEM basate su OpenAPI sono disponibili solo in AEM as a Cloud Service e non sono compatibili con AEM 6.X.

Per ulteriori dettagli sulle API AEM, vedi [API Adobe Experience Manager as a Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

Diamo un’occhiata più da vicino alle API AEM basate su OpenAPI e ai concetti importanti di accesso alle API Adobe.

## API AEM basate su OpenAPI{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>Le API AEM basate su OpenAPI sono disponibili come parte di un programma di accesso anticipato. Se ti interessa accedervi, ti invitiamo a inviare un&#39;e-mail a [aem-apis@adobe.com](mailto:aem-apis@adobe.com) con una descrizione del tuo caso d&#39;uso.

La specifica [OpenAPI](https://swagger.io/specification/) (precedentemente nota come Swagger) è uno standard ampiamente utilizzato per la definizione delle API RESTful. AEM as a Cloud Service fornisce diverse API basate sulle specifiche OpenAPI (o semplicemente API AEM basate su OpenAPI), semplificando la creazione di applicazioni personalizzate che interagiscono con i tipi di servizio Author o Publish dell&#39;AEM. Di seguito sono riportati alcuni esempi:

**Siti**

- [API Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/): API per l&#39;utilizzo di frammenti di contenuto.

**Assets**

- [API cartelle](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): API per l&#39;utilizzo di cartelle quali la creazione, l&#39;elenco e l&#39;eliminazione di cartelle.

- [API Assets Author](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): API per l&#39;utilizzo delle risorse e dei relativi metadati.

**Forms**

- [Servizi Forms Acrobat](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): API per l&#39;utilizzo di moduli e documenti.

Nelle versioni future, verranno aggiunte più API AEM basate su OpenAPI per supportare casi d’uso aggiuntivi.

## Supporto dell’autenticazione{#authentication-support}

Le API AEM basate su OpenAPI supportano i seguenti metodi di autenticazione:

- **Credenziali server-to-server OAuth**: ideale per i servizi back-end che richiedono l&#39;accesso API senza interazione dell&#39;utente. Utilizza il tipo di concessione _client_credentials_, che consente la gestione degli accessi sicuri a livello di server. Per ulteriori informazioni, vedere [Credenziali server-to-server OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Credenziali app Web OAuth**: idonee per applicazioni Web con componenti front-end e _back-end_ che accedono alle API AEM per conto degli utenti. Utilizza il tipo di concessione _authorization_code_, in cui il server backend gestisce in modo sicuro segreti e token. Per ulteriori informazioni, vedere [Credenziali app Web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Credenziali app a pagina singola OAuth**: progettate per l&#39;SPA in esecuzione nel browser, che deve accedere alle API per conto di un utente senza un server back-end. Utilizza il tipo di concessione _authorization_code_ e si basa su meccanismi di sicurezza lato client che utilizzano PKCE (Proof Key for Code Exchange) per proteggere il flusso del codice di autorizzazione. Per ulteriori informazioni, vedere [Credenziali app a pagina singola OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Accesso alle API di Adobe e ai concetti correlati{#accessing-adobe-apis-and-related-concepts}

Prima di accedere alle API di Adobe, è essenziale comprendere i seguenti concetti chiave:

- **[Adobe Developer Console](https://developer.adobe.com/)**: hub per sviluppatori per accedere a API Adobe, SDK, eventi in tempo reale, funzioni senza server e altro ancora. Si noti che questo è diverso dal Developer Console _AEM_, utilizzato per il debug delle applicazioni AEM.

- **[Progetto Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/projects/)**: posizione centrale per la gestione di integrazioni API, eventi e funzioni di runtime. In questo caso puoi configurare le API, impostare l’autenticazione e generare le credenziali richieste.

- **[Profili di prodotto](https://helpx.adobe.com/it/enterprise/using/manage-product-profiles.html)**: i profili di prodotto forniscono un predefinito di autorizzazione che consente di controllare l&#39;accesso dell&#39;utente o dell&#39;applicazione a prodotti Adobe come AEM, Adobe Target, Adobe Analytics e altri. A ogni prodotto Adobe sono associati profili di prodotto predefiniti.

- **Servizi**: i servizi definiscono le autorizzazioni effettive e sono associati al profilo di prodotto. Per ridurre o aumentare il predefinito di autorizzazioni, puoi deselezionare o selezionare i servizi associati al profilo di prodotto. In questo modo, puoi controllare il livello di accesso al prodotto e alle relative API. In AEM as a Cloud Service, i servizi rappresentano gruppi di utenti con elenchi di controllo di accesso (ACL) predefiniti per i nodi dell’archivio, consentendo una gestione granulare delle autorizzazioni.

## Passaggi successivi{#next-steps}

Comprensione dei diversi tipi di API dell’AEM, tra cui
API AEM basate su OpenAPI, e i concetti chiave per accedere alle API Adobe, ora puoi iniziare a creare applicazioni personalizzate che interagiscono con l’AEM.

Iniziamo con l&#39;esercitazione [Come richiamare le API AEM basate su OpenAPI](invoke-openapi-based-aem-apis.md).
