---
title: Modelli di pagina
description: Scopri come creare e modificare i modelli di pagina. Comprendere la relazione tra un modello di pagina e una pagina. Scopri come configurare i criteri di un modello di pagina per fornire governance granulare e coerenza del brand per i contenuti.  Viene creato un modello di articolo ben strutturato per la rivista, basato su un modello fornito da Adobe XD.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 0%

---

# Modelli di pagina {#page-templates}

In questo capitolo verrà esplorata la relazione tra un modello di pagina e una pagina. Verrà creato un modello di articolo non formattato per Magazine basato su alcuni modelli di [AdobeXD](https://www.adobe.com/products/xd.html). Durante la creazione del modello, vengono trattati i Componenti core e le configurazioni avanzate dei criteri.

## Prerequisiti {#prerequisites}

Questo è un tutorial in più parti e si presume che i passaggi descritti nel capitolo [Modifica contenuto e pubblicazione](./author-content-publish.md) siano stati completati.

## Obiettivo

1. Scopri i dettagli dei modelli di pagina e come utilizzare i criteri per applicare un controllo granulare del contenuto della pagina.
1. Scopri come vengono collegati modelli e pagine.
1. Crea un nuovo modello e crea una pagina.

## Cosa verrà creato {#what-you-will-build}

In questa parte dell&#39;esercitazione verrà creato un nuovo modello Pagina articolo rivista che può essere utilizzato per creare nuovi articoli e allinearli a una struttura comune. Il modello si basa sulle progettazioni e su un kit di interfaccia utente prodotto in AdobeXD. Questo capitolo si concentra solo sulla creazione della struttura o dell&#39;ossatura del modello. Non vengono implementati stili, ma il modello e le pagine funzionano.

## Crea il modello per pagina articolo rivista

Quando crei una pagina, devi selezionare un modello, che viene utilizzato come base per la creazione della nuova pagina. Il modello definisce la struttura della pagina risultante, il contenuto iniziale e i componenti consentiti.

Sono disponibili 3 aree principali di [Modelli di pagina](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=it):

1. **Struttura** - definisce i componenti che fanno parte del modello. Non sono modificabili dagli autori di contenuti.
1. **Contenuto iniziale** - definisce i componenti con cui inizia il modello, che possono essere modificati e/o eliminati dagli autori di contenuto
1. **Criteri**: definisce le configurazioni sul comportamento dei componenti e sulle opzioni disponibili per gli autori.

Quindi, crea un nuovo modello in AEM che corrisponda alla struttura dei modelli. Ciò si verificherà in un’istanza locale di AEM. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/3412999?quality=12&learn=on&captions=ita)

Puoi utilizzare la miniatura seguente per identificare il modello (o caricarne uno tuo!)

![Miniatura modello pagina articolo](./assets/page-templates/article-page-template-thumbnail.png)


### Pacchetto soluzione

È possibile scaricare e installare una [soluzione completata del modello di rivista](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) tramite Gestione pacchetti.

## Aggiornare intestazione e piè di pagina con frammenti esperienza {#experience-fragments}

Per la creazione di contenuto globale, ad esempio un&#39;intestazione o un piè di pagina, è in genere consigliabile utilizzare un [frammento di esperienza](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=it). Frammenti di esperienza, consente agli utenti di combinare più componenti per creare un singolo componente di riferimento. I frammenti di esperienza hanno il vantaggio di supportare la gestione multisito e la [localizzazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=it#localized-site-structure).

Il modello Sito ha generato un&#39;intestazione e un piè di pagina. Quindi, aggiorna i Frammenti esperienza in modo che corrispondano ai modelli. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/3447807?quality=12&learn=on&captions=ita)

Passaggi di alto livello per il video seguente:

1. Scarica il pacchetto di contenuti di esempio **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Carica e installa il pacchetto di contenuti utilizzando Gestione pacchetti.
1. Aggiorna i frammenti esperienza intestazione e piè di pagina per utilizzare il logo WKND

## Creare una pagina di articolo per la rivista

Quindi, crea una nuova pagina utilizzando il modello Pagina articolo rivista. Crea il contenuto della pagina in modo che corrisponda ai modelli del sito. Segui i passaggi descritti nel video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/343384?quality=12&learn=on&captions=ita)

Utilizza il testo [fornito](./assets/page-templates/la-skateparks-copy.txt) per popolare il corpo dell&#39;articolo.

## Congratulazioni. {#congratulations}

Congratulazioni, hai appena creato un nuovo modello e una nuova pagina con Adobe Experience Manager Sites.

### Passaggi successivi {#next-steps}

A questo punto la pagina dell’articolo della rivista e il sito non corrispondono agli stili del brand per WKND. Segui l&#39;esercitazione [Theming](theming.md) per scoprire le best practice per aggiornare il codice front-end CSS e JavaScript utilizzato per applicare stili globali al sito.

### Pacchetto soluzione

È possibile scaricare un pacchetto di soluzione per questo capitolo: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
