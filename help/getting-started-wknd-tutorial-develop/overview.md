---
title: 'Guida introduttiva ad AEM Sites: tutorial WKND'
description: 'Guida introduttiva ad AEM Sites: tutorial WKND. L’esercitazione WKND è un’esercitazione con più parti progettata per gli sviluppatori che hanno familiarità con Adobe Experience Manager. L''esercitazione illustra l''implementazione di un sito AEM per un marchio di stile di vita fittizio, il WKND. L''esercitazione tratta argomenti fondamentali come la configurazione del progetto, gli archetipi del cielo, i componenti core, i modelli modificabili, le librerie client e lo sviluppo di componenti.'
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
translation-type: tm+mt
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '893'
ht-degree: 13%

---


# Guida introduttiva ad AEM Sites: tutorial WKND {#introduction}

Questa esercitazione multiparte è stata progettata per gli sviluppatori che non hanno mai utilizzato Adobe Experience Manager (AEM). Questa esercitazione illustra l&#39;implementazione di un sito AEM per un marchio di stile di vita fittizio WKND. L&#39;esercitazione tratta argomenti fondamentali come la configurazione del progetto, i componenti core, i modelli modificabili, le librerie lato client e lo sviluppo di componenti con  Adobe Experience Manager Sites.

## Panoramica {#wknd-tutorial-overview}

L’obiettivo di questa esercitazione con più parti è insegnare a uno sviluppatore come implementare un sito Web utilizzando gli standard e le tecnologie più recenti in Adobe Experience Manager (AEM). Dopo aver completato questa esercitazione, uno sviluppatore deve comprendere le basi di base della piattaforma e conoscere i modelli di progettazione comuni in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

L&#39;esercitazione è progettata per funzionare con **AEM come Cloud Service** ed è retrocompatibile con **AEM 6.5.5.0+** e **AEM 6.4.8.1+**. Il sito è implementato utilizzando:

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componenti core](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelli Sling
* [Modelli modificabili](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema di stili](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Stimare 1-2 ore per passare attraverso ciascuna parte dell&#39;esercitazione.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione è necessario disporre di un ambiente di sviluppo locale. Gli screenshot e i video vengono acquisiti utilizzando l&#39;AEM come SDK di Cloud Service in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

È necessario installare localmente quanto segue:

* Istanza AEM locale **Author** (SDK per Cloud Service, 6.5.5+ o 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) (LTS - Supporto a lungo termine)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [IDE ](https://code.visualstudio.com/) codice di Visual Studio o equivalente
   * [VSCode AEM sincronizzazione](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Strumento utilizzato per tutta l&#39;esercitazione

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Per prima cosa AEM 6.5?** Per configurare un ambiente [ di sviluppo locale, consulta la ](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)seguente guida.

## Informazioni sull&#39;esercitazione {#about-tutorial}

Il WKND è una rivista e blog di fantasia che si concentra sulla vita notturna, le attività e gli eventi in diverse città internazionali.

###  Adobe XD UI Kit

Per avvicinare questa esercitazione a uno scenario reale  designer di talento  UX hanno creato i modelli per il sito utilizzando [ Adobe XD](https://www.adobe.com/products/xd.html). Nel corso dell&#39;esercitazione, varie parti delle progettazioni vengono implementate in un sito AEM completamente modificabile. Un ringraziamento speciale a **Lorenzo Buosi** e **Kilian Amendola** che ha creato il bel design per il sito WKND.

Scaricate i kit di interfaccia XD:

* [AEM Core Component UI Kit](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit interfaccia utente WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

Il nome WKND è adatto perché ci aspettiamo che uno sviluppatore prenda la parte migliore di un ***weekend*** per completare l&#39;esercitazione.

### Github {#github}

Tutto il codice per il progetto si trova su Github nella Guida AEM repo:

**[GitHub: Progetto Siti WKND](https://github.com/adobe/aem-guides-wknd)**

Inoltre, ciascuna parte dell&#39;esercitazione dispone di un proprio ramo in GitHub. Un utente può iniziare l&#39;esercitazione in qualsiasi momento semplicemente estraendo il ramo che corrisponde alla parte precedente.

>[!NOTE]
>
> Se lavoravi con la versione precedente di questa esercitazione, puoi comunque trovare i pacchetti [della soluzione](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) e [code](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) su GitHub.

## Sito di riferimento {#reference-site}

Una versione finale del sito WKND è disponibile anche come riferimento: [https://wknd.site/](https://wknd.site/)

L&#39;esercitazione illustra le principali competenze di sviluppo necessarie per uno sviluppatore AEM, ma *non* renderà disponibile l&#39;intero sito end-to-end. Il sito di riferimento finale è un&#39;altra grande risorsa per esplorare e vedere più AEM fuori dalla scatola funzionalità.

Per testare l&#39;ultimo codice prima di passare all&#39;esercitazione, scaricate e installate la versione **[più recente da GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Alimentato da  Adobe Stock

Molte delle immagini presenti nel sito Web WKND Reference provengono da [ Adobe Stock](https://stock.adobe.com/) e sono materiale di terze parti come definito nei Termini aggiuntivi per risorse demo all&#39;indirizzo [https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html). Se desiderate utilizzare un&#39;immagine Adobe Stock  per altri scopi oltre alla visualizzazione di questo sito Web dimostrativo, ad esempio la sua funzione su un sito Web o nel materiale di marketing, potete acquistare una licenza su  Adobe Stock.

Con  Adobe Stock, potete accedere a oltre 140 milioni di immagini di alta qualità, prive di royalty, tra cui foto, elementi grafici, video e modelli per dare un nuovo impulso ai vostri progetti creativi.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avviare l&#39;esercitazione andando al capitolo [Configurazione progetto](project-setup.md) e imparare a generare un nuovo progetto Adobe Experience Manager utilizzando AEM Project Archetype.
