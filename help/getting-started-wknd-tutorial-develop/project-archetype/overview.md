---
title: Guida introduttiva ad AEM Sites - Archetipo di progetto
description: Guida introduttiva ad AEM Sites - Archetipo di progetto. L’esercitazione WKND è un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato Adobe Experience Manager. Il tutorial illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio, il WKND. L’esercitazione tratta argomenti fondamentali come la configurazione del progetto, gli archetipi Maven, i componenti core, i modelli modificabili, le librerie client e lo sviluppo dei componenti.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 24%

---

# Guida introduttiva ad AEM Sites - Archetipo di progetto {#project-archetype}

Ti diamo il benvenuto in un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato Adobe Experience Manager (AEM). Questa esercitazione illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio WKND.

Questa esercitazione inizia utilizzando il [Archetipo di progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it) per generare un nuovo progetto.

L’esercitazione è progettata per lavorare con **AEM as a Cloud Service** ed è compatibile con le versioni precedenti **AEM 6.5.10+**. Il sito viene implementato utilizzando:

* [Archetipo AEM progetto Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)
* Modelli Sling
* [Modelli modificabili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=it)
* [Sistema di stili](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Stimare 1-2 ore per passare attraverso ciascuna parte dell’esercitazione.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione, è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l&#39;SDK AEM as a Cloud Service in esecuzione in un ambiente Mac OS con [Codice di Visual Studio](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

È necessario installare localmente quanto segue:

* [AEM locale **Autore** istanza](https://experience.adobe.com/#/downloads) (Cloud Service SDK, 6.5.10+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) (LTS - Supporto a lungo termine)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Codice di Visual Studio](https://code.visualstudio.com/) o IDE equivalenti
   * [Sincronizzazione AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Strumento utilizzato durante l’esercitazione

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).
>
> **Nuovo a AEM 6.5?** Consulta la sezione [guida alla configurazione di un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Github {#github}

Tutto il codice del progetto si trova su Github nel repository AEM Guide:

**[GitHub: Progetto Siti WKND](https://github.com/adobe/aem-guides-wknd)**

Inoltre, ogni parte dell’esercitazione ha un proprio ramo in GitHub. Un utente può iniziare l&#39;esercitazione in qualsiasi momento semplicemente selezionando il ramo che corrisponde alla parte precedente.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avvia l’esercitazione passando alla pagina [Configurazione del progetto](project-setup.md) questo capitolo e scopri come generare un nuovo progetto Adobe Experience Manager utilizzando AEM Project Archetype .
