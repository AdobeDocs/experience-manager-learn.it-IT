---
title: Reindirizzamenti URL
description: Scopri le varie opzioni per eseguire il reindirizzamento URL in AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 2b5f7a033921270113eb7f41df33444c4f3d7723
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 0%

---

# Reindirizzamenti URL

Il reindirizzamento URL è un aspetto comune nell’ambito delle operazioni sul sito web. Gli architetti e gli amministratori devono trovare la soluzione migliore per gestire i reindirizzamenti URL che offrano flessibilità e tempi di reindirizzamento rapidi.

Assicurati di avere familiarità con l&#39;infrastruttura [AEM (6.x) alias AEM Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) e [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture). Le principali differenze sono:

1. AEM as a Cloud Service dispone di [rete CDN integrata](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn), tuttavia i clienti possono fornire una rete CDN (BYOCDN) davanti alla rete CDN gestita dall&#39;AEM.
1. AEM 6.x non include una rete CDN gestita dall’AEM sia in locale che in Adobe Managed Services (AMS) e i clienti devono apportare il proprio contributo.

Gli altri servizi dell’AEM (AEM Author/Publish e Dispatcher) sono concettualmente simili tra AEM 6.x e AEM as a Cloud Service.

Le soluzioni di reindirizzamento URL AEM sono le seguenti:

