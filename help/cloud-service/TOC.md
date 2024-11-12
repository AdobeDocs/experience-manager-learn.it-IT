---
user-guide-title: Tutorial su Adobe Experience Manager as a Cloud Service
user-guide-description: Una raccolta di tutorial su Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutorial su AEM as a Cloud Service
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: 197f8b0d664971283cd893417a43e4e85e1b4923
workflow-type: tm+mt
source-wordcount: '1328'
ht-degree: 15%

---


# Tutorial su Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Panoramica](./overview.md)
+ Prove AEM {#aem-trials}
   + [Immagini](./aem-trials/images.md)
+ Playlist{#playlists}
   + [Sviluppo AEM](./playlists/development.md)
+ Introduzione ad AEM as a Cloud Service{#introduction}
   + [Cos’è AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Architettura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Strategia e leadership di pensiero{#strategy}
      + [Experience Manager - Modelli e archetipi per governance e personale](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Come velocizzare i contenuti con Adobe Experience Manager](./introduction/drive-content-velocity-for-sites.md)
+ Integrazioni Experience Cloud{#integrations}
   + [Integrazioni](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ Tecnologia sottostante {#underlying-technology}
   + [Architettura AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Archivio dei contenuti Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Servizi Author e Publish](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [Plug-in Sidekick AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [Programmi](./cloud-manager/programs.md)
   + [Ambienti](./cloud-manager/environments.md)
   + [Utilizzo di un archivio GitHub](./cloud-manager/byogithub.md)
   + [Pipeline CI/CD di produzione](./cloud-manager/cicd-production-pipeline.md)
   + [Pipeline CI/CD non di produzione](./cloud-manager/cicd-non-production-pipeline.md)
   + [Attività](./cloud-manager/activity.md)
   + [Nomi dominio personalizzati](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names){target=_blank}
   + Operazioni di sviluppo{#devops}
      + [Distribuzione del codice](./cloud-manager/devops/deploy-code.md)
      + [Unire i progetti](./cloud-manager/devops/merge-projects.md)
      + [Configurare le pipeline](./cloud-manager/devops/configure-pipelines.md)
      + [Integrazione continua](./cloud-manager/devops/continuous-integration.md)
      + [Analizzare i risultati del test](./cloud-manager/devops/analyze-test-results.md)
      + [Configurazioni di Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [Analisi del registro CDN](./cloud-manager/devops/cdn-log-analysis.md)
+ Impostazione ambiente di sviluppo locale {#local-development-environment-set-up}
   + [Panoramica](./local-development-environment/overview.md)
   + [Strumenti di sviluppo](./local-development-environment/development-tools.md)
   + [SDK AEM locale](./local-development-environment/aem-runtime.md)
   + [Strumenti Dispatcher locali](./local-development-environment/dispatcher-tools.md)
+ Sviluppo di {#developing}
   + Estendibilità{#extensibility}
      + App Builder{#app-builder}
         + [Genera token di accesso JWT](./developing/extensibility/app-builder/jwt-auth.md)
         + [Genera token di accesso server-to-server](./developing/extensibility/app-builder/server-to-server-auth.md)
         + [Verifica webhook Github](./developing/extensibility/app-builder/github-webhook-verification.md)
      + Estendibilità interfaccia utente{#ui}
         + [Panoramica](./developing/extensibility/ui/overview.md)
         + [Progetto Adobe Developer Console](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [Inizializza app](./developing/extensibility/ui/app-initialization.md)
         + [Registra estensione](./developing/extensibility/ui/extension-registration.md)
         + [Finestra modale](./developing/extensibility/ui/modal.md)
         + [Azione Adobe I/O Runtime](./developing/extensibility/ui/runtime-action.md)
         + [Verifica](./developing/extensibility/ui/verify.md)
         + [Distribuzione](./developing/extensibility/ui/deploy.md)
         + Frammenti di contenuto{#content-fragments}
            + [Panoramica](./developing/extensibility/ui/content-fragments/overview.md)
            + Esempi{#examples}
               + [Generazione di immagini AI](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [Aggiornamento proprietà in blocco](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [Colonne griglia personalizzate](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [Esporta come XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [Pulsante barra degli strumenti Editor Rich Text](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [Widget editor Rich Text](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [Badge RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
               + [Campi personalizzati](./developing/extensibility/ui/content-fragments/examples/editor-custom-field.md)
   + Nozioni di base sullo sviluppo{#basics}
      + [SDK AEM](./developing/basics/aem-sdk.md)
      + [Ambiente di sviluppo locale](./developing/basics/local-development-environment.md)
      + [Archetipo progetto AEM](./developing/basics/aem-project-archetype.md)
      + [Struttura dei progetti AEM](./developing/basics/project-structure.md)
      + [Contenuto variabile e immutabile](./developing/basics/mutable-immutable.md)
      + [Pacchetto struttura archivio](./developing/basics/repository-structure-package.md)
      + [Pubblicazione dei contenuti](./developing/basics/content-publishing.md)
      + [Configurazioni OSGi](./developing/basics/osgi-configurations.md)
      + [Migrazione configurazione Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Progetti AEM{#aem-projects}
      + [Progetto AEM Maven](./developing/projects/maven-project-structure.md)
      + [Pulizia di un progetto AEM Maven](./developing/projects/remove-samples.md)
   + Servizi OSGi{#osgi-services}
      + [Nozioni di base sui servizi OSGi](./developing/osgi-services/basics.md)
      + [Ciclo di vita del componente OSGi](./developing/osgi-services/lifecycle.md)
      + [Nozioni di base sulle configurazioni OSGi](./developing/osgi-services/configurations.md)
      + [Configurazioni OSGi con OCD](./developing/osgi-services/configurations-ocd.md)
   + Avanzate{#advanced}
      + [Caching delle varianti di pagina](./developing/advanced/variant-caching.md)
      + [Protezione CSRF](./developing/advanced/csrf-protection.md)
      + [Spazi dei nomi personalizzati](./developing/advanced/custom-namespaces.md)
      + [Parametrizza modelli Sling da HTL](./developing/advanced/sling-model-parameters.md)
      + [Segreti](./developing/advanced/secrets.md)
      + [Utenti del servizio](./developing/advanced/service-users.md)
      + [API per immagini ottimizzate per il web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [Esegui processo sull’istanza principale in AEM Author](./developing/advanced/run-job-on-leader-instance-in-aem-author.md)
   + Ambiente di sviluppo rapido{#rde}
      + [Panoramica](./developing/rde/overview.md)
      + [Come impostare](./developing/rde/how-to-setup.md)
      + [Come usare](./developing/rde/how-to-use.md)
      + [Ciclo di vita dello sviluppo](./developing/rde/development-life-cycle.md)
   + Editor universale{#universal-editor}
      + Modifica app React{#react-app-editing}
         + [Panoramica](./developing/universal-editor/react-app/overview.md)
         + [Configurazione sviluppo locale](./developing/universal-editor/react-app/local-development-setup.md)
         + [App react strumento](./developing/universal-editor/react-app/instrument-to-edit-content.md)
   + [JavaDocs API per AEM SDK](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ Debug di AEM{#debugging}
   + Debug dell&#39;SDK dell&#39;AEM{#debugging-aem-sdk}
      + [Panoramica](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registri](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Debug remoto](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Strumenti di Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Altri strumenti](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Debug di AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Panoramica](./debugging/cloud-service/overview.md)
      + [Registri](./debugging/cloud-service/logs.md)
      + [Build e implementazione](./debugging/cloud-service/build-and-deployment.md)
      + [Console di sviluppo](./debugging/cloud-service/developer-console.md)
      + [Browser dell’archivio](./debugging/cloud-service/repository-browser.md)
      + Rischi{#risks}
         + [Avvisi di attraversamento](./debugging/cloud-service/risks/traversals.md)
+ Consegna dei contenuti{#content-delivery}
   + [Nome di dominio personalizzato](./content-delivery/custom-domain-names.md)
   + [Nome di dominio personalizzato con CDN gestito da Adobe](./content-delivery/custom-domain-name-with-adobe-managed-cdn.md)
   + [Nome di dominio personalizzato con CDN cliente](./content-delivery/custom-domain-names-with-customer-managed-cdn.md)
   + [Memorizzazione in cache](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/overview){target=_blank}
   + [CDN Adobe - oltre la memorizzazione nella cache](./content-delivery/adobe-cdn-beyond-caching.md)
   + [Pagine di errore personalizzate](./content-delivery/custom-error-pages.md)
   + [Reindirizzamenti URL](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ Memorizzazione in cache{#caching}
   + [Panoramica](./caching/overview.md)
   + [Servizio Publish AEM](./caching/publish.md)
   + [Servizio di authoring AEM](./caching/author.md)
   + [Analisi percentuale riscontri cache CDN](./caching/cdn-cache-hit-ratio-analysis.md)
   + Come{#how-to}
      + [Abilita caching](./caching/how-to/enable-caching.md)
      + [Disattiva caching](./caching/how-to/disable-caching.md)
      + [Elimina cache](./caching/how-to/purge-cache.md)
+ Accesso a AEM{#accessing}
   + [Panoramica](./accessing/overview.md)
   + [Utenti di Adobe IMS](./accessing/adobe-ims-users.md)
   + [Gruppi di utenti di Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Profili di prodotto di Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [Utenti, gruppi e autorizzazioni AEM](./accessing/aem-users-groups-and-permissions.md)
   + [Procedura dettagliata della configurazione dell’accesso a AEM](./accessing/walk-through.md)
+ Autenticazione{#authentication}
   + [Panoramica](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ Rete avanzata{#networking}
   + [Panoramica](./networking/advanced-networking.md)
   + [Uscita da porta flessibile](./networking/flexible-port-egress.md)
   + [Indirizzo IP di uscita dedicato](./networking/dedicated-egress-ip-address.md)
   + [Rete privata virtuale](./networking/vpn.md)
   + Esempi di codice{#examples}
      + [HTTP/HTTPS su porte non standard per l’uscita della porta flessibile](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [HTTP/HTTPS per indirizzo IP/VPN in uscita dedicato](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [Connessioni SQL con DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Connessioni SQL con API Java SQL](./networking/examples/sql-java-apis.md)
      + [Servizio di posta elettronica](./networking/examples/email-service.md)
+ Sicurezza {#security}
   + [Blocco di attacchi DoS/DDoS tramite le regole del filtro del traffico](./security/blocking-dos-attack-using-traffic-filter-rules.md)
   + Regole del filtro del traffico, incluse le regole di WAF{#traffic-filter-and-waf-rules}
      + [Panoramica](./security/traffic-filter-rules/overview.md)
      + [Come impostare](./security/traffic-filter-rules/how-to-setup.md)
      + [Esempi e analisi dei risultati](./security/traffic-filter-rules/examples-and-analysis.md)
      + [Best practice](./security/traffic-filter-rules/best-practices.md)
+ Evento AEM{#aem-eventing}
   + [Panoramica](./eventing/overview.md)
   + Esempi{#examples}
      + [Webhook - Ricezione di eventi AEM](./eventing/examples/webhook.md)
      + [Inserimento nel journal - Carica eventi AEM](./eventing/examples/journaling.md)
      + [Azione Adobe I/O Runtime - Ricezione di eventi AEM](./eventing/examples/runtime-action.md)
      + [Azione Adobe I/O Runtime - Elabora eventi AEM](./eventing/examples/event-processing-using-runtime-action.md)
+ Migrazione {#migration}
   + [Strumento trasferimento contenuti](./migration/content-transfer-tool.md)
   + [Importazione in blocco delle risorse](./migration/bulk-import.md)
   + Passaggio ad AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Introduzione](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Onboarding](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA e CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [Strumenti di modernizzazione dell’AEM](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernizzazione archivio](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microservizi Asset compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Ricerca e indicizzazione](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migrazione dei contenuti {#content-migration}
         + [Servizio di importazione in blocco](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Strumento trasferimento contenuti](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [Domande frequenti](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [Risoluzione dei problemi](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [Introduzione](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [Iscrizione digitale](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [Comunicazioni](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [Introduzione](./migration/cloud-acceleration-manager/introduction.md)
      + [Preparazione e Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Fase di implementazione](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Strumenti di refactoring del codice](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Code Repository Modernizer](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Convertitore indice](./migration/cloud-acceleration-manager/index-converter.md)
      + [Strumento Migrazione dei flussi di lavoro delle risorse](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navigazione in Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Utilizzo di Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [Frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ Forms{#forms}
   + Sviluppo per Forms as a Cloud Service{#developing-for-cloud-service}
      + [1 - Guida introduttiva](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - Installazione di IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - Configurazione Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 - Sincronizza IntelliJ con AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 - Creare un modulo](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - Gestore di invio personalizzato](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - Registrazione del servlet utilizzando il tipo di risorsa](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 - Abilitare i componenti di Forms Portal](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 - Includi Cloud Service e FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 - Configurazione cloud in base al contesto](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 - Invia a Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 - Implementazione nell’ambiente di sviluppo](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 - Aggiornamento dell’archetipo Maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + Crea modulo adattivo{#create-first-af}
      + [Introduzione](./forms/create-first-af/introduction.md)
      + [Crea tema](./forms/create-first-af/create-theme.md)
      + [Crea modello](./forms/create-first-af/create-template.md)
      + [Crea frammento](./forms/create-first-af/create-fragments.md)
      + [Crea modulo](./forms/create-first-af/create-af.md)
      + [Configura pannello principale](./forms/create-first-af/configure-root-panel.md)
      + [Configurare il pannello persone](./forms/create-first-af/configure-people-panel.md)
      + [Configura pannello reddito](./forms/create-first-af/configure-income-panel.md)
      + [Configurare il pannello delle risorse](./forms/create-first-af/configure-assets-panel.md)
      + [Configurare il pannello iniziale](./forms/create-first-af/configure-start-panel.md)
      + [Barra degli strumenti Aggiungi e Configura](./forms/create-first-af/add-configure-toolbar.md)
   + Servizio di invio personalizzato con modulo headless{#custom-submit-headless-forms}
      + [1 - Introduzione](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - Creare un servizio di invio personalizzato](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - Visualizzare la risposta](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + Crea componente blocco di indirizzi{#create-address-block}
      + [1 - Introduzione](./forms/create-address-block-component/introduction.md)
      + [2 - Configurazione](./forms/create-address-block-component/set-up.md)
      + [3 - Creare un componente](./forms/create-address-block-component/creating-address-component.md)
      + [4 - Distribuire il componente](./forms/create-address-block-component/deploy-your-project.md)
   + Crea componente immagine cliccabile{#clickable-image-component}
      + [1 - Introduzione](./forms/clickable-image-component/introduction.md)
      + [2 - Creare un componente](./forms/clickable-image-component/create-component.md)
      + [3 - Gestire l’evento clic](./forms/clickable-image-component/handle-click-event.md)
   + AEM Forms e Analytics{#forms-and-analytics}
      + [Introduzione](./forms/form-data-analytics/introduction.md)
      + [Creare elementi dati](./forms/form-data-analytics/data-elements.md)
      + [Creare le regole](./forms/form-data-analytics/rules.md)
      + [Testare la soluzione](./forms/form-data-analytics/test.md)
   + Creazione di varianti di pulsanti{#style-system}
      + [Introduzione](./forms/style-system/introduction.md)
      + [Definisci criterio](./forms/style-system/style-policy.md)
      + [Definisci varianti](./forms/style-system/create-variations.md)
      + [Varianti di prova](./forms/style-system/build.md)
   + Utilizzo di tabulazioni verticali{#using-vertical-tabs}
      + [1. Introduzione](./forms/using-vertical-tabs/introduction.md)
      + [2. Crea modulo](./forms/using-vertical-tabs/create-af.md)
      + [3. Navigazione](./forms/using-vertical-tabs/navigation.md)
      + [4. Aggiunta di icone](./forms/using-vertical-tabs/icons.md)
   + Utilizzo del servizio di output e moduli{#forms-cs-output-and-forms-service}
      + [Genera PDF](./forms/forms-cs-output-and-forms-service/outputservice.md)
   + Generazione di documenti in AEM Forms CS{#doc-gen-formscs}
      + [Introduzione](./forms/doc-gen-forms-cs/introduction.md)
      + [Crea credenziali servizio](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Crea token JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Crea token di accesso](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Unisci dati con modello](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Testare la soluzione](./forms/doc-gen-forms-cs/test.md)
      + [Sfida](./forms/doc-gen-forms-cs/challenge.md)
   + Utilizzo di DocAssurance API{#doc-assurance-api}
      + [Frammenti di codice di esempio](./forms/doc-assurance-api/using-doc-assurance-api.md)
   + Generazione di documenti tramite API Batch{#formscs-batch-api}
      + [Introduzione](./forms/formscs-batch-api/introduction.md)
      + [Configura archiviazione Azure](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Crea configurazione batch USC](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Crea configurazione batch](./forms/formscs-batch-api/create-batch-config.md)
      + [Esegui batch](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Manipolazione di PDF in Forms CS{#forms-cs-assembler}
      + [Introduzione](./forms/forms-cs-assembler/introduction.md)
      + [Crea credenziali servizio](./forms/forms-cs-assembler/service-credentials.md)
      + [Crea token JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Crea token di accesso](./forms/forms-cs-assembler/create-access-token.md)
      + [Assembla file PDF](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilità PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Testare la soluzione](./forms/forms-cs-assembler/test.md)
      + [Sfida](./forms/forms-cs-assembler/challenge.md)
   + Integrare con Marketo{#froms-cs-with-marketo}
      + [Introduzione](./forms/forms-cs-with-marketo/part1.md)
      + [Creazione di Data Source](./forms/forms-cs-with-marketo/part2.md)
      + [Crea modello dati modulo](./forms/forms-cs-with-marketo/part3.md)
   + Archivia invii modulo con tag indice BLOB{#store-submiited-data-with-metadata-tags}
      + [Introduzione](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [Estendi componente gruppo di scelta](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [Crea configurazione OSGi](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [Creare tag indice](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [Creare un invio personalizzato](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + Precompila modulo basato su Componente core{#prefill-core-component-based-form}
      + [Introduzione](./forms/prefill-core-component-form/introduction.md)
      + [Servizio di precompilazione scrittura](./forms/prefill-core-component-form/pre-fill-service.md)
      + [Testare la soluzione](./forms/prefill-core-component-form/test-solution.md)
   + Archiviazione portale di Azure{#forms-cs-azure-portal}
      + [Introduzione](./forms/forms-cs-azure-portal/introduction.md)
      + [Crea modello dati modulo](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Archivia dati modulo in Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Precompila modulo](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Invio query](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Salva e riprendi compilazione modulo{#prefill-azure-storage}
      + [1- Introduzione](./forms/prefill-azure-storage/introduction.md)
      + [2- Creare il componente Pagina](./forms/prefill-azure-storage/page-component.md)
      + [3- Creare un modello di modulo adattivo](./forms/prefill-azure-storage/associate-page-component.md)
      + [4- Creare l’integrazione di Azure Storage](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - Creare l’integrazione SendGrid](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - Creare il modulo adattivo](./forms/prefill-azure-storage/create-af.md)
      + [7 - Distribuire le risorse di esempio](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + Crea flusso di lavoro di revisione{#create-aem-workflow}
      + [Esternalizzazione dell’archiviazione dei flussi di lavoro](./forms/create-aem-workflow/externalize-workflow.md)
      + [Crea modello di flusso di lavoro](./forms/create-aem-workflow/create-workflow.md)
      + [Attiva flusso di lavoro](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign con AEM Forms{#forms-and-sign}
      + [Introduzione](./forms/forms-and-sign/introduction.md)
      + [Applicazione API Acrobat Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configurazione cloud Acrobat Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Creare un modulo adattivo](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configura per compilazione e firma](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integrare con Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configurare l’integrazione](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [Analizzare i dati del modulo inviati](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [Invia DoR come allegato e-mail](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [Estrarre gli allegati dai dati inviati](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Integrare con Microsoft Dynamics{#formscs-dynamics-crm}
      + [Crea applicazione Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configurare Data Source](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Crea modello dati modulo](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Creare un modulo adattivo](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Integrare con Salesforce{#integrate-with-salesforce}
      + [Introduzione](./forms/integrate-with-salesforce/introduction.md)
      + [Crea app connessa](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Crea file Swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [Crea origine dati](./forms/integrate-with-salesforce/create-data-source.md)
      + [Crea modello dati modulo](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [Invio modulo di prova](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [Evento clic di prova](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + Archivia gli invii di moduli in un&#39;unità e SharePoint{#one-drive}
      + [Memorizzare i dati del modulo in un&#39;unica unità](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [Memorizzare i dati del modulo in sharepoint](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [Precompila modulo con dati da elenco SharePoint](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [Inserire dati nell’elenco di SharePoint tramite il flusso di lavoro](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Estendibilità Asset compute{#asset-compute}
   + [Panoramica](./asset-compute/overview.md)
   + Configura{#set-up}
      + [Provisioning di account e servizi](./asset-compute/set-up/accounts-and-services.md)
      + [Ambiente di sviluppo locale](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + Sviluppa{#develop}
      + [Creazione di un progetto di Asset compute](./asset-compute/develop/project.md)
      + [Configurare le variabili di ambiente](./asset-compute/develop/environment-variables.md)
      + [Configurare manifest.yml](./asset-compute/develop/manifest.md)
      + [Sviluppa un lavoratore](./asset-compute/develop/worker.md)
      + [Utilizzare lo strumento di sviluppo](./asset-compute/develop/development-tool.md)
   + Test e debug{#test-debug}
      + [Eseguire il test di un lavoratore](./asset-compute/test-debug/test.md)
      + [Debug di un processo di lavoro](./asset-compute/test-debug/debug.md)
   + Distribuisci{#deploy}
      + [Implementare in Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrare con AEM](./asset-compute/deploy/processing-profiles.md)
   + Avanzate{#advanced}
      + [Lavoratori metadati](./asset-compute/advanced/metadata.md)
   + [Risoluzione dei problemi](./asset-compute/troubleshooting.md)

+ Tutorials con più passaggi{#multi-step-tutorials}
   + [Sviluppo AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=it){target=_blank}
   + [Editor SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites e Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [Autenticazione basata su token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ Risorse esperti {#expert-resources}
   + Campioni AEM {#aem-champions}
      + [Playbook di onboarding Cloud Manager](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Tipi di ambiente Cloud Manager](./expert-resources/aem-champions/environment-types.md)
      + [Interfaccia utente Cloud Manager](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [Serie di esperti AEM](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [Introduzione](./expert-resources/cloud-5/cloud5-introduction.md)
      + [Stagione 1](./expert-resources/cloud-5/cloud5-season-1.md)
      + [Stagione 2](./expert-resources/cloud-5/cloud5-season-2.md)
      + [Stagione 3](./expert-resources/cloud-5/cloud5-season-3.md)
      + [AEM CDN Parte 1](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN Parte 2](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [File di registro AEM](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [Token di accesso](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Cloud Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [Migrazione 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Convalida Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [Ricerca e indicizzazione](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + Stagione 2{#season-2}
         + [Frammenti](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Repo Modernizer](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Sling Job Scheduler](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [Correggi la cache](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [Correggi le riscritture](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager - Audit dell’esperienza](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager - Test di unità](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager - Test funzionali](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + Stagione 3{#season-3}
         + [Ricerca di terze parti](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
         + [Real User Monitoring (RUM)](./expert-resources/cloud-5/season-3/cloud5-rum.md)
         + [Lavoratori Edge](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [Publish, annulla la pubblicazione di eventi in Edge Delivery Services](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [Indici di query e formule di Excel](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [Porta la tua CDN di Cloudflare](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [Integrare AEM Assets](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
         + [IA generativa per AEM Sites](./expert-resources/cloud-5/season-3/cloud5-generative-ai-for-aem-sites.md)
         + [Esplorazione dell’editor universale](./expert-resources/cloud-5/season-3/cloud5-exploring-universal-editor.md)
         + [Importa siti](./expert-resources/cloud-5/season-3/cloud5-import-sites-to-edge-delivery-services.md)
         + [Utilizzo dell’API di amministrazione](./expert-resources/cloud-5/season-3/cloud5-using-admin-api.md)
         + [Ottimizzazione punteggio faro - Parte1](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part1.md)
         + [Ottimizzazione punteggio faro - Parte2](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part2.md)
         + [Ottimizzazione punteggio faro - Parte3](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part3.md)

