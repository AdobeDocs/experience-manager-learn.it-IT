---
title: Flusso di lavoro Theming | Creazione rapida di siti AEM
description: Scopri come aggiornare le origini del tema di un sito Adobe Experience Manager per applicare stili specifici per il brand. Scopri come utilizzare un server proxy per visualizzare un’anteprima live degli aggiornamenti CSS e JavaScript. Questo tutorial illustra anche come distribuire aggiornamenti del tema in un sito AEM utilizzando la pipeline front-end di Adobe Cloud Manager.
version: Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 1%

---

# Flusso di lavoro Theming {#theming}

In questo capitolo vengono aggiornate le origini tema di un sito Adobe Experience Manager per applicare stili specifici per il brand. Scopri come utilizzare un server proxy per visualizzare un’anteprima degli aggiornamenti CSS e JavaScript mentre eseguiamo il codice rispetto al sito live. Questo tutorial illustra anche come distribuire aggiornamenti del tema in un sito AEM utilizzando la pipeline front-end di Adobe Cloud Manager.

Alla fine il nostro sito viene aggiornato per includere stili corrispondenti al brand WKND.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti in cui si presume che i passaggi descritti in [Modelli di pagina](./page-templates.md) capitolo sono stati completati.

## Obiettivi

1. Scopri come scaricare e modificare le origini del tema di un sito.
1. Scopri come confrontare il codice con il sito live per un’anteprima in tempo reale.
1. Comprendere il flusso di lavoro end-to-end per distribuire aggiornamenti compilati CSS e JavaScript come parte di un tema utilizzando la pipeline front-end di Adobe Cloud Manager.

## Aggiornare un tema {#theme-update}

Quindi, apporta modifiche alle sorgenti del tema in modo che il sito corrisponda all’aspetto del marchio WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

Passaggi di alto livello per il video:

1. Creare un utente locale in AEM da utilizzare con un server di sviluppo proxy.
1. Scarica le sorgenti del tema dall’AEM e aprile utilizzando un IDE locale, come VSCode.
1. Modifica le origini del tema e utilizza un server di sviluppo proxy per visualizzare in anteprima le modifiche CSS e JavaScript in tempo reale.
1. Aggiorna le sorgenti del tema in modo che l’articolo della rivista corrisponda agli stili e ai modelli del marchio WKND.

### File di soluzione

Scarica gli stili completati per [Tema di esempio WKND](assets/theming/WKND-THEME-src-1.1.zip)

## Distribuire un tema utilizzando una pipeline front-end {#deploy-theme}

Distribuire gli aggiornamenti a un tema in un ambiente AEM utilizzando la pipeline front-end di Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Passaggi di alto livello per il video:

1. Crea un nuovo Git [archivio in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Aggiungi il progetto delle origini del tema all’archivio Git di Cloud Manager:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configurare un [Pipeline front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) in Cloud Manager per distribuire il codice front-end.
1. Esegui la pipeline front-end per distribuire gli aggiornamenti all’ambiente AEM di destinazione.

### Esempio di pronti contro termine

Esistono un paio di esempi di repository GitHub che possono essere utilizzati come riferimento:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Utilizzato come esempio per progetti &quot;reali&quot;.

## Congratulazioni. {#congratulations}

Congratulazioni, hai appena aggiornato e implementato un tema per AEM!

### Passaggi successivi {#next-steps}

Approfondisci lo sviluppo dell’AEM e scopri di più sulla tecnologia di base creando un sito utilizzando [Archetipo progetto AEM](../project-archetype/overview.md).
