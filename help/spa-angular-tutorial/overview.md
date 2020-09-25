---
title: Guida introduttiva all'editor SPA AEM e all'Angular
description: Create la vostra prima applicazione Angular Single Page (SPA) modificabile in Adobe Experience Manager, AEM con l'SPA WKND. Scoprite come creare un'app SPA utilizzando il framework Angular JS con AEM SPA Editor. Questa esercitazione multiparte illustra l'implementazione di un'applicazione Angular per un marchio di stile di vita fittizio, il WKND. L'esercitazione riguarda la creazione finale dell'SPA e l'integrazione con AEM.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 13%

---


# Crea la tua prima SPA Angolare in AEM {#introduction}

Questa esercitazione multiparte è stata progettata per gli sviluppatori che hanno introdotto la funzione **SPA Editor** in Adobe Experience Manager (AEM). Questa esercitazione illustra l&#39;implementazione di un&#39;applicazione Angular per un marchio di stile di vita fittizio, il WKND. L&#39;app Angular verrà sviluppata e progettata per essere implementata con AEM SPA Editor, che esegue la mappatura dei componenti Angular ai componenti AEM. L&#39;app SPA completa, implementata in AEM, può essere creata in modo dinamico con i tradizionali strumenti di modifica in linea di AEM.

![Implementazione definitiva dell&#39;SPA](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L&#39;obiettivo di questa esercitazione multi-parte è insegnare a uno sviluppatore come implementare un&#39;applicazione Angular per lavorare con la funzione SPA Editor di AEM. In uno scenario reale, le attività di sviluppo sono suddivise per persona, spesso con la partecipazione di uno sviluppatore **** Front End e uno sviluppatore **** Back End. Riteniamo che sia vantaggioso per qualsiasi sviluppatore che sarà coinvolto in un progetto AEM SPA Editor completare questa esercitazione.

L&#39;esercitazione è progettata per lavorare con **AEM come Cloud Service** ed è compatibile con le versioni precedenti con **AEM 6.5.4+** e **AEM 6.4.8+**. La SPA è implementata utilizzando:

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componenti core](https://docs.adobe.com/content/help/it-IT/experience-manager-core-components/using/introduction.html)
* [Angolare](https://angular.io/)

*Stimare 1-2 ore per passare attraverso ciascuna parte dell&#39;esercitazione.*

## Ultimo codice

Tutto il codice di esercitazione si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

L&#39; [ultima base](https://github.com/adobe/aem-guides-wknd-spa/releases) di codice è disponibile come AEM pacchetti scaricabili.

## Prerequisiti

Prima di iniziare questa esercitazione, è necessario disporre dei seguenti elementi:

* Conoscenza di base di HTML, CSS e JavaScript
* Conoscenza di base con [Angular](https://angular.io/)
* [AEM come SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)Cloud Service, [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.js](https://nodejs.org/it/) e [npm](https://www.npmjs.com/)

*Sebbene non sia necessario, è utile avere una conoscenza di base dello[sviluppo di componenti](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)AEM Sites  tradizionali.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione è necessario disporre di un ambiente di sviluppo locale. Gli screenshot e i video vengono acquisiti utilizzando l&#39;AEM come SDK di Cloud Service in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Per prima cosa AEM 6.5?** Per configurare un ambiente [di sviluppo locale, consulta la](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)seguente guida.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avviate l&#39;esercitazione andando al capitolo Progetto [di](create-project.md) SPA Editor e scoprite come generare un progetto abilitato per l&#39;editor SPA utilizzando il AEM Project Archetype.

## Compatibilità con le versioni precedenti {#compatibility}

Il codice del progetto per questa esercitazione è stato creato per AEM come Cloud Service. Per rendere il codice del progetto retrocompatibile per **6.5.4+** e **6.4.8+** sono state apportate diverse modifiche.

La [versione](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) UberJar **v6.4.4** è stata inclusa come dipendenza:

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

È stato aggiunto un altro profilo Maven denominato `classic` per modificare la build in ambienti AEM 6.x:

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

Il `classic` profilo è disabilitato per impostazione predefinita. Se si segue l&#39;esercitazione con AEM 6.x, aggiungere il `classic` profilo ogni volta che è richiesto di eseguire una build Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Quando si genera un nuovo progetto per un&#39;implementazione AEM, utilizzare sempre la versione più recente dell&#39; [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) e aggiornare `aemVersion` per eseguire il targeting della versione di AEM prevista.
