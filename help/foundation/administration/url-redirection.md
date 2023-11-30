---
title: Reindirizzamenti URL
description: Scopri le varie opzioni per eseguire il reindirizzamento URL in AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 2%

---

# Reindirizzamenti URL

Il reindirizzamento URL è un aspetto comune nell’ambito delle operazioni sul sito web. Gli architetti e gli amministratori devono trovare la soluzione migliore per gestire i reindirizzamenti URL che offrano flessibilità e tempi di reindirizzamento rapidi.

Assicurati di conoscere il [AEM (6.x) alias AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) e [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) infrastrutture. Le principali differenze sono:

1. AEM as a Cloud Service ha [CDN integrata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)Tuttavia, i clienti possono fornire una rete CDN (BYOCDN) davanti alla rete CDN gestita dall’AEM.
1. AEM 6.x non include una rete CDN gestita dall’AEM sia in locale che in Adobe Managed Services (AMS) e i clienti devono apportare il proprio contributo.

Gli altri servizi dell’AEM (Autore/Pubblicazione AEM e Dispatcher) sono concettualmente simili tra AEM 6.x e AEM as a Cloud Service.

Le soluzioni di reindirizzamento URL AEM sono le seguenti:

|                                                   | Gestito e implementato come codice progetto AEM | Possibilità di modifica da parte del team marketing/contenuti | AEM compatibile con il Cloud Service | Dove avviene l’esecuzione del reindirizzamento |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [In Edge tramite porta la tua CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` regole come configurazione di Dispatcher](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Gestione mappa di reindirizzamento](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons - Gestione reindirizzamento](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [Il `Redirect` proprietà page](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opzioni della soluzione

Di seguito sono riportate le opzioni di soluzione nell’ordine in cui sono più vicine al browser del visitatore del sito web.

### In Edge tramite porta la tua CDN

Alcuni servizi CDN forniscono soluzioni di reindirizzamento a livello di Edge, riducendo così i round trip all’origine. Consulta [Redirector Akamai Edge](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funzioni AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulta il provider di servizi CDN per informazioni sulla funzionalità di reindirizzamento a livello Edge.

La gestione dei reindirizzamenti a livello Edge o CDN offre vantaggi in termini di prestazioni, ma non è gestita come parte dell’AEM, bensì come progetto discreto. Per evitare problemi è fondamentale un processo ben concepito per gestire e distribuire le regole di reindirizzamento.


### Apache `mod_rewrite` modulo

Una soluzione comune utilizza [Modulo Apache mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). Il [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) fornisce una struttura di progetto di Dispatcher per entrambi [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) e [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) progetto. Le regole di riscrittura predefinite (immutabili) e personalizzate sono definite nella `conf.d/rewrites` e il motore di riscrittura è attivato per `virtualhosts` che ascolta sulla porta `80` tramite `conf.d/dispatcher_vhost.conf` file. Un esempio di implementazione è disponibile nella [Progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

In AEM as a Cloud Service, queste regole di reindirizzamento sono gestite come parte del codice AEM e distribuite tramite Cloud Manager [Pipeline di configurazione a livello web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) o [Pipeline full stack](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). Pertanto, è in gioco il processo specifico del progetto AEM per gestire, distribuire e tracciare le regole di reindirizzamento.

La maggior parte dei servizi CDN memorizza nella cache i reindirizzamenti HTTP 301 e 302 a seconda dei `Cache-Control` o `Expires` intestazioni. Questo aiuta ad evitare il round trip dopo il reindirizzamento iniziale proveniente da Apache/Dispatcher.


### ACS AEM Commons

Sono disponibili due funzioni in [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) per gestire i reindirizzamenti URL. Nota: ACS AEM Commons è un progetto gestito dalla community, open-source e non supportato da Adobe.

#### Gestione mappa di reindirizzamento

[Gestione mappa di reindirizzamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) consente agli amministratori di AEM 6.x di gestire e pubblicare facilmente [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) senza accedere direttamente al server web Apache o richiedere il riavvio del server web Apache. Questa funzione consente agli utenti di creare, aggiornare ed eliminare regole di reindirizzamento da una console in AEM, senza l’aiuto del team di sviluppo o di una distribuzione AEM. Il gestore delle mappe di reindirizzamento è **NON compatibile con AEM as a Cloud Service**.

#### Gestione reindirizzamento

[Gestione reindirizzamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) consente agli utenti dell’AEM di gestire e pubblicare facilmente i reindirizzamenti dall’AEM. L’implementazione si basa sul filtro servlet Java™, che corrisponde al consumo tipico delle risorse JVM. Questa funzione elimina anche la dipendenza dal team di sviluppo AEM e dalle implementazioni AEM. Redirect Manager è entrambi **AEM as a Cloud Service** e **AEM 6.x** compatibile. Mentre la richiesta reindirizzata iniziale deve raggiungere il servizio di pubblicazione AEM per generare la cache 301/302 (la maggior parte) delle CDN per impostazione predefinita, consentendo alle richieste successive di essere reindirizzate al server Edge/CDN.

### Il `Redirect` proprietà page

La soluzione preconfigurata `Redirect` proprietà di pagina da [Scheda Avanzate](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) consente agli autori di contenuto di definire la posizione di reindirizzamento per la pagina corrente. Questa soluzione è ideale per scenari di reindirizzamento per pagina e non dispone di una posizione centrale per visualizzare e gestire i reindirizzamenti della pagina.

## Quale soluzione è adatta per l’implementazione

Di seguito sono riportati alcuni criteri per determinare la soluzione giusta. Inoltre, il processo IT e di marketing della tua organizzazione dovrebbe aiutare a scegliere la soluzione giusta.

1. Consentire al team marketing o agli utenti con privilegi avanzati di gestire le regole di reindirizzamento senza il team di sviluppo AEM e le distribuzioni AEM.
1. Processo per gestire, verificare, tenere traccia e ripristinare le modifiche o la riduzione dei rischi.
1. Disponibilità di _Competenze in materia_ per **In Edge tramite il servizio CDN** soluzione.
