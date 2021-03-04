---
title: 'Guida introduttiva ad AEM Sites: tutorial WKND'
description: 'Guida introduttiva ad AEM Sites: tutorial WKND. L’esercitazione WKND è un tutorial in più parti progettato per gli sviluppatori che hanno poca esperienza con Adobe Experience Manager. Il tutorial illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio, il WKND. L’esercitazione tratta argomenti fondamentali come la configurazione del progetto, gli archetipi Maven, i componenti core, i modelli modificabili, le librerie client e lo sviluppo dei componenti.'
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Componenti core, Editor pagina, Modelli modificabili, AEM Project Archetype
topic: Gestione dei contenuti, sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 14%

---


# Guida introduttiva ad AEM Sites: tutorial WKND {#introduction}

Ti diamo il benvenuto in un tutorial in più parti progettato per gli sviluppatori che hanno poca esperienza con Adobe Experience Manager (AEM). Questa esercitazione illustra l’implementazione di un sito AEM per un brand di lifestyle fittizio WKND. L’esercitazione tratta argomenti fondamentali come la configurazione del progetto, i componenti core, i modelli modificabili, le librerie lato client e lo sviluppo di componenti con Adobe Experience Manager Sites.

## Panoramica {#wknd-tutorial-overview}

L’obiettivo di questa esercitazione in più parti è insegnare agli sviluppatori come implementare un sito web utilizzando gli standard e le tecnologie più recenti in Adobe Experience Manager (AEM). Dopo aver completato questa esercitazione, uno sviluppatore deve comprendere le basi di base della piattaforma e con la conoscenza dei pattern di progettazione comuni in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

Il tutorial è progettato per funzionare con **AEM as a Cloud Service** ed è retrocompatibile con **AEM 6.5.5.0+** e **AEM 6.4.8.1+**. Il sito viene implementato utilizzando:

* [Archetipo di progetto AEM Maven](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componenti core](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelli Sling
* [Modelli modificabili](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema di stili](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Stimare 1-2 ore per passare attraverso ciascuna parte dell’esercitazione.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione, è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l’SDK di AEM as a Cloud Service in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

È necessario installare localmente quanto segue:

* Istanza locale di AEM **Author** (SDK di Cloud Service, 6.5.5+ o 6.4.8.1+)
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
> **Ti avvicini ora ad AEM 6.5?** Consulta la  [seguente guida per configurare un ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) di sviluppo locale.

## Informazioni sull&#39;esercitazione {#about-tutorial}

Il WKND è una rivista e blog fittizia online che si concentra sulla vita notturna, le attività e gli eventi in diverse città internazionali.

### Kit interfaccia utente Adobe XD

Per avvicinare questa esercitazione a uno scenario reale, i designer UX di talento di Adobe hanno creato i modelli per il sito utilizzando [Adobe XD](https://www.adobe.com/products/xd.html). Nel corso dell’esercitazione, diverse parti delle progettazioni vengono implementate in un sito AEM completamente accessibile all’autore. Un ringraziamento speciale a **Lorenzo Buosi** e **Kilian Amendola** che ha creato il bellissimo design per il sito WKND.

Scarica i kit di interfaccia utente XD:

* [Kit interfaccia utente dei componenti core AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit interfaccia utente WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

Il nome WKND è adatto perché ci aspettiamo che uno sviluppatore prenda la parte migliore di un ***weekend*** per completare l&#39;esercitazione.

### Github {#github}

Tutto il codice del progetto si trova su Github nell’archivio della Guida AEM:

**[GitHub: Progetto Siti WKND](https://github.com/adobe/aem-guides-wknd)**

Inoltre, ogni parte dell’esercitazione ha un proprio ramo in GitHub. Un utente può iniziare l&#39;esercitazione in qualsiasi momento semplicemente selezionando il ramo che corrisponde alla parte precedente.

>[!NOTE]
>
> Se stavi lavorando con la versione precedente di questa esercitazione, puoi comunque trovare i [pacchetti di soluzioni](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) e [codice](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) su GitHub.

## Sito di riferimento {#reference-site}

Una versione completa del sito WKND è disponibile anche come riferimento: [https://wknd.site/](https://wknd.site/)

L’esercitazione illustra le principali competenze di sviluppo necessarie per uno sviluppatore AEM, ma *non* consente di creare l’intero sito end-to-end. Il sito di riferimento completo è un’altra grande risorsa per esplorare e visualizzare ulteriori informazioni sulle funzionalità predefinite di AEM.

Per testare il codice più recente prima di passare all’esercitazione, scarica e installa la versione **[più recente da GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Alimentato da Adobe Stock

Molte delle immagini nel sito web WKND Reference provengono da [Adobe Stock](https://stock.adobe.com/) e sono materiale di terze parti come definito nei Termini aggiuntivi per le risorse demo all&#39;indirizzo [https://www.adobe.com/legal/terms.html](https://www.adobe.com/it/legal/terms.html). Se desideri utilizzare un’immagine di Adobe Stock per altri scopi oltre a visualizzare questo sito web dimostrativo, ad esempio la sua funzione su un sito web o nel materiale di marketing, puoi acquistare una licenza su Adobe Stock.

Con Adobe Stock, puoi accedere a oltre 140 milioni di immagini di alta qualità e prive di royalty, incluse foto, immagini, video e modelli per dare un nuovo impulso ai tuoi progetti creativi.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avvia l&#39;esercitazione andando al capitolo [Configurazione progetto](project-setup.md) e scopri come generare un nuovo progetto Adobe Experience Manager utilizzando AEM Project Archetype.
