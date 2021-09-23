---
user-guide-title: Tutorial su Adobe Experience Manager as a Cloud Service
user-guide-description: Una raccolta di tutorial su Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutorial su AEM as a Cloud Service
sub-product: cloud-service
team: TM
source-git-commit: 598d00578e5179f76b6f309c5c14dc7b1634f051
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 23%

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
   + [Archivio dei contenuti Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Servizi di authoring e pubblicazione](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [Programmi](./cloud-manager/programs.md)
   + [Ambienti](./cloud-manager/environments.md)
   + [Pipeline di produzione CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Pipeline di non produzione CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Attività](./cloud-manager/activity.md)
   + Dev Ops{#devops}
      + [Distribuzione del codice](./cloud-manager/devops/deploy-code.md)
      + [Unisci progetti](./cloud-manager/devops/merge-projects.md)
      + [Configurare le pipeline](./cloud-manager/devops/configure-pipelines.md)
      + [Integrazione continua](./cloud-manager/devops/continuous-integration.md)
      + [Analizzare i risultati del test](./cloud-manager/devops/analyze-test-results.md)
      + [Configurazioni del Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [API di Cloud Manager](./cloud-manager/devops/cloud-manager-apis.md)
+ Configurazione ambiente di sviluppo locale {#local-development-environment-set-up}
   + [Panoramica](./local-development-environment/overview.md)
   + [Strumenti di sviluppo](./local-development-environment/development-tools.md)
   + [Runtime AEM locale](./local-development-environment/aem-runtime.md)
   + [Strumenti del Dispatcher locale](./local-development-environment/dispatcher-tools.md)
+ Sviluppo{#developing}
   + Nozioni di base sullo sviluppo{#basics}
      + [SDK AEM](./developing/basics/aem-sdk.md)
      + [Ambiente di sviluppo locale](./developing/basics/local-development-environment.md)
      + [AEM Project Archetype](./developing/basics/aem-project-archetype.md)
      + [Struttura dei progetti AEM](./developing/basics/project-structure.md)
      + [Contenuto variabile e contenuto immutabile](./developing/basics/mutable-immutable.md)
      + [Pacchetto struttura archivio](./developing/basics/repository-structure-package.md)
      + [Pubblicazione dei contenuti](./developing/basics/content-publishing.md)
      + [Configurazioni OSGi](./developing/basics/osgi-configurations.md)
      + [Migrazione alla configurazione del Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Progetti AEM{#aem-projects}
      + [Progetto AEM Maven](./developing/projects/maven-project-structure.md)
   + OSGi Services{#osgi-services}
      + [Nozioni di base sui servizi OSGi](./developing/osgi-services/basics.md)
      + [Ciclo di vita dei componenti OSGi](./developing/osgi-services/lifecycle.md)
      + [Nozioni di base sulle configurazioni OSGi](./developing/osgi-services/configurations.md)
      + [Configurazioni OSGi con OCD](./developing/osgi-services/configurations-ocd.md)
   + [JavaDocs API AEM SDK](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ Debug AEM{#debugging}
   + Debug dell&#39;SDK AEM{#debugging-aem-sdk}
      + [Panoramica](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registri](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Debug remoto](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Strumenti Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Altri strumenti](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Debug AEM come Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Panoramica](./debugging/cloud-service/overview.md)
      + [Registri](./debugging/cloud-service/logs.md)
      + [Generazione e distribuzione](./debugging/cloud-service/build-and-deployment.md)
      + [Console per sviluppatori](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ Accesso a AEM{#accessing}
   + [Panoramica](./accessing/overview.md)
   + [Adobe utenti IMS](./accessing/adobe-ims-users.md)
   + [Adobe di gruppi di utenti IMS](./accessing/adobe-ims-user-groups.md)
   + [Profili di prodotto Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [AEM utenti, gruppi e autorizzazioni](./accessing/aem-users-groups-and-permissions.md)
   + [Configurazione dell’accesso a AEM procedura dettagliata](./accessing/walk-through.md)
+ Migrazione {#migration}
   + [Strumento Content Transfer (Trasferimento contenuti) ](./migration/content-transfer-tool.md)
   + [Importazione in blocco delle risorse](./migration/bulk-import.md)

   + Passaggio ad AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Introduzione](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Onboarding](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA e CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [Strumenti di modernizzazione AEM](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernizzazione archivio](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microservizi di Asset compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Ricerca e indicizzazione](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migrazione dei contenuti {#content-migration}
         + [Servizio di importazione in blocco](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Strumento Content Transfer (Trasferimento contenuti) ](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
      + [Risoluzione dei problemi](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [Introduzione](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [Registrazione digitale](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [Comunicazioni](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [Introduzione](./migration/cloud-acceleration-manager/introduction.md)
      + [Analisi di preparazione e best practice](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Fase di implementazione](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Strumento Content Transfer (Trasferimento contenuti) ](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [Strumenti di refactoring del codice](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernizzatore dell&#39;archivio del codice](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Convertitore indice](./migration/cloud-acceleration-manager/index-converter.md)
      + [Strumento Asset Workflow Migration (Migrazione flussi di lavoro per risorse)](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navigazione in Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Utilizzo di Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}
   + Crea modulo adattivo{#create-first-af}
      + [Introduzione](./forms/create-first-af/introduction.md)
      + [Crea tema](./forms/create-first-af/create-theme.md)
      + [Crea modello](./forms/create-first-af/create-template.md)
      + [Crea frammento](./forms/create-first-af/create-fragments.md)
      + [Crea modulo](./forms/create-first-af/create-af.md)
      + [Configurare il pannello principale](./forms/create-first-af/configure-root-panel.md)
      + [Pannello Persone](./forms/create-first-af/configure-people-panel.md)
      + [Configurare il pannello dei redditi](./forms/create-first-af/configure-income-panel.md)
      + [Pannello delle risorse](./forms/create-first-af/configure-assets-panel.md)
      + [Configurare il pannello iniziale](./forms/create-first-af/configure-start-panel.md)
      + [Barra degli strumenti Aggiungi e Configura](./forms/create-first-af/add-configure-toolbar.md)
   + API di Document Cloud e AEM Forms CS{#doc-cloud-sdk}
      + [Introduzione](./forms/doc-cloud-sdk/introduction.md)
      + [Crea progetto di Adobe I/O](./forms/doc-cloud-sdk/create-document-cloud-credentials.md)
      + [Creare la configurazione OSGi](./forms/doc-cloud-sdk/create-doc-cloud-configuration.md)
      + [Definisci interfaccia](./forms/doc-cloud-sdk/create-interface.md)
      + [Implementare l’interfaccia](./forms/doc-cloud-sdk/implement-interface.md)
      + [Crea parte JSON](./forms/doc-cloud-sdk/get-content-analyzer.md)
      + [Passaggio processo personalizzato](./forms/doc-cloud-sdk/custom-process-step.md)
   + Archiviazione del portale di Azure{#forms-cs-azure-portal}
      + [Introduzione](./forms/forms-cs-azure-portal/introduction.md)
      + [Crea modello dati modulo](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Archiviare i dati del modulo in Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Modulo di pre-compilazione](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Invio di query](./forms/forms-cs-azure-portal/query-submitted-data.md)


      + Crea flusso di lavoro di revisione{#create-aem-workflow}
         + [Creare un modello di flusso di lavoro](./forms/create-aem-workflow/create-workflow.md)
         + [Flusso di lavoro di attivazione](./forms/create-aem-workflow/configure-af.md)
      + Adobe Sign con AEM Forms{#forms-and-sign}
         + [Introduzione](./forms/forms-and-sign/introduction.md)
         + [Applicazione API Adobe Sign](./forms/forms-and-sign/create-sign-api-application.md)
         + [Configurazione Adobe Sign Cloud](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
         + [Creare un modulo adattivo](./forms/forms-and-sign/create-adaptive-form.md)
         + [Configura per il riempimento e la firma](./forms/forms-and-sign/configure-form-fill-and-sign.md)
      + Integrare con Salesforce{#integrate-with-salesforce}
         + [Introduzione](./forms/integrate-with-salesforce/introduction.md)
         + [Creare un’app connessa](./forms/integrate-with-salesforce/create-connected-app.md)
         + [Crea file swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
         + [Creare un’origine dati](./forms/integrate-with-salesforce/create-data-source.md)
         + [Crea modello dati modulo](./forms/integrate-with-salesforce/create-form-data-model.md)
         + [Invio del modulo di prova](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
         + [Evento clic del test](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ Estensibilità Asset compute{#asset-compute}
   + [Panoramica](./asset-compute/overview.md)
   + Imposta{#set-up}
      + [Provisioning di account e servizi](./asset-compute/set-up/accounts-and-services.md)
      + [Ambiente di sviluppo locale](./asset-compute/set-up/development-environment.md)
      + [Progetto Adobe Firefly](./asset-compute/set-up/firefly.md)
   + Sviluppa{#develop}
      + [Creare un progetto di Asset compute](./asset-compute/develop/project.md)
      + [Configurare le variabili di ambiente](./asset-compute/develop/environment-variables.md)
      + [Configura il file manifest.yml](./asset-compute/develop/manifest.md)
      + [Sviluppare un processo di lavoro](./asset-compute/develop/worker.md)
      + [Utilizzare lo strumento di sviluppo](./asset-compute/develop/development-tool.md)
   + Test e debug{#test-debug}
      + [Test di un processo di lavoro](./asset-compute/test-debug/test.md)
      + [Debug di un processo di lavoro](./asset-compute/test-debug/debug.md)
   + Implementa{#deploy}
      + [Distribuzione in Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrare con AEM](./asset-compute/deploy/processing-profiles.md)
   + Avanzate {#advanced}
      + [Operatori di metadati](./asset-compute/advanced/metadata.md)
   + [Risoluzione dei problemi](./asset-compute/troubleshooting.md)
+ Tutorials a più passaggi{#multi-step-tutorials}
   + [Sviluppo AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [Editor SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [Editor SPA (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
