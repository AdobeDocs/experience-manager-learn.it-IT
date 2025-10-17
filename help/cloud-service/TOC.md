---
user-guide-title: Tutorial su Adobe Experience Manager as a Cloud Service
user-guide-description: Una raccolta di tutorial su Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutorial su AEM as a Cloud Service
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Experience Manager as a Cloud Service
team: TM
source-git-commit: c367564acb6465d5f203e5db943c5470607b63c9
workflow-type: tm+mt
source-wordcount: '1412'
ht-degree: 99%

---


# Tutorial su Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Panoramica](./overview.md)
+ Versioni di prova AEM {#aem-trials}
   + [Immagini](./aem-trials/images.md)
+ Playlist{#playlists}
   + [Sviluppo AEM](./playlists/development.md)
+ Introduzione ad AEM as a Cloud Service{#introduction}
   + [Che cos’è AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Architettura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Strategia e leadership di pensiero{#strategy}
      + [Experience Manager - Modelli e archetipi per governance e personale](./introduction/experience-manager-governance-and-staffing-models.md)
+ [Experience Hub](./experience-hub.md)
+ [Assistente IA di AEM](./aem-ai-assisstant.md)
+ Integrazioni di Experience Cloud{#integrations}
   + [Integrazioni](./integrations/experience-cloud.md)
   + [AEM Headless e Target](./integrations/target.md)
+ Tecnologia di base {#underlying-technology}
   + [Architettura di AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java Content Repository](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Servizi di authoring e pubblicazione](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [Plug-in AEM Assets Sidekick](https://experienceleague.adobe.com/it/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [Programmi](./cloud-manager/programs.md)
   + [Ambienti](./cloud-manager/environments.md)
   + [Utilizzo di un archivio GitHub](./cloud-manager/byogithub.md)
   + [Pipeline CI/CD di produzione](./cloud-manager/cicd-production-pipeline.md)
   + [Pipeline CI/CD non di produzione](./cloud-manager/cicd-non-production-pipeline.md)
   + [Attività](./cloud-manager/activity.md)
   + [Nomi di dominio personalizzati](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names){target=_blank}
   + [Ripristino del contenuto](./cloud-manager/content-restore.md)
   + Operazioni di sviluppo{#devops}
      + [Distribuzione del codice](./cloud-manager/devops/deploy-code.md)
      + [Unire i progetti](./cloud-manager/devops/merge-projects.md)
      + [Configurare le pipeline](./cloud-manager/devops/configure-pipelines.md)
      + [Integrazione continua](./cloud-manager/devops/continuous-integration.md)
      + [Analizzare i risultati del test](./cloud-manager/devops/analyze-test-results.md)
      + [Configurazioni di Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [Analisi del registro CDN](./cloud-manager/devops/cdn-log-analysis.md)
+ Configurazione dell’ambiente di sviluppo locale {#local-development-environment-set-up}
   + [Panoramica](./local-development-environment/overview.md)
   + [Strumenti di sviluppo](./local-development-environment/development-tools.md)
   + [SDK AEM locale](./local-development-environment/aem-runtime.md)
   + [Strumenti Dispatcher locali](./local-development-environment/dispatcher-tools.md)
+ Sviluppo{#developing}
   + Estensibilità{#extensibility}
      + App Builder{#app-builder}
         + [Generare il token di accesso JWT](./developing/extensibility/app-builder/jwt-auth.md)
         + [Generare l token di accesso server-to-server](./developing/extensibility/app-builder/server-to-server-auth.md)
         + [Verifica del webhook Github](./developing/extensibility/app-builder/github-webhook-verification.md)
      + Estensibilità dell’interfaccia utente{#ui}
         + [Panoramica](./developing/extensibility/ui/overview.md)
         + [Progetto Adobe Developer Console](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [Inizializzare l’app](./developing/extensibility/ui/app-initialization.md)
         + [Registrare l’estensione](./developing/extensibility/ui/extension-registration.md)
         + [Finestra modale](./developing/extensibility/ui/modal.md)
         + [Azione Adobe I/O Runtime](./developing/extensibility/ui/runtime-action.md)
         + [Verificare](./developing/extensibility/ui/verify.md)
         + [Distribuzione](./developing/extensibility/ui/deploy.md)
         + Frammenti di contenuto{#content-fragments}
            + [Panoramica](./developing/extensibility/ui/content-fragments/overview.md)
            + Esempi{#examples}
               + [Generazione di immagini IA](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [Aggiornamento delle proprietà in blocco](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [Colonne della griglia personalizzate](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [Esportare come XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [Pulsante editor Rich Text nella barra degli strumenti](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [Widget per l’editor Rich Text](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [Badge per l’editor Rich Text](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
               + [Campi personalizzati](./developing/extensibility/ui/content-fragments/examples/editor-custom-field.md)
   + Nozioni di base sullo sviluppo{#basics}
      + [SDK AEM](./developing/basics/aem-sdk.md)
      + [Ambiente di sviluppo locale](./developing/basics/local-development-environment.md)
      + [Archetipo di progetto AEM](./developing/basics/aem-project-archetype.md)
      + [Struttura dei progetti AEM](./developing/basics/project-structure.md)
      + [Contenuti mutabili e immutabili ](./developing/basics/mutable-immutable.md)
      + [Pacchetto di struttura dell’archivio](./developing/basics/repository-structure-package.md)
      + [Pubblicazione dei contenuti](./developing/basics/content-publishing.md)
      + [Configurazioni OSGi](./developing/basics/osgi-configurations.md)
      + [Migrazione della configurazione Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Progetti AEM{#aem-projects}
      + [Progetto AEM Maven](./developing/projects/maven-project-structure.md)
      + [Pulizia di un progetto AEM Maven](./developing/projects/remove-samples.md)
   + Servizi OSGi{#osgi-services}
      + [Nozioni di base sul servizio OSGi](./developing/osgi-services/basics.md)
      + [Ciclo di vita del componente OSGi](./developing/osgi-services/lifecycle.md)
      + [Nozioni di base sulle configurazioni OSGi](./developing/osgi-services/configurations.md)
      + [Configurazioni OSGi con OCD](./developing/osgi-services/configurations-ocd.md)
   + Avanzato{#advanced}
      + [Memorizzazione in cache delle varianti di pagina](./developing/advanced/variant-caching.md)
      + [Protezione CSRF](./developing/advanced/csrf-protection.md)
      + [Spazi dei nomi personalizzati](./developing/advanced/custom-namespaces.md)
      + [Parametrizzare i modelli Sling da HTL](./developing/advanced/sling-model-parameters.md)
      + [Segreti](./developing/advanced/secrets.md)
      + [Utenti del servizio](./developing/advanced/service-users.md)
      + [API per immagini ottimizzate per il web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [Eseguire il processo sull’istanza iniziale nell’Authoring AEM](./developing/advanced/run-job-on-leader-instance-in-aem-author.md)
   + Ambiente di sviluppo rapido{#rde}
      + [Panoramica](./developing/rde/overview.md)
      + [Procedura di configurazione](./developing/rde/how-to-setup.md)
      + [Procedura di utilizzo](./developing/rde/how-to-use.md)
      + [Ciclo di vita dello sviluppo](./developing/rde/development-life-cycle.md)
   + Editor universale{#universal-editor}
      + Modificare l’app React{#react-app-editing}
         + [Panoramica](./developing/universal-editor/react-app/overview.md)
         + [Configurazione dello sviluppo locale](./developing/universal-editor/react-app/local-development-setup.md)
         + [Preparare l’app React](./developing/universal-editor/react-app/instrument-to-edit-content.md)
   + [JavaDocs delle API SDK di AEM](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ Debug di AEM{#debugging}
   + Debug di AEM SDK{#debugging-aem-sdk}
      + [Panoramica](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registri](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Debug remoto](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Strumenti di Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Altri strumenti](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Debug di AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Panoramica](./debugging/cloud-service/overview.md)
      + [Registri](./debugging/cloud-service/logs.md)
      + [Creazione e distribuzione](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [Browser dell’archivio](./debugging/cloud-service/repository-browser.md)
      + Rischi{#risks}
         + [Avvisi di attraversamento](./debugging/cloud-service/risks/traversals.md)
+ Personalizzazione {#personalization}
   + [Panoramica](./personalization/overview.md)
   + Configurazione{#setup}
      + [Integrare Adobe Target](./personalization/setup/integrate-adobe-target.md)
      + [Integrare i tag](./personalization/setup/integrate-adobe-tags.md)
   + Casi d’uso {#use-cases}
      + [Sperimentazione (test A/B)](./personalization/use-cases/experimentation.md)
      + [Targeting comportamentale](./personalization/use-cases/behavioral-targeting.md)
      + [Personalization per utenti noti](./personalization/use-cases/known-user-personalization.md)
+ API di AEM{#aem-apis}
   + [Panoramica](./apis/overview.md)
   + OpenAPI{#openapis}
      + [Panoramica](./apis/openapis/overview.md)
      + [Procedura di configurazione](./apis/openapis/setup.md)
      + [Autenticazione da server a server](./apis/openapis/use-cases/invoke-api-using-oauth-s2s.md)
      + [Autenticazione utente (app web)](./apis/openapis/use-cases/invoke-api-using-oauth-web-app.md)
      + [Autenticazione utente (SPA)](./apis/openapis/use-cases/invoke-api-using-oauth-single-page-app.md)
      + Procedura{#how-to}
         + [Gestione di credenziali e profilo di prodotto](./apis/openapis/how-to/credentials-and-product-profile-management.md)
         + [Gestione delle autorizzazioni](./apis/openapis/how-to/services-user-group-permission-management.md)
+ Consegna dei contenuti{#content-delivery}
   + [Nome di dominio personalizzato](./content-delivery/custom-domain-names.md)
   + [Nome di dominio personalizzato con CDN gestita da Adobe](./content-delivery/custom-domain-name-with-adobe-managed-cdn.md)
   + [Nome di dominio personalizzato con CDN cliente](./content-delivery/custom-domain-names-with-customer-managed-cdn.md)
   + [Memorizzazione in cache](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/caching/overview){target=_blank}
   + [CDN Adobe - Oltre la memorizzazione in cache](./content-delivery/adobe-cdn-beyond-caching.md)
   + [Pagine di errore personalizzate](./content-delivery/custom-error-pages.md)
   + [Reindirizzamenti URL](https://experienceleague.adobe.com/it/docs/experience-manager-learn/foundation/administration/url-redirection){target=_blank}
+ Memorizzazione in cache{#caching}
   + [Panoramica](./caching/overview.md)
   + [Servizio di pubblicazione AEM](./caching/publish.md)
   + [Servizio AEM Author](./caching/author.md)
   + [Analisi del rapporto hit della cache CDN](./caching/cdn-cache-hit-ratio-analysis.md)
   + Procedura{#how-to}
      + [Abilitare la memorizzazione in cache](./caching/how-to/enable-caching.md)
      + [Disabilitare la memorizzazione in cache](./caching/how-to/disable-caching.md)
      + [Pulizia della cache](./caching/how-to/purge-cache.md)
+ Accedere ad AEM{#accessing}
   + [Panoramica](./accessing/overview.md)
   + [Utenti di Adobe IMS](./accessing/adobe-ims-users.md)
   + [Gruppi di utenti di Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Profili di prodotto di Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [Utenti, gruppi e autorizzazioni di AEM](./accessing/aem-users-groups-and-permissions.md)
   + [Procedura dettagliata della configurazione dell’accesso a AEM](./accessing/walk-through.md)
+ Autenticazione{#authentication}
   + [Panoramica](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ Rete avanzata{#networking}
   + [Panoramica](./networking/advanced-networking.md)
   + [Uscita da porta flessibile](./networking/flexible-port-egress.md)
   + [Indirizzo IP di uscita dedicato](./networking/dedicated-egress-ip-address.md)
   + [Rete privata virtuale (VPN)](./networking/vpn.md)
   + Esempi di codice{#examples}
      + [HTTP/HTTPS su porte non standard per l’uscita da porta flessibile](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [HTTP/HTTPS per indirizzo IP/VPN di uscita dedicato](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [Connessioni SQL utilizzando DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Connessioni SQL utilizzando l’API Java SQL](./networking/examples/sql-java-apis.md)
      + [Servizio e-mail](./networking/examples/email-service.md)
+ Sicurezza {#security}
   + [Bloccare gli attacchi DoS/DDoS utilizzando le regole del filtro del traffico](./security/blocking-dos-attack-using-traffic-filter-rules.md)
   + Regole del filtro del traffico che includono le regole WAF {#traffic-filter-and-waf-rules}
      + [Protezione dei siti web di AEM](./security/traffic-filter-and-waf-rules/overview.md)
      + [Configurazione](./security/traffic-filter-and-waf-rules/setup.md)
      + [Utilizzo delle regole per il filtro del traffico](./security/traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md)
      + [Utilizzo delle regole di WAF](./security/traffic-filter-and-waf-rules/use-cases/using-waf-rules.md)
      + [Best practice](./security/traffic-filter-and-waf-rules/best-practices.md)
      + Procedura{#how-to}
         + [Monitoraggio delle richieste sensibili](./security/traffic-filter-and-waf-rules/how-to/request-logging.md)
         + [Limitazione dell’accesso](./security/traffic-filter-and-waf-rules/how-to/request-blocking.md)
         + [Normalizzare le richieste](./security/traffic-filter-and-waf-rules/how-to/request-transformation.md)
+ Eventi AEM{#aem-eventing}
   + [Panoramica](./eventing/overview.md)
   + Esempi{#examples}
      + [Webhook - Ricezione di eventi AEM](./eventing/examples/webhook.md)
      + [Giornale di registrazione - Caricare eventi AEM](./eventing/examples/journaling.md)
      + [Azione Adobe I/O Runtime - Ricezione di eventi AEM](./eventing/examples/runtime-action.md)
      + [Azione Adobe I/O Runtime - Elaborazione di eventi AEM](./eventing/examples/event-processing-using-runtime-action.md)
      + [Eventi AEM Assets - Integrazione PIM](./eventing/examples/assets-pim-integration.md)
+ Migrazione {#migration}
   + [Strumento di trasferimento contenuti](./migration/content-transfer-tool.md)
   + [Importazione in blocco delle risorse](./migration/bulk-import.md)
   + Passaggio ad AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Introduzione](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Onboarding](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA e CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [Strumenti di modernizzazione AEM](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernizzazione dell’archivio](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microservizi Asset Compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Ricerca e indicizzazione](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migrazione dei contenuti {#content-migration}
         + [Servizio di importazione in blocco](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Strumento di trasferimento contenuti](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [Domande frequenti](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [Risoluzione di problemi](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [Introduzione](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [Registrazione digitale](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [Comunicazioni](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [Introduzione](./migration/cloud-acceleration-manager/introduction.md)
      + [Preparazione e Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Fase di implementazione](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Strumenti di refactoring del codice](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernizzatore dell’archivio del codice](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Convertitore di Dispatcher](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Convertitore indice](./migration/cloud-acceleration-manager/index-converter.md)
      + [Strumento per la migrazione dei flussi di lavoro delle risorse](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navigazione in Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Utilizzo di Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [Frammenti di contenuto](https://experienceleague.adobe.com/it/docs/experience-manager-learn/content-fragments-console/overview){target=_blank}
+ Moduli{#forms}
   + Sviluppo per Forms as a Cloud Service{#developing-for-cloud-service}
      + [1 - Guida introduttiva](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - Installare IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - Configurare Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 - Sincronizzare IntelliJ con AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 - Creare un modulo](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - Handler di invio personalizzato](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - Registrazione del servlet utilizzando un tipo di risorsa](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 - Abilitare i componenti del Portale moduli](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 - Includere servizi cloud e FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 - Configurazione cloud in base al contesto](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 - Inviare a Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 - Distribuire nell’ambiente di sviluppo](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 - Aggiornamento dell’archetipo Maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + Creare un modulo adattivo{#create-first-af}
      + [Introduzione](./forms/create-first-af/introduction.md)
      + [Creare un tema](./forms/create-first-af/create-theme.md)
      + [Creare un modello](./forms/create-first-af/create-template.md)
      + [Creare un frammento](./forms/create-first-af/create-fragments.md)
      + [Creare un modulo](./forms/create-first-af/create-af.md)
      + [Configurare il pannello principale](./forms/create-first-af/configure-root-panel.md)
      + [Configurare il pannello persone](./forms/create-first-af/configure-people-panel.md)
      + [Configurare il pannello reddito](./forms/create-first-af/configure-income-panel.md)
      + [Configurare il pannello delle risorse](./forms/create-first-af/configure-assets-panel.md)
      + [Configurare il pannello iniziale](./forms/create-first-af/configure-start-panel.md)
      + [Aggiungere e configurare la barra degli strumenti](./forms/create-first-af/add-configure-toolbar.md)
   + Servizio di invio personalizzato con modulo headless{#custom-submit-headless-forms}
      + [1 - Introduzione](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - Creare un servizio di invio personalizzato](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - Visualizzare la risposta](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + Creare un componente blocco di indirizzi{#create-address-block}
      + [1 - Introduzione](./forms/create-address-block-component/introduction.md)
      + [2 - Configurazione](./forms/create-address-block-component/set-up.md)
      + [3 - Creare un componente](./forms/create-address-block-component/creating-address-component.md)
      + [4 - Distribuire un componente](./forms/create-address-block-component/deploy-your-project.md)
   + Creare un componente immagine cliccabile{#clickable-image-component}
      + [1 - Introduzione](./forms/clickable-image-component/introduction.md)
      + [2 - Creare un componente](./forms/clickable-image-component/create-component.md)
      + [3 - Gestire l’evento clic](./forms/clickable-image-component/handle-click-event.md)
   + AEM Forms e Analytics{#forms-and-analytics}
      + [Introduzione](./forms/form-data-analytics/introduction.md)
      + [Creare elementi dati](./forms/form-data-analytics/data-elements.md)
      + [Creare le regole](./forms/form-data-analytics/rules.md)
      + [Testare la soluzione](./forms/form-data-analytics/test.md)
   + Creazione di un componente a discesa Paesi{#countries-drop-down}
      + [Introduzione](./forms/countries-drop-down/introduction.md)
      + [Creare un componente](./forms/countries-drop-down/component.md)
      + [Creare una finestra di dialogo](./forms/countries-drop-down/dialog.md)
      + [Creare un modello Sling](./forms/countries-drop-down/slingmodel.md)
      + [Creare e testare](./forms/countries-drop-down/build.md)
   + Creazione di varianti di pulsanti{#style-system}
      + [Introduzione](./forms/style-system/introduction.md)
      + [Definire un criterio](./forms/style-system/style-policy.md)
      + [Definire le varianti](./forms/style-system/create-variations.md)
      + [Testare le varianti](./forms/style-system/build.md)
   + Utilizzo delle schede verticali{#using-vertical-tabs}
      + [&#x200B;1. Introduzione](./forms/using-vertical-tabs/introduction.md)
      + [&#x200B;2. Creare un modulo](./forms/using-vertical-tabs/create-af.md)
      + [&#x200B;3. Navigazione](./forms/using-vertical-tabs/navigation.md)
      + [&#x200B;4. Aggiunta di icone](./forms/using-vertical-tabs/icons.md)
   + Utilizzo del servizio di output e moduli{#forms-cs-output-and-forms-service}
      + [Generare un PDF](./forms/forms-cs-output-and-forms-service/outputservice.md)
   + Generazione di documenti in AEM Forms CS{#doc-gen-formscs}
      + [Introduzione](./forms/doc-gen-forms-cs/introduction.md)
      + [Creare le credenziali del servizio](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Creare un token JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Creare un token di accesso](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Unire i dati con un modello](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Testare la soluzione](./forms/doc-gen-forms-cs/test.md)
      + [Sfida](./forms/doc-gen-forms-cs/challenge.md)
   + Utilizzo dell’API dei servizi per documenti di Forms{#forms-document-services-api}
      + [Introduzione](./forms/forms-document-services/introduction.md)
      + [Configurare OpenAPI](./forms/forms-document-services/using-open-api.md)
      + [Generare un token di accesso](./forms/forms-document-services/generate-access-token.md)
      + [Applicare i diritti di utilizzo](./forms/forms-document-services/make-api-calls.md)
      + [Codice di esempio](./forms/forms-document-services/sample-project.md)
   + Generazione di documenti tramite API in batch{#formscs-batch-api}
      + [Introduzione](./forms/formscs-batch-api/introduction.md)
      + [Configurare l’archiviazione Azure](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Creare una configurazione USC in batch](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Creare una configurazione in batch](./forms/formscs-batch-api/create-batch-config.md)
      + [Eseguire un batch](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Manipolazione di PDF in Forms CS{#forms-cs-assembler}
      + [Introduzione](./forms/forms-cs-assembler/introduction.md)
      + [Creare le credenziali del servizio](./forms/forms-cs-assembler/service-credentials.md)
      + [Creare un token JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Creare un token di accesso](./forms/forms-cs-assembler/create-access-token.md)
      + [Assemblare file PDF](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilità PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Testare la soluzione](./forms/forms-cs-assembler/test.md)
      + [Sfida](./forms/forms-cs-assembler/challenge.md)
   + Archiviare gli invii modulo con i tag dell’indice BLOB{#store-submiited-data-with-metadata-tags}
      + [Introduzione](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [Estendere un componente del gruppo di scelta](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [Creare una configurazione OSGi](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [Creare i tag dell’indice](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [Creare un invio personalizzato](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + Precompilare un modulo basato su componente core{#prefill-core-component-based-form}
      + [Introduzione](./forms/prefill-core-component-form/introduction.md)
      + [Scrivere un servizio di precompilazione](./forms/prefill-core-component-form/pre-fill-service.md)
      + [Testare la soluzione](./forms/prefill-core-component-form/test-solution.md)
   + Portale archiviazione di Azure{#forms-cs-azure-portal}
      + [Introduzione](./forms/forms-cs-azure-portal/introduction.md)
      + [Creare un modello di dati per moduli](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Archiviare dati modulo in Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Precompilare un modulo](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Invii di query](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Salvare e riprendere la compilazione di un modulo{#prefill-azure-storage}
      + [1- Introduzione](./forms/prefill-azure-storage/introduction.md)
      + [2- Creare il componente Pagina](./forms/prefill-azure-storage/page-component.md)
      + [3- Creare un modello per moduli adattivi](./forms/prefill-azure-storage/associate-page-component.md)
      + [4- Creare l’integrazione di archiviazione Azure](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - Creare l’integrazione SendGrid](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - Creare il modulo adattivo](./forms/prefill-azure-storage/create-af.md)
      + [7 - Implementare le risorse di esempio](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + Creare un flusso di lavoro di revisione{#create-aem-workflow}
      + [Esternalizzare l’archiviazione dei flussi di lavoro](./forms/create-aem-workflow/externalize-workflow.md)
      + [Creare un modello di flusso di lavoro](./forms/create-aem-workflow/create-workflow.md)
      + [Attivare un flusso di lavoro](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign con AEM Forms{#forms-and-sign}
      + [Introduzione](./forms/forms-and-sign/introduction.md)
      + [Applicazione API Acrobat Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configurazione cloud di Acrobat Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Creare un modulo adattivo](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurare per compilazione e firma](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integrare con Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configurare l’integrazione](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [Analizzare i dati inviati dai moduli](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [Inviare un DoR come allegato e-mail](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [Estrarre gli allegati del modulo dai dati inviati](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Integrare con Microsoft Dynamics{#formscs-dynamics-crm}
      + [Creare un’applicazione Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configurare l’origine dati](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Creare un modello di dati per moduli](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Creare un modulo adattivo](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Integrare con Salesforce{#integrate-with-salesforce}
      + [Introduzione](./forms/integrate-with-salesforce/introduction.md)
      + [Creare un’app connessa](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Creare un file Swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [Creare l’origine dati](./forms/integrate-with-salesforce/create-data-source.md)
      + [Creare un modello di dati per moduli](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [Inviare un modulo di prova](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [Testare un evento clic](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + Memorizzare gli invii di moduli in OneDrive e SharePoint{#one-drive}
      + [Memorizzare i dati del modulo in OneDrive](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [Memorizzare i dati del modulo in SharePoint](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [Precompilare il modulo con dati da elenco SharePoint](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [Inserire dati nell’elenco di SharePoint tramite un flusso di lavoro](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Estensibilità con Asset Compute{#asset-compute}
   + [Panoramica](./asset-compute/overview.md)
   + Configurazione{#set-up}
      + [Provisioning di account e servizi](./asset-compute/set-up/accounts-and-services.md)
      + [Ambiente di sviluppo locale](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + Sviluppare{#develop}
      + [Creare un progetto Asset Compute](./asset-compute/develop/project.md)
      + [Configurare le variabili di ambiente](./asset-compute/develop/environment-variables.md)
      + [Configurare il manifest.yml](./asset-compute/develop/manifest.md)
      + [Sviluppare un processo di lavoro](./asset-compute/develop/worker.md)
      + [Utilizzare lo strumento di sviluppo](./asset-compute/develop/development-tool.md)
   + Test e debug{#test-debug}
      + [Eseguire un test su un processo di lavoro](./asset-compute/test-debug/test.md)
      + [Debug di un processo di lavoro](./asset-compute/test-debug/debug.md)
   + Distribuzione{#deploy}
      + [Distribuzione in Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrare con AEM](./asset-compute/deploy/processing-profiles.md)
   + Avanzato{#advanced}
      + [Processi di lavoro metadati](./asset-compute/advanced/metadata.md)
   + [Risoluzione di problemi](./asset-compute/troubleshooting.md)

+ Tutorial in più passaggi{#multi-step-tutorials}
   + [Sviluppo AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=it){target=_blank}
   + [Editor SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/it/docs/experience-manager-learn/aem-target-tutorial/overview){target=_blank}
   + [Autenticazione basata su token](https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview){target=_blank}
+ Risorse da esperti {#expert-resources}
   + AEM Champions {#aem-champions}
      + [Playbook di onboarding per Cloud Manager](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Tipi di ambiente di Cloud Manager](./expert-resources/aem-champions/environment-types.md)
      + [Interfaccia utente di Cloud Manager](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [Serie AEM Experts](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [Introduzione](./expert-resources/cloud-5/cloud5-introduction.md)
      + [Stagione 4](./expert-resources/cloud-5/cloud5-season-4.md)
      + [Stagione 3](./expert-resources/cloud-5/cloud5-season-3.md)
      + [Stagione 2](./expert-resources/cloud-5/cloud5-season-2.md)
      + [Stagione 1](./expert-resources/cloud-5/cloud5-season-1.md)
      + [AEM CDN Parte 1](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN Parte 2](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [File di registro di AEM](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [Token di accesso](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Cloud Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [Migrazione 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Convalida dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [Ricerca e indicizzazione](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + Stagione 2{#season-2}
         + [Frammenti](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Repository Modernizer](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Modulo di pianificazione processi Sling](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [Correggere la cache](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [Correggere le riscritture](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager - Audit dell’esperienza](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager - Test unitari](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager - Test funzionali](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + Stagione 3{#season-3}
         + [Ricerca di terze parti](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
         + [Processi di lavoro Edge](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [Pubblicare, annullare pubblicazione di eventi in Edge Delivery Services](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [Indici di query e formule di Excel](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [Usare la propria CDN Cloudflare](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [Integrare AEM Assets](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
         + [IA generativa per AEM Sites](./expert-resources/cloud-5/season-3/cloud5-generative-ai-for-aem-sites.md)
         + [Introduzione all’editor universale](./expert-resources/cloud-5/season-3/cloud5-exploring-universal-editor.md)
         + [Importare siti](./expert-resources/cloud-5/season-3/cloud5-import-sites-to-edge-delivery-services.md)
         + [Utilizzo dell’API di amministrazione](./expert-resources/cloud-5/season-3/cloud5-using-admin-api.md)
         + [Ottimizzazione del punteggio Lighthouse - Parte 1](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part1.md)
         + [Ottimizzazione del punteggio Lighthouse - Parte 2](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part2.md)
         + [Ottimizzazione del punteggio Lighthouse - Parte 3](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part3.md)
      + Stagione 4{#season-4}
         + [Best practice](./expert-resources/cloud-5/season-4/cloud5-edge-delivery-services-best-practices.md)
         + [Ottimizzazioni per le ricerche](./expert-resources/cloud-5/season-4/cloud5-search-optimization.md)
         + [Google Maps](./expert-resources/cloud-5/season-4/cloud5-google-maps.md)
