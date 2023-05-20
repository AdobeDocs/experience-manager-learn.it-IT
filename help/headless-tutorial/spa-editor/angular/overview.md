---
title: Guida introduttiva ad AEM SPA Editor e Angular
description: Crea la prima applicazione a pagina singola Angular modificabile in Adobe Experience Manager (AEM) con l’applicazione a pagina singola WKND.
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 20%

---

# Creazione del primo SPA Angular in AEM {#introduction}

Benvenuti in un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato **Editor SPA** in Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un’applicazione di Angular per un brand di lifestyle fittizio, WKND. L’app Angular è sviluppata e progettata per essere implementata con l’Editor SPA dell’AEM, che mappa i componenti Angular ai componenti AEM. L’SPA completo, implementato nell’AEM, può essere creato in modo dinamico con i tradizionali strumenti di modifica in linea dell’AEM.

![SPA finale implementato](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L’obiettivo di questo tutorial in più parti è insegnare a uno sviluppatore come implementare un’applicazione di Angular per lavorare con la funzione dell’editor SPA dell’AEM. In uno scenario reale le attività di sviluppo vengono suddivise per persona, spesso coinvolgendo un **Sviluppatore front-end** e un **Sviluppatore back-end**. Crediamo che sia utile per qualsiasi sviluppatore coinvolto in un progetto dell’Editor SPA dell’AEM completare questa esercitazione.

L’esercitazione è progettata per funzionare con **AEM as a Cloud Service** ed è retrocompatibile con **AEM 6.5.4+** e **AEM 6.4.8+**. L’SPA viene attuato utilizzando:

* [Archetipo di progetto AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=it)
* [Editor SPA AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componenti di base](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [Angular](https://angular.io/)

*Si stima che 1-2 ore passino attraverso ogni parte dell&#39;esercitazione.*

## Codice più recente

Tutto il codice del tutorial è disponibile su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

Il [base di codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetto AEM scaricabile.

## Prerequisiti

Prima di iniziare questa esercitazione, è necessario disporre dei seguenti elementi:

* Conoscenza di base di HTML, CSS e JavaScript
* Conoscenza di base di [Angular](https://angular.io/)
* [SDK AS A CLOUD SERVICE AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) e [npm](https://www.npmjs.com/)

*Anche se non obbligatorio, è utile avere una conoscenza di base di [sviluppo di componenti AEM Sites tradizionali](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=it).*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l’SDK as a Cloud Service per l’AEM in esecuzione in un ambiente Mac OS con [Codice di Visual Studio](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it).
>
> **Ti avvicini ora a AEM 6.5?** Consulta la sezione [guida seguente alla configurazione di un ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=it).

## Passaggi successivi {#next-steps}

Cosa state aspettando?! Avvia l’esercitazione passando al [Progetto editor SPA](create-project.md) e scopri come generare un progetto abilitato per l’editor SPA utilizzando l’archetipo di progetto AEM.

## Compatibilità con le versioni precedenti {#compatibility}

Il codice del progetto per questa esercitazione è stato creato per AEM as a Cloud Service. Per rendere il codice del progetto compatibile con le versioni precedenti per **6.5.4+** e **6.4.8+** sono state apportate diverse modifiche.

Il [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** è stato incluso come dipendenza:

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

Un profilo Maven aggiuntivo, denominato `classic` È stato aggiunto per modificare la build per gli ambienti AEM 6.x di destinazione:

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

Il `classic` Il profilo è disabilitato per impostazione predefinita. Se segui il tutorial con AEM 6.x, aggiungi `classic` profilo ogni volta che viene richiesto di eseguire una build Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Quando generi un nuovo progetto per un’implementazione AEM, utilizza sempre la versione più recente del [Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype) e aggiorna `aemVersion` per eseguire il targeting della versione desiderata di AEM.
