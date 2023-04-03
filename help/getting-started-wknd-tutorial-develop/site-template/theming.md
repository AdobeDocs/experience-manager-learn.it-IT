---
title: Flusso di lavoro dei temi | Creazione rapida di siti AEM
description: Scopri come aggiornare le origini dei temi di un sito Adobe Experience Manager per applicare stili specifici per il marchio. Scopri come utilizzare un server proxy per visualizzare un’anteprima live degli aggiornamenti CSS e JavaScript. Questa esercitazione illustra anche come distribuire gli aggiornamenti a un tema in un sito AEM utilizzando la pipeline Front End di Adobe Cloud Manager.
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 1%

---

# Flusso di lavoro dei temi {#theming}

In questo capitolo vengono aggiornate le origini dei temi di un sito Adobe Experience Manager per applicare stili specifici per il marchio. Impariamo a utilizzare un server proxy per visualizzare un’anteprima degli aggiornamenti CSS e Javascript mentre effettuiamo il codice per il sito live. Questa esercitazione illustra anche come distribuire gli aggiornamenti a un tema in un sito AEM utilizzando la pipeline Front End di Adobe Cloud Manager.

Alla fine il nostro sito viene aggiornato per includere gli stili che corrispondono al marchio WKND.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti e si presume che i passaggi descritti in [Modelli di pagina](./page-templates.md) capitolo completato.

## Obiettivi

1. Scopri come scaricare e modificare le origini dei temi di un sito.
1. Scopri come eseguire il codice rispetto al sito live per un’anteprima in tempo reale.
1. Comprendi il flusso di lavoro end-to-end della distribuzione degli aggiornamenti CSS e JavaScript compilati come parte di un tema utilizzando la pipeline front-end di Adobe Cloud Manager.

## Aggiornare un tema {#theme-update}

Quindi, apporta modifiche alle origini tema in modo che il sito corrisponda all’aspetto del marchio WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

Passi di alto livello per il video:

1. Crea un utente locale in AEM da utilizzare con un server di sviluppo proxy.
1. Scarica le origini di tema da AEM e apri utilizzando un IDE locale, come VSCode.
1. Modifica le origini dei temi e utilizza un server di sviluppo proxy per visualizzare in anteprima le modifiche CSS e JavaScript in tempo reale.
1. Aggiorna le origini dei temi in modo che l&#39;articolo della rivista corrisponda agli stili e ai modelli con brand WKND.

### File della soluzione

Scarica gli stili completati per la [Tema di esempio WKND](assets/theming/WKND-THEME-src-1.1.zip)

## Distribuire un tema utilizzando una pipeline front-end {#deploy-theme}

Distribuire gli aggiornamenti a un tema in un ambiente AEM utilizzando la pipeline front-end di Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Passi di alto livello per il video:

1. Creare un nuovo git [archivio in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Aggiungi il progetto origini tema all’archivio Git di Cloud Manager:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configura un [Pipeline front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) in Cloud Manager per distribuire il codice front-end.
1. Esegui la pipeline Front End per distribuire gli aggiornamenti all’ambiente AEM di destinazione.

### Operazioni di pronti contro termine di esempio

Ci sono un paio di esempi di repository GitHub che possono essere utilizzati come riferimento:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Utilizzato come esempio per progetti &quot;nel mondo reale&quot;.

## Congratulazioni.  {#congratulations}

Congratulazioni, hai appena aggiornato e distribuito un tema a AEM!

### Passaggi successivi {#next-steps}

Approfondisci l&#39;AEM dello sviluppo e scopri di più sulla tecnologia sottostante creando un sito utilizzando il [Archetipo di progetto AEM](../project-archetype/overview.md).
