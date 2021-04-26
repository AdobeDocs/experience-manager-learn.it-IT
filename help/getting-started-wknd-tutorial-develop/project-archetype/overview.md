---
title: Guida introduttiva ad AEM Sites - Archetipo di progetto
description: Guida introduttiva ad AEM Sites - Archetipo di progetto. L’esercitazione WKND è un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato Adobe Experience Manager. Il tutorial illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio, il WKND. L’esercitazione tratta argomenti fondamentali come la configurazione del progetto, gli archetipi Maven, i componenti core, i modelli modificabili, le librerie client e lo sviluppo dei componenti.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Componenti core, Editor pagina, Modelli modificabili, Archetipo AEM progetto
topic: Gestione dei contenuti, sviluppo
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 20%

---


# Guida introduttiva ad AEM Sites - Archetipo di progetto {#project-archetype}

Ti diamo il benvenuto in un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato Adobe Experience Manager (AEM). Questa esercitazione illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio WKND.

Questa esercitazione inizia utilizzando [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) per generare un nuovo progetto.

L&#39;esercitazione è progettata per funzionare con **AEM come Cloud Service** ed è retrocompatibile con **AEM 6.5.5.0+** e **AEM 6.4.8.1+**. Il sito viene implementato utilizzando:

* [Archetipo AEM progetto Maven](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componenti core](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelli Sling
* [Modelli modificabili](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema di stili](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Stimare 1-2 ore per passare attraverso ciascuna parte dell’esercitazione.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione, è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l&#39;SDK di Cloud Service AEM in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

È necessario installare localmente quanto segue:

* AEM locale **Autore** istanza (Cloud Service SDK, 6.5.5+ o 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/)  (LTS - Supporto a lungo termine)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [IDE ](https://code.visualstudio.com/) codice di Visual Studio o equivalente
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  - Strumento utilizzato durante l&#39;esercitazione

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nuovo a AEM 6.5?** Consulta la  [seguente guida per configurare un ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) di sviluppo locale.

## Github {#github}

Tutto il codice del progetto si trova su Github nel repository AEM Guide:

**[GitHub: Progetto Siti WKND](https://github.com/adobe/aem-guides-wknd)**

Inoltre, ogni parte dell’esercitazione ha un proprio ramo in GitHub. Un utente può iniziare l&#39;esercitazione in qualsiasi momento semplicemente selezionando il ramo che corrisponde alla parte precedente.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avvia l&#39;esercitazione andando al capitolo [Configurazione progetto](project-setup.md) e scopri come generare un nuovo progetto Adobe Experience Manager utilizzando AEM Project Archetype.
