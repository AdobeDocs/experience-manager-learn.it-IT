---
title: Guida introduttiva all’editor di SPA di AEM e React
description: Crea la prima applicazione a pagina singola React modificabile in Adobe Experience Manager (AEM) con l’applicazione a pagina singola WKND. Scopri come creare un’applicazione a pagina singola utilizzando la piattaforma JS di React con l’editor per applicazioni a pagina singola di AEM. Questo tutorial in più parti illustra l’implementazione di un’applicazione React per un brand lifestyle fittizio, WKND. Il tutorial descrive tutte le fasi di creazione dell’applicazione a pagina singola e l’integrazione con AEM.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '417'
ht-degree: 100%

---

# Creare la prima applicazione a pagina singola React in AEM {#overview}

{{edge-delivery-services}}

Questo tutorial in più parti è progettato per gli sviluppatori che non hanno mai utilizzato la funzione **Editor di SPA** in Adobe Experience Manager (AEM). Questo tutoria illustra l’implementazione di un’applicazione React per un brand lifestyle fittizio, WKND. L’app React è sviluppata e progettata per essere implementata con l’editor SPA di AEM, che mappa i componenti React sui componenti AEM. L’applicazione a pagina singola completata, implementata in AEM, può essere creata dinamicamente con i tradizionali strumenti di modifica in linea di AEM.

![Applicazione a pagina singola finale implementata](assets/wknd-spa-implementation.png)

*Implementazione dell’applicazione a pagina singola WKND*

## Informazioni

Il tutorial è progettato per utilizzare **AEM as a Cloud Service** ed è compatibile con le versioni precedenti di **AEM 6.5.4+** e **AEM 6.4.8+**.

## Codice più recente

Tutto il codice utilizzato nel tutorial si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base del codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetto AEM scaricabile.

## Prerequisiti

Prima di iniziare questo tutorial, è necessario disporre dei seguenti elementi:

* Nozioni di base su HTML, CSS e JavaScript
* Familiarità di base con [React](https://reactjs.org/tutorial/tutorial.html)

*Anche se non obbligatorio, è utile avere una conoscenza di base di [sviluppo di componenti AEM Sites tradizionali](https://experienceleague.adobe.com/it/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview).*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questo tutorial è necessario un ambiente di sviluppo locale. Le schermate e i video sono stati acquisiti utilizzando AEM as a Cloud Service SDK in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=it), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=it#aem-65) o [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=it#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) e [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview).
>
> **Utilizzi AEM 6.5 per la prima volta?** Consulta la [guida seguente per configurare un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Passaggi successivi {#next-steps}

Per iniziare il tutorial, passa al capitolo [Crea progetto](create-project.md) e scopri come creare un progetto abilitato per l’editor SPA utilizzando l’archetipo di progetto AEM.
