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
duration: 71
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 30%

---

# Creare il primo SPA React nell’AEM {#overview}

{{edge-delivery-services}}

Questo tutorial in più parti è stato progettato per gli sviluppatori che non hanno mai utilizzato la funzionalità **SPA Editor** in Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un’applicazione React per un brand di lifestyle fittizio, WKND. L’app React è sviluppata e progettata per essere implementata con l’editor SPA dell’AEM, che mappa i componenti React sui componenti AEM. L’SPA completo, implementato nell’AEM, può essere creato in modo dinamico con i tradizionali strumenti di modifica in linea dell’AEM.

![SPA finale implementato](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L&#39;esercitazione è progettata per funzionare con **AEM as a Cloud Service** ed è compatibile con le versioni precedenti di **AEM 6.5.4+** e **AEM 6.4.8+**.

## Codice più recente

Tutto il codice del tutorial si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base di codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetti AEM scaricabili.

## Prerequisiti

Prima di iniziare questa esercitazione, è necessario disporre dei seguenti elementi:

* Conoscenza di base di HTML, CSS e JavaScript
* Familiarità di base con [React](https://reactjs.org/tutorial/tutorial.html)

*Anche se non obbligatorio, è utile avere una conoscenza di base di [sviluppo di componenti AEM Sites tradizionali](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it).*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l&#39;SDK AEM as a Cloud Service in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) o [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) e [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).
>
> **Nuovo AEM 6.5?** Consulta la [guida seguente per configurare un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Passaggi successivi {#next-steps}

Cosa state aspettando?! Avvia l&#39;esercitazione passando al capitolo [Crea progetto](create-project.md) e scopri come generare un progetto abilitato per l&#39;editor SPA utilizzando l&#39;archetipo di progetto AEM.