|                                                   | Gestito e implementato come codice progetto AEM | Possibilità di modifica da parte del team marketing/contenuti | AEM compatibile con il Cloud Service | Dove avviene l’esecuzione del reindirizzamento |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [In Edge tramite CDN gestito da AEM](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (integrato) |
| [In Edge tramite porta la tua rete CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BIOCDN) |
| [Apache `mod_rewrite` regole come configurazione Dispatcher](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Gestione mappe di reindirizzamento](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [Commoni ACS - Responsabile reindirizzamento](#redirect-manager) | ✘ | ✔ | ✔ | AEM/Dispatcher |
| [Proprietà pagina `Redirect`](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Opzioni della soluzione

Di seguito sono riportate le opzioni di soluzione nell’ordine in cui sono più vicine al browser del visitatore del sito web.

### In Edge tramite CDN gestita dall’AEM {#at-edge-via-aem-managed-cdn}

Questa opzione è disponibile solo per i clienti AEM as a Cloud Service.

La rete CDN [gestita dall&#39;AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) fornisce una soluzione di reindirizzamento a livello di Edge, riducendo in tal modo i round trip all&#39;origine. La funzionalità [Reindirizzamenti lato client](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) consente di configurare le regole di reindirizzamento nel codice del progetto AEM e di distribuirle utilizzando la [pipeline di configurazione](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager). La dimensione del file di configurazione CDN (`cdn.yaml`) non deve superare i 100 KB.

La gestione dei reindirizzamenti a livello di Edge o CDN offre vantaggi in termini di prestazioni.

### In Edge tramite porta la tua CDN

Alcuni servizi CDN forniscono soluzioni di reindirizzamento a livello di Edge, riducendo in tal modo i round trip all’origine. Consulta [Akamai Edge Redirector](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funzioni AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Consulta il provider di servizi CDN per informazioni sulla funzionalità di reindirizzamento a livello di Edge.

La gestione dei reindirizzamenti a livello di Edge o CDN offre vantaggi in termini di prestazioni, ma non è gestita come parte dell’AEM, bensì come progetto discreto. Per evitare problemi è fondamentale un processo ben definito per gestire e distribuire le regole di reindirizzamento.


### Modulo Apache `mod_rewrite`

Una soluzione comune utilizza [il modulo Apache mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). L&#39;[Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) fornisce una struttura di progetto Dispatcher sia per il progetto [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) che per il progetto [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure). Le regole di riscrittura predefinite (immutabili) e personalizzate sono definite nella cartella `conf.d/rewrites` e il motore di riscrittura è attivato per `virtualhosts` che è in ascolto sulla porta `80` tramite il file `conf.d/dispatcher_vhost.conf`. Un esempio di implementazione è disponibile nel [progetto WKND Sites dell&#39;AEM](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

In AEM as a Cloud Service, queste regole di reindirizzamento sono gestite come parte del codice AEM e distribuite tramite la pipeline di configurazione a livello web [Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) o la [pipeline full stack](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines). Pertanto, è in gioco il processo specifico del progetto AEM per gestire, distribuire e tracciare le regole di reindirizzamento.

La maggior parte dei servizi CDN memorizza nella cache i reindirizzamenti HTTP 301 e 302 a seconda delle intestazioni `Cache-Control` o `Expires`. Aiuta ad evitare il round trip dopo il reindirizzamento iniziale proveniente da Apache/Dispatcher.


### ACS AEM Commons

In [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) sono disponibili due funzioni per gestire i reindirizzamenti URL. Nota: ACS AEM Commons è un progetto gestito dalla community, open-source e non supportato da Adobe.

#### Gestione mappa di reindirizzamento

[Redirect Map Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) consente agli amministratori di AEM di gestire e pubblicare facilmente [file Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) senza accedere direttamente al server Web Apache o richiedere il riavvio di un server Web Apache. Questa funzione consente agli utenti di creare, aggiornare ed eliminare regole di reindirizzamento da una console in AEM, senza l’aiuto del team di sviluppo o di una distribuzione AEM. Redirect Map Manager è compatibile sia con **AEM as a Cloud Service** (vedi [Strategia di reindirizzamenti URL senza pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) e [Esercitazione](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/url-redirects-using-pipeline-free-configurations#acs-commons---redirect-map-manager) correlata) che con **AEM 6.x**.

#### Gestione reindirizzamento

[Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) consente agli utenti dell&#39;AEM di gestire e pubblicare facilmente i reindirizzamenti dall&#39;AEM. L’implementazione si basa sul filtro servlet Java™, che corrisponde al consumo tipico delle risorse JVM. Questa funzione elimina anche la dipendenza dal team di sviluppo AEM e dalle implementazioni AEM. Redirect Manager è compatibile con **AEM as a Cloud Service** e **AEM 6.x**. La richiesta reindirizzata iniziale deve invece pervenire al servizio Publish dell’AEM per generare la cache 301/302 (la maggior parte) delle CDN per impostazione predefinita, consentendo di reindirizzare le richieste successive alla rete Edge/CDN.

[Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) supporta anche la strategia di [reindirizzamenti URL senza pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) per **AEM as a Cloud Service** mediante [compilazione dei reindirizzamenti in un file di testo](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html) per [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html), in modo da consentire l&#39;aggiornamento dei reindirizzamenti utilizzati nel server Web Apache senza accedervi direttamente o riavviarlo. Per ulteriori dettagli, consulta l&#39;[esercitazione](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/url-redirects-using-pipeline-free-configurations#acs-commons---redirect-manager).
In questo scenario, la richiesta di reindirizzamento iniziale arriva al server web Apache e non al servizio Publish dell’AEM.

### Proprietà pagina `Redirect`

La proprietà predefinita della pagina `Redirect` dalla [scheda Avanzate](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html) consente agli autori di contenuto di definire il percorso di reindirizzamento per la pagina corrente. Questa soluzione è ideale per scenari di reindirizzamento per pagina e non dispone di una posizione centrale per visualizzare e gestire i reindirizzamenti della pagina.

## Quale soluzione è adatta per l’implementazione

Di seguito sono riportati alcuni criteri per determinare la soluzione giusta. Inoltre, il processo IT e di marketing della tua organizzazione dovrebbe aiutare a scegliere la soluzione giusta.

1. Consentire al team marketing o agli utenti con privilegi avanzati di gestire le regole di reindirizzamento senza il team di sviluppo AEM e le distribuzioni AEM.
1. Processo per gestire, verificare, tenere traccia e ripristinare le modifiche o la riduzione dei rischi.
1. Disponibilità di _Competenze sull&#39;argomento_ per **In Edge tramite la soluzione CDN Service**.
