---
title: Modelli di pagina
description: Scopri come creare e modificare i modelli di pagina. Comprendere la relazione tra un modello di pagina e una pagina. Scopri come configurare i criteri di un modello di pagina per fornire governance granulare e coerenza del marchio per i contenuti.  Viene creato un modello di articolo di Magazine ben strutturato basato su un modello di Adobe XD.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 2%

---

# Modelli di pagina {#page-templates}

In questo capitolo esamineremo la relazione tra un modello di pagina e una pagina. Costruiremo un modello di articolo di Magazine senza stile basato su alcuni modelli da [AdobeXD](https://www.adobe.com/products/xd.html). Durante il processo di creazione del modello, sono inclusi i componenti core e le configurazioni di policy avanzate.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti e si presume che i passaggi descritti in [Modifica del contenuto dell’autore e pubblicazione](./author-content-publish.md) capitolo completato.

## Obiettivo

1. Comprendi i dettagli dei modelli di pagina e come possono essere utilizzati i criteri per applicare il controllo granulare del contenuto della pagina.
1. Scopri come i modelli e le pagine sono collegati.
1. Crea un nuovo modello e crea una pagina.

## Cosa verrà creato {#what-you-will-build}

In questa parte dell’esercitazione, verrà creato un nuovo modello Pagina articoli di Magazine che può essere utilizzato per creare nuovi articoli della rivista e allinearsi con una struttura comune. Il modello si basa sulle progettazioni e su un kit di interfaccia utente prodotto in AdobeXD. Questo capitolo si concentra solo sulla costruzione della struttura o dello scheletro del modello. Non vengono implementati stili, ma il modello e le pagine sono funzionanti.

## Crea il modello di pagina dell’articolo per la rivista

Quando crei una pagina devi selezionare un modello, che viene utilizzato come base per la creazione della nuova pagina. Il modello definisce la struttura della pagina risultante, il contenuto iniziale e i componenti consentiti.

Ci sono 3 aree principali di [Modelli di pagina](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=it):

1. **Struttura** - definisce i componenti che fanno parte del modello. Non sono modificabili dagli autori dei contenuti.
1. **Contenuto iniziale** - definisce i componenti con cui inizia il modello, che possono essere modificati e/o eliminati dagli autori di contenuti
1. **Criteri** - definisce le configurazioni sul comportamento dei componenti e sulle opzioni disponibili per gli autori.

Quindi, crea un nuovo modello in AEM che corrisponda alla struttura dei modelli. Questo si verificherà in un&#39;istanza locale di AEM. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

Puoi usare la seguente miniatura per identificare il modello (o caricarne uno personalizzato!)

![Articolo Miniatura del modello di pagina](./assets/page-templates/article-page-template-thumbnail.png)


### Pacchetto soluzione

Una fine [soluzione del modello Magazine](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) può essere scaricato e installato tramite Gestione pacchetti.

## Aggiornare intestazione e piè di pagina con frammenti esperienza {#experience-fragments}

Una pratica comune nella creazione di contenuto globale, ad esempio intestazione o piè di pagina, consiste nell’utilizzare un’ [Frammento esperienza](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Frammenti esperienza consente agli utenti di combinare più componenti per creare un singolo componente utilizzabile come riferimento. I frammenti esperienza hanno il vantaggio di supportare la gestione multisito e [localizzazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

Il modello Sito generava un’intestazione e un piè di pagina. Quindi, aggiorna i frammenti esperienza in modo che corrispondano ai pattern. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Passaggi di alto livello per il video seguente:

1. Scarica il pacchetto di contenuti di esempio **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Carica e installa il pacchetto di contenuti utilizzando Gestione pacchetti.
1. Aggiornare i frammenti esperienza di intestazione e piè di pagina per utilizzare il logo WKND

## Creare una pagina dell’articolo di Magazine

Quindi, crea una nuova pagina utilizzando il modello Pagina articolo di Magazine . Crea il contenuto della pagina in modo che corrisponda ai modelli di sito. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

Utilizza la [testo fornito](./assets/page-templates/la-skateparks-copy.txt) per popolare il corpo dell&#39;articolo.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato un nuovo modello e una nuova pagina con Adobe Experience Manager Sites.

### Passaggi successivi {#next-steps}

A questo punto la pagina dell&#39;articolo della rivista e il sito non corrispondono agli stili del marchio per WKND. Segui [Tema](theming.md) esercitazione per scoprire le best practice per aggiornare il codice front-end CSS e JavaScript utilizzato per applicare gli stili globali al sito.

### Pacchetto soluzione

È disponibile un pacchetto della soluzione per questo capitolo da scaricare: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
