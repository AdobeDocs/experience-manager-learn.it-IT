---
title: Guida introduttiva ad AEM SPA Editor e Angular
description: Crea la tua prima applicazione a pagina singola di Angular (SPA) modificabile in Adobe Experience Manager, AEM con il SPA WKND. Scopri come creare un SPA utilizzando il framework JS di Angular con AEM Editor SPA. Questa esercitazione in più parti illustra l’implementazione di un’applicazione Angular per un brand di lifestyle fittizio, il WKND. L’esercitazione descrive la creazione end to end del SPA e l’integrazione con AEM.
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 10%

---

# Crea il tuo primo SPA Angular in AEM {#introduction}

Ti diamo il benvenuto in un tutorial in più parti progettato per gli sviluppatori che non hanno mai utilizzato la funzione **SPA Editor** in Adobe Experience Manager (AEM). Questo tutorial illustra l’implementazione di un’applicazione Angular per un brand di lifestyle fittizio, il WKND. L’app Angular verrà sviluppata e progettata per essere implementata con AEM editor di SPA, che mappa i componenti di Angular su componenti AEM. Il SPA completato, distribuito a AEM, può essere creato dinamicamente con i tradizionali strumenti di modifica in linea di AEM.

![SPA finale implementato](assets/wknd-spa-implementation.png)

*Implementazione SPA WKND*

## Informazioni su

L’obiettivo di questo tutorial in più parti è quello di insegnare a uno sviluppatore come implementare un’applicazione Angular per lavorare con la funzione Editor di SPA di AEM. In uno scenario reale, le attività di sviluppo sono suddivise per persona, spesso coinvolgendo uno **sviluppatore front-end** e uno **sviluppatore back-end**. Crediamo che sia utile per qualsiasi sviluppatore che sarà coinvolto in un progetto di editor SPA AEM completare questa esercitazione.

L&#39;esercitazione è progettata per funzionare con **AEM come Cloud Service** ed è retrocompatibile con **AEM 6.5.4+** e **AEM 6.4.8+**. L’SPA viene implementato utilizzando:

* [Archetipo AEM progetto Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [Editor SPA AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componenti core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=it)
* [Angular](https://angular.io/)

*Stimare 1-2 ore per passare attraverso ciascuna parte dell’esercitazione.*

## Codice più recente

Tutto il codice dell&#39;esercitazione si trova su [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base di codice più recente](https://github.com/adobe/aem-guides-wknd-spa/releases) è disponibile come pacchetti AEM scaricabili.

## Prerequisiti

Prima di avviare questa esercitazione, è necessario quanto segue:

* Conoscenza di base di HTML, CSS e JavaScript
* Familiarità di base con [Angular](https://angular.io/)
* [AEM come SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) Cloud Service,  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o successivo)
* [Node.](https://nodejs.org/it/) jand  [npm](https://www.npmjs.com/)

*Sebbene non sia necessario, è utile avere una comprensione di base dello  [sviluppo di componenti](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) AEM Sites tradizionali.*

## Ambiente di sviluppo locale {#local-dev-environment}

Per completare questa esercitazione, è necessario un ambiente di sviluppo locale. Le schermate e i video vengono acquisiti utilizzando l&#39;SDK di Cloud Service AEM in esecuzione in un ambiente Mac OS con [Visual Studio Code](https://code.visualstudio.com/) come IDE. I comandi e il codice devono essere indipendenti dal sistema operativo locale, salvo diversa indicazione.

>[!NOTE]
>
> **Ti avvicini adesso ad AEM as a Cloud Service?** Consulta la [seguente guida per configurare un ambiente di sviluppo locale utilizzando SDK di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nuovo a AEM 6.5?** Consulta la  [seguente guida per configurare un ambiente](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) di sviluppo locale.

## Passaggi successivi {#next-steps}

Cosa stai aspettando?! Avvia l&#39;esercitazione andando al capitolo [SPA Editor Project](create-project.md) e scopri come generare un progetto abilitato per l&#39;editor di SPA utilizzando l&#39;Archetipo di progetto AEM.

## Compatibilità con le versioni precedenti {#compatibility}

Il codice di progetto per questa esercitazione è stato creato per AEM come Cloud Service. Per rendere il codice del progetto retrocompatibile per **6.5.4+** e **6.4.8+** sono state apportate diverse modifiche.

La [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** è stata inclusa come dipendenza:

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

È stato aggiunto un profilo Maven aggiuntivo, denominato `classic`, per modificare la build in ambienti AEM 6.x target:

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

Quando si genera un nuovo progetto per un&#39;implementazione AEM, utilizza sempre la versione più recente di [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) e aggiorna `aemVersion` per eseguire il targeting della versione di AEM prevista.
