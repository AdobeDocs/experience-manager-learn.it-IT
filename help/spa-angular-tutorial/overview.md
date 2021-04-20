---
title: Guida introduttiva ad AEM SPA Editor e Angular
description: Crea la tua prima applicazione per pagina singola angolare (SPA) modificabile in Adobe Experience Manager, AEM con l’applicazione a pagina singola WKND. Scopri come creare un’applicazione a pagina singola utilizzando il framework Angular JS con l’Editor SPA di AEM. Questa esercitazione in più parti illustra l’implementazione di un’applicazione Angular per un brand di lifestyle fittizio, il WKND. L’esercitazione illustra la creazione end-to-end dell’applicazione a pagina singola e l’integrazione con AEM.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '720'
ht-degree: 14%

---


# Crea la tua prima applicazione a pagina singola Angular in AEM {#introduction}

Ti diamo il benvenuto in un tutorial in più parti progettato per gli sviluppatori che hanno introdotto la funzionalità **Editor SPA** in Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un’applicazione Angular per un brand di lifestyle fittizio, il WKND. L’app Angular verrà sviluppata e progettata per essere implementata con l’Editor SPA di AEM, che mappa i componenti Angular ai componenti AEM. L’applicazione a pagina singola completata, implementata in AEM, può essere creata in modo dinamico con i tradizionali strumenti di modifica in linea di AEM.

![SPA finale implementata](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L’obiettivo di questa esercitazione in più parti è quello di insegnare a uno sviluppatore come implementare un’applicazione Angular per lavorare con la funzione Editor SPA di AEM. In uno scenario reale, le attività di sviluppo sono suddivise per tipo, spesso coinvolgendo uno **sviluppatore front-end** e uno **sviluppatore back-end**. Riteniamo utile per qualsiasi sviluppatore che sarà coinvolto in un progetto AEM SPA Editor di completare questa esercitazione.

Il tutorial è progettato per funzionare con **AEM as a Cloud Service** ed è retrocompatibile con **AEM 6.5.4+** e **AEM 6.4.8+**. L’applicazione a pagina singola è implementata utilizzando:

* [Archetipo di progetto AEM Maven](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/developing/archetype/overview.html)
* [Editor SPA di AEM](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componenti core](https://docs.adobe.com/content/help/it/experience-manager-core-components/using/introduction.html)
* [Angolare](https://angular.io/)

*Stimare 1-2 ore per passare attraverso ciascuna parte dell’esercitazione.*

## Codice più recente

Tutto il codice dell&#39;esercitazione si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base di codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetti AEM scaricabili.

## Prerequisiti

Prima di avviare questa esercitazione, è necessario quanto segue:

* Conoscenza di base di HTML, CSS e JavaScript
* Familiarità di base con [Angular](https://angular.io/)
* [SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) di AEM as a Cloud Service,  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.](https://nodejs.org/it/) jand  [npm](https://www.npmjs.com/)

*Sebbene non sia necessario, è utile avere una conoscenza di base sullo  [sviluppo dei componenti](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) AEM Sites tradizionali.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione, è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l’SDK di AEM as a Cloud Service in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Ti avvicini ora ad AEM 6.5?** Consulta la  [seguente guida per configurare un ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) di sviluppo locale.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avvia l’esercitazione andando al capitolo [Progetto editor SPA](create-project.md) e scopri come generare un progetto abilitato per l’editor SPA utilizzando AEM Project Archetype.

## Compatibilità con le versioni precedenti {#compatibility}

Il codice del progetto per questa esercitazione è stato creato per AEM as a Cloud Service. Per rendere il codice del progetto retrocompatibile per **6.5.4+** e **6.4.8+** sono state apportate diverse modifiche.

La [UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** è stata inclusa come dipendenza:

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

È stato aggiunto un profilo Maven aggiuntivo, denominato `classic`, per modificare la build per gli ambienti AEM 6.x:

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

Il profilo `classic` è disabilitato per impostazione predefinita. Se segui l’esercitazione con AEM 6.x, aggiungi il profilo `classic` ogni volta che ti viene richiesto di eseguire una build Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Quando si genera un nuovo progetto per un&#39;implementazione AEM, utilizza sempre la versione più recente di [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) e aggiorna `aemVersion` per eseguire il targeting della versione prevista di AEM.
