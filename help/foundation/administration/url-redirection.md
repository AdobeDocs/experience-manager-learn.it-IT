---
title: Reindirizzamenti URL
description: Scopri le varie opzioni per eseguire il reindirizzamento URL in AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: d5645e975aa290392348cc69d078b24921a7d13a
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 2%

---


# Reindirizzamenti URL

Il reindirizzamento URL è un aspetto comune nell’ambito del funzionamento del sito web. Gli architetti e gli amministratori sono invitati a trovare la soluzione migliore su come e dove gestire i reindirizzamenti URL, che offrono flessibilità e tempi di implementazione di reindirizzamento rapidi.

Assicurati di avere familiarità con le [AEM 6.x)](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) e [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) infrastrutture. Le principali differenze sono:

1.  AEM as a Cloud Service [CDN integrato](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)Tuttavia, i clienti possono fornire una rete CDN (BYOCDN) davanti a una rete CDN gestita da AEM.
1.  AEM 6.x se on-premise o Adobe Managed Services (AMS) non include un CDN gestito da AEM e i clienti devono portare il proprio.

Gli altri servizi AEM (Author/Publish e Dispatcher di AEM) sono concettualmente simili tra AEM 6.x e AEM as a Cloud Service.

AEM soluzioni di reindirizzamento URL sono le seguenti:

|  | Gestito e distribuito come codice AEM progetto | Possibilità di cambiare per team di marketing/contenuti | AEM compatibile con il Cloud Service | Dove si verifica l&#39;esecuzione del reindirizzamento |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [Su Edge tramite porta il tuo CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ↓ | Edge/CDN |
| [Apache `mod_rewrite` regole come configurazione del Dispatcher ](#apache-mod_rewrite-module) | ↓ | ✘ | ↓ | Dispatcher |
| [ACS Commons - Gestione mappa di reindirizzamento](#redirect-map-manager) | ✘ | ↓ | ✘ | Dispatcher |
| [ACS Commons - Gestione reindirizzamenti](#redirect-manager) | ✘ | ↓ | ↓ | AEM |


## Opzioni della soluzione

Di seguito sono riportate le opzioni della soluzione per avvicinarsi al browser del visitatore del sito web.

### A Edge tramite la tua CDN

Alcuni servizi CDN forniscono soluzioni di reindirizzamento a livello di Edge, riducendo così i viaggi di andata e ritorno all&#39;origine. Vedi [Redirector Akamai Edge](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Funzioni di AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Per informazioni sulla funzionalità di reindirizzamento a livello di Edge, rivolgiti al provider di servizi CDN.

La gestione dei reindirizzamenti a livello di Edge o CDN presenta vantaggi in termini di prestazioni, ma non è gestita come parte di progetti AEM ma piuttosto discreti. Per evitare problemi è fondamentale un processo ben concepito per gestire e implementare le regole di reindirizzamento.


### Apache `mod_rewrite` modulo

Una soluzione comune utilizza [mod_rewrite del modulo Apache](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). La [Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype) fornisce una struttura di progetto del dispatcher per entrambi [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) e [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) progetti. Le regole di riscrittura predefinite (immutabili) e personalizzate sono definite nella `conf.d/rewrites` e il motore di riscrittura è attivato per `virtualhosts` che ascolta sulla porta `80` tramite `conf.d/dispatcher_vhost.conf` file. Un esempio di implementazione è disponibile in [AEM progetto Siti WKND](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

In AEM as a Cloud Service, queste regole di reindirizzamento vengono gestite come parte del codice AEM e distribuite tramite Cloud Manager [pipeline di configurazione a livello web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) o [pipeline a stack completo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). Pertanto, il processo specifico per il progetto AEM è in esecuzione per gestire, implementare e tracciare le regole di reindirizzamento.

La maggior parte dei servizi CDN memorizza in cache i reindirizzamenti HTTP 301 e 302 a seconda dei loro `Cache-Control` o `Expires` intestazioni. Questo aiuta a evitare il round trip dopo il reindirizzamento iniziale originato da Apache/Dispatcher.


### ACS AEM Commons

Sono disponibili due funzioni in [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) per gestire i reindirizzamenti URL. ACS AEM Commons è un progetto open source gestito dalla comunità e non supportato dall&#39;Adobe.

#### Gestione mappa di reindirizzamento

[Gestione mappa di reindirizzamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) consente agli amministratori di AEM 6.x di gestire e pubblicare facilmente [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) file senza accedere direttamente al server web Apache o senza richiedere il riavvio di un server web Apache. Questa funzione consente agli utenti delle autorizzazioni di creare, aggiornare ed eliminare regole di reindirizzamento da una console in AEM, senza l’aiuto del team di sviluppo o di una distribuzione AEM. Il gestore della mappa di reindirizzamento è **NON AEM compatibile as a Cloud Service**.

#### Gestione reindirizzamento

[Gestione reindirizzamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) consente agli utenti di AEM di gestire e pubblicare facilmente i reindirizzamenti da AEM. L&#39;implementazione si basa sul filtro servlet Java™, quindi il consumo tipico delle risorse JVM. Questa funzione elimina anche la dipendenza dal team di sviluppo AEM e dalle distribuzioni di AEM. Redirect Manager è **AEM as a Cloud Service** e **AEM 6.x** compatibile. Tieni presente che, mentre la richiesta reindirizzata iniziale deve essere il suo servizio AEM Publish per generare la cache 301/302, (la maggior parte) CDN 301/302 per impostazione predefinita, consentendo il reindirizzamento delle richieste successive a edge/CDN.


## Quale soluzione è giusta per l&#39;implementazione

Di seguito sono riportati alcuni criteri per determinare la soluzione corretta. Inoltre, il processo di IT e marketing della tua organizzazione dovrebbe contribuire a scegliere la soluzione giusta.

1. Consente al team di marketing o agli utenti avanzati di gestire le regole di reindirizzamento senza il team di sviluppo AEM e i cicli di distribuzione e rilascio AEM.
1. Il processo per verificare, tracciare e ripristinare le modifiche o l’attenuazione dei rischi.
1. Disponibilità _Competenze in materia_ per **Su Edge tramite CDN Service** soluzione.

