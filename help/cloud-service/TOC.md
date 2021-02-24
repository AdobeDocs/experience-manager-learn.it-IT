---
user-guide-title: Tutorial su Adobe Experience Manager as a Cloud Service
user-guide-description: Una raccolta di tutorial per Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutorial su AEM as a Cloud Service
sub-product: servizio cloud
team: TM
translation-type: tm+mt
source-git-commit: 59b786d95d1428916adad37ceca4412b93463e9b
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 25%

---


# Tutorial su Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Panoramica](./overview.md)
+ Introduzione ad AEM as a Cloud Service{#introduction}
   + [Che cos&#39;è AEM come Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Evoluzione](./introduction/evolution.md)
   + [Architettura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ Tecnologia di base {#underlying-technology}
   + [Architettura AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Repository di contenuti Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Servizi di authoring e pubblicazione](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [Programmi](./cloud-manager/programs.md)
   + [Ambienti](./cloud-manager/environments.md)
   + [Tubazione di produzione CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Pipeline di non produzione CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Attività](./cloud-manager/activity.md)
   + Ops sviluppatore{#devops}
      + [Distribuzione del codice](./cloud-manager/devops/deploy-code.md)
      + [Unisci progetti](./cloud-manager/devops/merge-projects.md)
      + [Configurare le tubazioni](./cloud-manager/devops/configure-pipelines.md)
      + [Integrazione continua](./cloud-manager/devops/continuous-integration.md)
      + [Analisi dei risultati del test](./cloud-manager/devops/analyze-test-results.md)
      + [Configurazioni del dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [API di Cloud Manager](./cloud-manager/devops/cloud-manager-apis.md)
+ Impostazione ambiente di sviluppo locale {#local-development-environment-set-up}
   + [Panoramica](./local-development-environment/overview.md)
   + [Strumenti di sviluppo](./local-development-environment/development-tools.md)
   + [Runtime AEM locale](./local-development-environment/aem-runtime.md)
   + [Strumenti per il dispatcher locale](./local-development-environment/dispatcher-tools.md)
+ Sviluppo{#developing}
   + Nozioni di base sullo sviluppo{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [Ambiente di sviluppo locale](./developing/basics/local-development-environment.md)
      + [AEM Project Archetype](./developing/basics/aem-project-archetype.md)
      + [Struttura dei progetti AEM](./developing/basics/project-structure.md)
      + [Contenuto variabile e contenuto non modificabile](./developing/basics/mutable-immutable.md)
      + [Pacchetto struttura archivio](./developing/basics/repository-structure-package.md)
      + [Pubblicazione dei contenuti](./developing/basics/content-publishing.md)
      + [Configurazioni OSGi](./developing/basics/osgi-configurations.md)
      + [Migrazione alla configurazione del dispatcher](./developing/basics/dispatcher-configuration.md)
   + [AEM JavaDocs API SDK](https://docs.adobe.com/content/help/en/experience-manager-cloud-service-javadoc/)
+ Debug AEM{#debugging}
   + Debug dell&#39;SDK AEM{#debugging-aem-sdk}
      + [Panoramica](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registri](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Debug remoto](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console Web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Strumenti Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Altri strumenti](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Debug AEM come Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Panoramica](./debugging/cloud-service/overview.md)
      + [Registri](./debugging/cloud-service/logs.md)
      + [Creazione e implementazione](./debugging/cloud-service/build-and-deployment.md)
      + [Console per sviluppatori](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ Accesso a AEM{#accessing}
   + [Panoramica](./accessing/overview.md)
   + [ utenti IMS Adobe](./accessing/adobe-ims-users.md)
   + [Gruppi di utenti  Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [ profili di prodotto IMS Adobe](./accessing/adobe-ims-product-profiles.md)
   + [AEM utenti, gruppi e autorizzazioni](./accessing/aem-users-groups-and-permissions.md)
   + [Configurazione dell&#39;accesso alla AEM dettagliata](./accessing/walk-through.md)
+ Migrazione {#migration}
   + [Strumento Content Transfer (Trasferimento contenuti) ](./migration/content-transfer-tool.md)
   + [Importazione in blocco delle risorse](./migration/bulk-import.md)
+ Estensibilità  Asset compute{#asset-compute}
   + [Panoramica](./asset-compute/overview.md)
   + Imposta{#set-up}
      + [Provisioning di account e servizi](./asset-compute/set-up/accounts-and-services.md)
      + [Ambiente di sviluppo locale](./asset-compute/set-up/development-environment.md)
      + [ progetto Adobe](./asset-compute/set-up/firefly.md)
   + Sviluppa{#develop}
      + [Creare un progetto di Asset compute ](./asset-compute/develop/project.md)
      + [Configurare le variabili di ambiente](./asset-compute/develop/environment-variables.md)
      + [Configurare manifest.yml](./asset-compute/develop/manifest.md)
      + [Sviluppare un lavoratore](./asset-compute/develop/worker.md)
      + [Utilizzare lo strumento di sviluppo](./asset-compute/develop/development-tool.md)
   + Test e debug{#test-debug}
      + [Test di un lavoratore](./asset-compute/test-debug/test.md)
      + [Debug di un lavoratore](./asset-compute/test-debug/debug.md)
   + Implementa{#deploy}
      + [Distribuisci in Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrazione con AEM](./asset-compute/deploy/processing-profiles.md)
   + Avanzate {#advanced}
      + [Operatori metadati](./asset-compute/advanced/metadata.md)
   + [Risoluzione dei problemi](./asset-compute/troubleshooting.md)
+ Tutorials a più passaggi{#multi-step-tutorials}
   + [ sviluppo AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/develop-wknd-tutorial.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [Editor SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [Editor SPA (Angular )](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [ AEM Sites e  Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)

