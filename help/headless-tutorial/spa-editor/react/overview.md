---
title: Guida introduttiva ad AEM SPA Editor e React
description: Crea la prima applicazione a pagina singola React modificabile in Adobe Experience Manager (AEM) con l’applicazione a pagina singola WKND. Scopri come creare un’applicazione a pagina singola utilizzando la piattaforma JS di React con l’editor per applicazioni a pagina singola di AEM. Questo tutorial in più parti illustra l’implementazione di un’applicazione React per un brand lifestyle fittizio, WKND. Il tutorial descrive tutte le fasi di creazione dell’applicazione a pagina singola e l’integrazione con AEM.
version: Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 81
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 30%

---

# Creare il primo SPA React nell’AEM {#overview}

{{edge-delivery-services}}

Benvenuti in un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato **Editor SPA** in Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un’applicazione React per un brand di lifestyle fittizio, WKND. L’app React è sviluppata e progettata per essere implementata con l’editor SPA dell’AEM, che mappa i componenti React sui componenti AEM. L’SPA completo, implementato nell’AEM, può essere creato in modo dinamico con i tradizionali strumenti di modifica in linea dell’AEM.

![SPA finale implementato](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L’esercitazione è progettata per funzionare con **AEM as a Cloud Service** ed è retrocompatibile con **AEM 6.5.4+** e **AEM 6.4.8+**.

## Codice più recente

Tutto il codice del tutorial è disponibile su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

Il [base di codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetto AEM scaricabile.

## Prerequisiti

Prima di iniziare questa esercitazione, è necessario disporre dei seguenti elementi:

* Conoscenza di base di HTML, CSS e JavaScript
* Conoscenza di base di [React](https://reactjs.org/tutorial/tutorial.html)

*Anche se non obbligatorio, è utile avere una conoscenza di base di [sviluppo di componenti AEM Sites tradizionali](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it).*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l’SDK as a Cloud Service per l’AEM in esecuzione in un ambiente Mac OS con [Codice di Visual Studio](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

* [SDK AS A CLOUD SERVICE AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) o [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) e [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).
>
> **Ti avvicini ora a AEM 6.5?** Consulta la sezione [guida seguente alla configurazione di un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Passaggi successivi {#next-steps}

Cosa state aspettando?! Avvia l’esercitazione passando al [Crea progetto](create-project.md) e scopri come generare un progetto abilitato per l’editor SPA utilizzando l’archetipo di progetto AEM.
