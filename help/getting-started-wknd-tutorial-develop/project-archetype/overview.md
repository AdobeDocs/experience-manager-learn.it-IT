---
title: Guida introduttiva ad AEM Sites - Archetipo progetto
description: 'Guida introduttiva di AEM Sites: Archetipo progetto. L’esercitazione WKND è un’esercitazione in più parti progettata per sviluppatori che non hanno mai utilizzato Adobe Experience Manager. Il tutorial illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio, WKND. Il tutorial tratta argomenti fondamentali come la configurazione del progetto, gli archetipi Maven, i componenti core, i modelli modificabili, le librerie client e lo sviluppo di componenti.'
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 74
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 36%

---

# Guida introduttiva ad AEM Sites - Archetipo progetto {#project-archetype}

{{traditional-aem}}

Ti diamo il benvenuto in un tutorial in più parti, progettato per sviluppatori che utilizzano per la prima volta Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio, WKND.

Questa esercitazione inizia utilizzando [Archetipo progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it) per generare un nuovo progetto.

Il tutorial è progettato per funzionare con **AEM as a Cloud Service** ed è compatibile con le versioni precedenti di **AEM 6.5.14+**. Il sito viene implementato utilizzando:

* [Archetipo di progetto AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it)
* [Componenti di base](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=it)
* [Modelli Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Modelli modificabili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=it)
* [Sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=it)

*Il completamento di ogni parte del tutorial dovrebbe richiedere 1-2 ore.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questo tutorial è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando AEM as a Cloud Service SDK in esecuzione in un ambiente macOS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

È necessario installare localmente quanto segue:

* [Istanza ](https://experience.adobe.com/#/downloads) dell&#39;istanza **Author** di AEM locale (Cloud Service SDK o 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) (LTS - Supporto a lungo termine)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Codice Visual Studio](https://code.visualstudio.com/) o IDE equivalente
   * [AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) VSCode - Strumento utilizzato in tutta l&#39;esercitazione

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview).
>
> **Utilizzi AEM 6.5 per la prima volta?** Consulta la [guida seguente per configurare un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## GitHub {#github}

Il codice di questa esercitazione si trova su GitHub nell’archivio della guida di AEM:

**[GitHub: progetto WKND Sites](https://github.com/adobe/aem-guides-wknd)**

Inoltre, ogni parte dell’esercitazione ha un proprio ramo in GitHub. Un utente può iniziare l’esercitazione in qualsiasi momento semplicemente estraendo il ramo che corrisponde alla parte precedente.

## Passaggi successivi {#next-steps}

Cosa state aspettando? Avvia l&#39;esercitazione passando al capitolo [Configurazione del progetto](project-setup.md) e scopri come generare un nuovo progetto Adobe Experience Manager utilizzando l&#39;archetipo di progetto AEM.
