---
title: Guida introduttiva ad AEM SPA Editor e Angular
description: Crea la prima applicazione a pagina singola Angular modificabile in Adobe Experience Manager (AEM) con l’applicazione a pagina singola WKND.
version: Experience Manager as a Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 123
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 14%

---

# Creare la prima applicazione a pagina singola Angular in AEM {#introduction}

{{edge-delivery-services}}

Questo tutorial in più parti è stato progettato per gli sviluppatori che non hanno mai utilizzato la funzionalità **Editor SPA** in Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un’applicazione Angular per un brand lifestyle fittizio, WKND. L’app Angular è sviluppata e progettata per essere implementata con l’Editor SPA di AEM, che mappa i componenti di Angular ai componenti di AEM. L’applicazione a pagina singola completata, implementata in AEM, può essere creata dinamicamente con i tradizionali strumenti di modifica in linea di AEM.

![Applicazione a pagina singola finale implementata](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L’obiettivo di questo tutorial in più parti è insegnare a uno sviluppatore come implementare un’applicazione Angular per funzionare con la funzione Editor SPA di AEM. In uno scenario reale le attività di sviluppo vengono suddivise per utente tipo, spesso coinvolgendo uno **sviluppatore front-end** e uno **sviluppatore back-end**. Questo tutorial è molto utile per gli sviluppatori coinvolti in un progetto dell’editor di applicazioni a pagina singola di AEM.

Il tutorial è progettato per funzionare con **AEM as a Cloud Service** ed è compatibile con le versioni precedenti di **AEM 6.5.4+** e **AEM 6.4.8+**. L’applicazione a pagina singola viene implementata utilizzando:

* [Archetipo di progetto AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it)
* [Editor SPA di AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componenti di base](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [Angular](https://angular.io/)

*Si stima che 1-2 ore saranno sufficienti per completare ogni parte dell&#39;esercitazione.*

## Codice più recente

Tutto il codice del tutorial si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base di codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetto AEM scaricabile.

## Prerequisiti

Prima di iniziare questa esercitazione, è necessario disporre dei seguenti elementi:

* Conoscenza di base di HTML, CSS e JavaScript
* Familiarità di base con [Angular](https://angular.io/)
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) e [npm](https://www.npmjs.com/)

*Anche se non obbligatorio, è utile avere una conoscenza di base di [sviluppo di componenti AEM Sites tradizionali](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it).*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando AEM as a Cloud Service SDK in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).
>
> **Ti avvicini ora ad AEM 6.5?** Consulta la [guida seguente per configurare un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Passaggi successivi {#next-steps}

Cosa state aspettando?! Avvia l&#39;esercitazione passando al capitolo [Progetto Editor SPA](create-project.md) e scopri come generare un progetto abilitato per l&#39;Editor SPA utilizzando l&#39;archetipo di progetto AEM.

## Compatibilità con le versioni precedenti {#compatibility}

Il codice del progetto per questa esercitazione è stato creato per AEM as a Cloud Service. Per rendere il codice del progetto compatibile con le versioni precedenti per **6.5.4+** e **6.4.8+** sono state apportate diverse modifiche.

[UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** è stato incluso come dipendenza:

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

È stato aggiunto un ulteriore profilo Maven, denominato `classic`, per modificare la build in modo da eseguire il targeting degli ambienti AEM 6.x:

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

Il profilo `classic` è disabilitato per impostazione predefinita. Se segui l&#39;esercitazione con AEM 6.x, aggiungi il profilo `classic` ogni volta che ti viene richiesto di eseguire una build Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Durante la generazione di un nuovo progetto per un&#39;implementazione AEM utilizza sempre la versione più recente del [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) e aggiorna `aemVersion` per eseguire il targeting della versione desiderata di AEM.
