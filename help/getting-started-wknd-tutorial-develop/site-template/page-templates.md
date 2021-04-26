---
title: Modelli di pagina
seo-title: Guida introduttiva ad AEM Sites - Modelli di pagina
description: Scopri come creare e modificare i modelli di pagina. Comprendere la relazione tra un modello di pagina e una pagina. Scopri come configurare i criteri di un modello di pagina per fornire governance granulare e coerenza del marchio per i contenuti.  Verrà creato un modello di articolo per la rivista ben strutturato basato su un modello di Adobe XD.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Gestione dei contenuti
feature: Componenti core, modelli modificabili, Editor pagina
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 3%

---


# Modelli di pagina {#page-templates}

>[!CAUTION]
>
> Le funzionalità di creazione rapida dei siti mostrate qui saranno rilasciate nella seconda metà del 2021. La relativa documentazione è disponibile a scopo di anteprima.

In questo capitolo esamineremo la relazione tra un modello di pagina e una pagina. Verrà creato un modello di articolo per riviste senza stile basato su alcuni modelli di [AdobeXD](https://www.adobe.com/products/xd.html). Durante il processo di creazione del modello, sono inclusi i componenti core e le configurazioni di policy avanzate.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti e si presume che i passaggi descritti nel capitolo [Creazione di contenuti e pubblicazione di modifiche](./author-content-publish.md) siano stati completati.

## Obiettivo

1. Inspect è una progettazione di pagina creata in Adobe XD e la associa a Componenti core.
1. Comprendi i dettagli dei modelli di pagina e come possono essere utilizzati i criteri per applicare il controllo granulare del contenuto della pagina.
1. Scopri come i modelli e le pagine sono collegati.

## Cosa verrà creato {#what-you-will-build}

In questa parte dell’esercitazione, verrà creato un nuovo modello Pagina articoli di Magazine che può essere utilizzato per creare nuovi articoli della rivista e allinearsi con una struttura comune. Il modello sarà basato sulle progettazioni e su un kit di interfaccia utente prodotto in AdobeXD. Questo capitolo si concentra solo sulla costruzione della struttura o dello scheletro del modello. Non verrà implementato alcun stile, ma il modello e le pagine funzioneranno.

## Pianificazione dell’interfaccia utente con Adobe XD {#adobexd}

Nella maggior parte dei casi, la pianificazione di un nuovo sito web inizia con modelli e progetti statici. [Adobe ](https://www.adobe.com/products/xd.html) XDè uno strumento di progettazione che crea esperienze utente. Ora esamineremo un kit di interfaccia utente e i modelli per pianificare la struttura del modello di pagina dell’articolo.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Scarica il file di progettazione dell’articolo  [WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> È disponibile anche un [AEM kit interfaccia utente per componenti core ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) generico come punto di partenza per i progetti personalizzati.

## Crea il modello di pagina dell’articolo per la rivista

Quando si crea una pagina, è necessario selezionare un modello da utilizzare come base per la creazione della nuova pagina. Il modello definisce la struttura della pagina risultante, il contenuto iniziale e i componenti consentiti.

Ci sono 3 aree principali di [Modelli di pagina](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **Struttura** : definisce i componenti che fanno parte del modello. Gli autori dei contenuti non potranno modificarli.
1. **Contenuto iniziale** : definisce i componenti con cui inizierà il modello, che possono essere modificati e/o eliminati dagli autori di contenuti
1. **Criteri** : definisce le configurazioni in base al comportamento dei componenti e alle opzioni disponibili per gli autori.

Quindi, crea un nuovo modello in AEM che corrisponda alla struttura dei modelli. Questo si verificherà in un&#39;istanza locale di AEM. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### Pacchetto soluzione

È possibile scaricare e installare una soluzione [completa del modello di Magazine](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip) tramite Gestione pacchetti.

## Aggiornare intestazione e piè di pagina con frammenti esperienza {#experience-fragments}

Una pratica comune durante la creazione di contenuto globale, ad esempio intestazione o piè di pagina, consiste nell’utilizzare un [Frammento esperienza](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Frammenti esperienza consente agli utenti di combinare più componenti per creare un singolo componente utilizzabile come riferimento. I frammenti esperienza hanno il vantaggio di supportare la gestione multisito e la [localizzazione](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

Il modello Sito generava un’intestazione e un piè di pagina. Quindi, aggiorna i frammenti esperienza in modo che corrispondano ai pattern. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Passaggi di alto livello per il video seguente:

1. Scarica il pacchetto di contenuti di esempio **[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**.
1. Carica e installa il pacchetto di contenuti utilizzando Gestione pacchetti.
1. Aggiornare i frammenti esperienza di intestazione e piè di pagina per utilizzare il logo WKND

## Creare una pagina dell’articolo di Magazine

Quindi, crea una nuova pagina utilizzando il modello Pagina articolo di Magazine . Crea il contenuto della pagina in modo che corrisponda ai modelli di sito. Segui i passaggi del video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato un nuovo modello e una nuova pagina con Adobe Experience Manager Sites.

### Passaggi successivi {#next-steps}

A questo punto la pagina dell&#39;articolo della rivista e il sito non corrispondono agli stili del marchio per WKND. Segui l’ esercitazione [Theming](theming.md) per scoprire le best practice per l’aggiornamento del codice front-end CSS e Javascript utilizzato per applicare gli stili globali al sito.

### Pacchetto soluzione

È disponibile un pacchetto della soluzione per questo capitolo da scaricare: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
