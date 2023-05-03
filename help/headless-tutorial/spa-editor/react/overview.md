---
title: Guida introduttiva ad AEM SPA Editor e React
description: Crea la prima applicazione a pagina singola React modificabile in Adobe Experience Manager (AEM) con l’applicazione a pagina singola WKND. Scopri come creare un’applicazione a pagina singola utilizzando la piattaforma JS di React con l’editor per applicazioni a pagina singola di AEM. Questo tutorial in più parti illustra l’implementazione di un’applicazione React per un brand lifestyle fittizio, WKND. Il tutorial descrive tutte le fasi di creazione dell’applicazione a pagina singola, e l’integrazione con AEM.
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Crea il tuo primo SPA React in AEM {#overview}

Ti diamo il benvenuto in un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato **Editor SPA** in Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un’applicazione React per un brand di lifestyle fittizio, il WKND. L’app React è sviluppata e progettata per essere implementata con AEM Editor, che mappa i componenti React su AEM componenti. Il SPA completato, distribuito a AEM, può essere creato dinamicamente con i tradizionali strumenti di modifica in linea di AEM.

![SPA finale implementato](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L’esercitazione è progettata per lavorare con **AEM as a Cloud Service** ed è compatibile con le versioni precedenti **AEM 6.5.4+** e **AEM 6.4.8+**.

## Codice più recente

Tutto il codice dell&#39;esercitazione si trova in [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base di codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetti AEM scaricabili.

## Prerequisiti

Prima di avviare questa esercitazione, è necessario quanto segue:

* Conoscenza di base di HTML, CSS e JavaScript
* Conoscenza di base con [Reagire](https://reactjs.org/tutorial/tutorial.html)

*Sebbene non sia necessario, è utile avere una comprensione di base di [sviluppo di componenti AEM Sites tradizionali](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it).*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione, è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l&#39;SDK AEM as a Cloud Service in esecuzione in un ambiente Mac OS con [Codice di Visual Studio](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

* [AEM SDK as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) o [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) e [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).
>
> **Nuovo a AEM 6.5?** Consulta la sezione [guida alla configurazione di un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avvia l’esercitazione passando alla pagina [Crea progetto](create-project.md) questo capitolo e scopri come generare un progetto abilitato per l’editor di SPA utilizzando AEM Project Archetype.
