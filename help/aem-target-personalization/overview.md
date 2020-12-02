---
title: Guida introduttiva ad AEM e Adobe Target
seo-title: Guida introduttiva ad AEM e Adobe Target
description: Un'esercitazione completa che mostra come creare e distribuire esperienze personalizzate utilizzando Adobe Experience Manager e  Adobe Target. Questa esercitazione illustra anche le diverse persone coinvolte nel processo finale e come collaborano tra di loro
seo-description: Un'esercitazione completa che mostra come creare e distribuire esperienze personalizzate utilizzando Adobe Experience Manager e  Adobe Target. Questa esercitazione illustra anche le diverse persone coinvolte nel processo finale e come collaborano tra di loro
translation-type: tm+mt
source-git-commit: c4ddafe392f74be8401f3ef6e07fc9d463d7620a
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 2%

---


# Guida introduttiva ad AEM e Adobe Target {#getting-started-with-aem-target}

AEM e Target sono entrambe soluzioni potenti con funzionalità apparentemente sovrapposte. A volte i clienti hanno difficoltà a comprendere come e quando utilizzare questi prodotti insieme per offrire esperienze personalizzate. Per offrire un&#39;esperienza ottimizzata a ogni utente finale, i diversi team all&#39;interno dell&#39;organizzazione dovrebbero collaborare strettamente e definire chi esegue cosa.

Questa esercitazione illustra tre scenari diversi per AEM e Target, per consentirvi di comprendere meglio cosa funziona per la vostra organizzazione e in che modo i diversi team collaborano.

* Scenario 1: Personalizzazione mediante AEM frammenti esperienza
* Scenario 2: Personalizzazione mediante Visual Experience Composer (Compositore esperienza visivo)
* Scenario 3: Personalizzazione di esperienze con pagine Web complete

## Personalizzazione mediante AEM frammenti esperienza {#personalization-using-aem-experience-fragment}

Per questo scenario, utilizzeremo AEM e Target. Ovviamente, entrambi i prodotti hanno i propri punti di forza e, quando si tratta di offrire esperienze personalizzate agli utenti del sito, è necessario **contenuto personalizzato (contenuto da AEM)** e un **modo intelligente (Target)** per distribuire questi contenuti in base a un utente specifico.

AEM consente di creare contenuti personalizzati, riunendo tutti i contenuti e le risorse in una posizione centrale per rafforzare la strategia di personalizzazione. AEM consente di creare facilmente contenuti per desktop, tablet e dispositivi mobili in un&#39;unica posizione senza scrivere codice. Non è necessario creare pagine per ogni dispositivo: AEM regola automaticamente ogni esperienza utilizzando il contenuto. Potete anche esportare i contenuti da AEM a  Adobe Target come offerte con un pulsante.

Ora disponiamo di contenuti personalizzati sotto forma di Offerte AEM in Target. Target consente di distribuire queste offerte su scala basata su una combinazione di approcci di machine learning basati su regole e basati su AI che includono variabili comportamentali, contestuali e offline.  Con Target potete facilmente impostare ed eseguire attività A/B e multivariato (MVT) per determinare le migliori offerte, contenuti ed esperienze.

**I** frammenti di esperienza rappresentano un enorme passo in avanti per collegare i creatori di contenuti/esperienze ai professionisti della personalizzazione che stanno portando i risultati aziendali utilizzando Target.

* AEM autori di editor di contenuti contenuti personalizzati come frammenti esperienza e relative varianti
* AEM esporta HTML frammento esperienza in Target &#x200B;
* In Target &#x200B; utilizza AEM markup dei frammenti esperienza come offerte nelle attività
* Target distribuisce HTML con frammento esperienza, AEM fornisce immagini di riferimento

   ![Personalizzazione mediante il diagramma dei frammenti esperienza](assets/personalization-use-case-1/use-case-1-diagram.png)

**Per implementare questo scenario, è necessario:**

* [Integrare AEM e  Adobe Target con Launch e  Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM e  Adobe Target con Cloud Services legacy](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato le integrazioni di cui sopra, è possibile esaminare  [lo scenario nel dettaglio](./personalization-use-case-1.md).***

## Personalizzazione mediante Visual Experience Composer (Compositore esperienza visivo)

Gli addetti al marketing possono apportare modifiche rapide sul sito Web senza modificare il codice per eseguire un test utilizzando  Adobe Target Visual Experience Composer (VEC). Il VEC è un&#39;interfaccia utente WYSIWYG (quello che vedi è quello che ottieni) che ti consente di creare e testare facilmente esperienze e offerte personalizzate nel contesto del sito. Potete creare esperienze e offerte per le attività Target trascinando e rilasciando, scambiando e modificando il layout e il contenuto di una pagina Web (o offerta) o di una pagina Web per dispositivi mobili.

VEC è una delle caratteristiche principali di  Adobe Target. Il VEC consente agli esperti di marketing e ai designer di creare e modificare contenuti utilizzando un&#39;interfaccia visiva. Molte scelte di progettazione possono essere effettuate senza dover modificare direttamente il codice. La modifica di HTML e JavaScript è possibile anche utilizzando le opzioni di modifica disponibili in Composer.

* Il contenuto risiede in AEM, e gli editor di contenuti creano e gestiscono le pagine del sito
* In Target vengono utilizzate AEM pagine del sito ospitate per eseguire test e personalizzare
* Target offre contenuti personalizzati
* Creazione di nuovo contenuto Net tramite  Adobe Target VEC
* Si applica sia ai siti ospitati AEM che ai siti ospitati non AEM

   ![Personalizzazione mediante il diagramma di Visual Experience Composer (Compositore esperienza visivo)](assets/personalization-use-case-3/use-case-diagram-3.png)

**Per implementare questo scenario, è necessario:**

* [Integrare AEM e  Adobe Target con Launch e  Adobe I/O](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato l&#39;integrazione di cui sopra, consente di esaminare in dettaglio  [lo scenario.](./personalization-use-case-3.md)***

## Personalizzazione di esperienze con pagine Web complete

L&#39;integrazione di Adobe Experience Manager con  Adobe Target consente di offrire un&#39;esperienza personalizzata agli utenti del sito. Inoltre, consente di comprendere meglio quali versioni del contenuto del sito Web migliorano al meglio le conversioni durante un periodo di test specificato. Ad esempio, un test A/B confronta due o più versioni del contenuto del sito Web per vedere quale incrementa maggiormente le conversioni, le vendite o altre metriche identificate. Un esperto di marketing può creare attività all&#39;interno  Adobe Target per comprendere in che modo gli utenti interagiscono con il contenuto del sito e in che modo questo influisce sulle metriche del sito.

* Il contenuto risiede in AEM, e gli editor di contenuti creano e gestiscono le pagine del sito
* In Target vengono utilizzate AEM pagine del sito ospitate per eseguire test e personalizzare
* Target offre contenuti personalizzati
* Nessun nuovo contenuto netto creato qui
* Si applica a siti AEM e non AEM

   ![diagramma](assets/personalization-use-case-2/use-case-2-diagram.png)

**Per implementare questo scenario, è necessario:**

* [Integrare AEM e  Adobe Target con Launch e  Adobe I/O](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato l&#39;integrazione di cui sopra, consente di esaminare in dettaglio  [lo scenario.](./personalization-use-case-2.md)***
