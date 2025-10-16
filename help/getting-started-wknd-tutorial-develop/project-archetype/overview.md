---
title: Guida introduttiva di AEM Sites - Archetipo del progetto
description: 'Guida introduttiva di AEM Sites - Archetipo del progetto. Il tutorial WKND è un tutorial in più parti progettato per sviluppatori che non hanno mai utilizzato Adobe Experience Manager. Questo tutorial illustra l''implementazione di un sito AEM per un brand di lifestyle fittizio: WKND. Il tutorial offre una descrizione di argomenti fondamentali su Experience Manager come la configurazione del progetto, gli archetipi Maven, i Componenti core, i Modelli modificabili, le librerie client e lo sviluppo di componenti.'
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: display, noCatalog
duration: 74
source-git-commit: b612e2e36af8f56661a07577e979959c650564ee
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 100%

---

# Guida introduttiva di AEM Sites - Archetipo del progetto {#project-archetype}

{{traditional-aem}}

Ti diamo il benvenuto in un tutorial in più parti, progettato per sviluppatori che utilizzano per la prima volta Adobe Experience Manager (AEM). Questo tutorial illustra l&#39;implementazione di un sito AEM per un brand di lifestyle fittizio: WKND.

Questo tutorial inizia utilizzando [Archetipo progetto AEM](https://experienceleague.adobe.com/it/docs/experience-manager-core-components/using/developing/archetype/overview) per generare un nuovo progetto.

Il tutorial è progettato per funzionare con **AEM as a Cloud Service** ed è compatibile con le versioni precedenti di **AEM 6.5.14+**. Il sito viene implementato utilizzando:

* [Archetipo di progetto AEM Maven](https://experienceleague.adobe.com/it/docs/experience-manager-core-components/using/developing/archetype/overview)
* [Componenti di base](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=it&=it)
* [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Modelli modificabili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=it)
* [Sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=it)

*Il completamento di ogni parte del tutorial dovrebbe richiedere 1-2 ore.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questo tutorial è necessario un ambiente di sviluppo locale. Le schermate e i video sono stati acquisiti utilizzando l&#39;SDK di AEM as a Cloud Service in esecuzione in un ambiente macOS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

È necessario installare localmente quanto segue:

* [Istanza **di authoring** di AEM locale ](https://experience.adobe.com/#/downloads)&#x200B;(Cloud Service SDK o 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o versione successiva)
* [Node.js](https://nodejs.org/it/) (LTS - Supporto a lungo termine)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) o IDE equivalente
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync): strumento utilizzato durante tutto il tutorial

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview).
>
> **Utilizzi AEM 6.5 per la prima volta?** Consulta la [guida seguente per configurare un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## GitHub {#github}

Il codice di questo tutorial si trova su GitHub nell’archivio della guida di AEM:

**[GitHub: progetto WKND Sites](https://github.com/adobe/aem-guides-wknd)**

Inoltre, ogni parte del tutorial ha un proprio ramo in GitHub. Un utente può iniziare il tutorial in qualsiasi momento semplicemente controllando il ramo che corrisponde alla parte precedente.

## Passaggi successivi {#next-steps}

Cosa aspetti? Avvia i ltutorial passando al capitolo [Configurazione del progetto](project-setup.md) e scopri come generare un nuovo progetto Adobe Experience Manager utilizzando l’archetipo di progetto AEM.
