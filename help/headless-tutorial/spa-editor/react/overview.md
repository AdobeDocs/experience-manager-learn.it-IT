---
title: Guida introduttiva ad AEM SPA Editor e React
description: Crea la tua prima applicazione React a pagina singola (SPA) modificabile in Adobe Experience Manager AEM con il SPA WKND. Scopri come creare un SPA utilizzando il framework JS React con AEM Editor SPA. Questa esercitazione in più parti illustra l’implementazione di un’applicazione React per un brand di lifestyle fittizio, il WKND. L’esercitazione descrive la creazione end to end del SPA e l’integrazione con AEM.
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 12%

---

# Crea il tuo primo SPA React in AEM {#overview}

Ti diamo il benvenuto in un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato la funzione **SPA Editor** in Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un’applicazione React per un brand di lifestyle fittizio, il WKND. L’app React verrà sviluppata e progettata per essere implementata con AEM Editor, che mappa i componenti React su AEM componenti. Il SPA completato, distribuito a AEM, può essere creato dinamicamente con i tradizionali strumenti di modifica in linea di AEM.

![SPA finale implementato](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L&#39;esercitazione è progettata per funzionare con **AEM come Cloud Service** ed è retrocompatibile con **AEM 6.5.4+** e **AEM 6.4.8+**.

## Codice più recente

Tutto il codice dell&#39;esercitazione si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base di codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetti AEM scaricabili.

## Prerequisiti

Prima di avviare questa esercitazione, è necessario quanto segue:

* Conoscenza di base di HTML, CSS e JavaScript
* familiarità di base con [React](https://reactjs.org/tutorial/tutorial.html)

*Sebbene non sia necessario, è utile avere una comprensione di base dello  [sviluppo di componenti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) AEM Sites tradizionali.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione, è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l&#39;SDK di Cloud Service AEM in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

### Software richiesto

* [AEM come SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html) Cloud Service,  [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) o  [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.](https://nodejs.org/it/) jand  [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nuovo a AEM 6.5?** Consulta la  [seguente guida per configurare un ambiente](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) di sviluppo locale.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avvia l&#39;esercitazione andando al capitolo [Crea progetto](create-project.md) e scopri come generare un progetto abilitato per l&#39;editor di SPA utilizzando AEM Project Archetype.
