---
title: Guida introduttiva ad AEM e Adobe Target
seo-title: Guida introduttiva ad AEM e Adobe Target
description: Un tutorial end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando Adobe Experience Manager e Adobe Target. Questa esercitazione illustra anche le diverse figure coinvolte nel processo end to end e la loro collaborazione tra di loro
seo-description: Un tutorial end-to-end che mostra come creare e distribuire esperienze personalizzate utilizzando Adobe Experience Manager e Adobe Target. Questa esercitazione illustra anche le diverse figure coinvolte nel processo end to end e la loro collaborazione tra di loro
feature: Frammenti di esperienza
topic: Personalizzazione
role: Developer (Sviluppatore)
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 2%

---


# Guida introduttiva ad AEM e Adobe Target {#getting-started-with-aem-target}

AEM e Target sono entrambe soluzioni potenti con funzionalità apparentemente sovrapposte. A volte i clienti hanno difficoltà a comprendere come e quando utilizzare questi prodotti insieme per fornire un’esperienza personalizzata. Per offrire un’esperienza ottimizzata a ogni utente finale, i diversi team all’interno dell’organizzazione devono collaborare strettamente e definire le attività da intraprendere.

Questa esercitazione descrive tre scenari diversi per AEM e Target, utili per comprendere cosa funziona meglio per la tua organizzazione e in che modo i diversi team collaborano.

* Scenario 1 : Personalizzazione tramite Frammenti esperienza AEM
* Scenario 2 : Personalizzazione tramite Compositore esperienza visivo
* Scenario 3 : Personalizzazione delle esperienze di pagine web complete

## Personalizzazione tramite frammenti esperienza AEM {#personalization-using-aem-experience-fragment}

Per questo scenario, utilizzeremo AEM e Target. Chiaramente, entrambi i prodotti hanno i propri punti di forza e, quando si tratta di fornire esperienze personalizzate agli utenti del sito, è necessario **contenuti personalizzati (contenuti da AEM)** e un **modo intelligente (Target)** per distribuire questi contenuti in base a un utente specifico.

AEM consente di creare contenuti personalizzati, riunendo tutti i contenuti e le risorse in una posizione centrale per alimentare la strategia di personalizzazione. AEM consente di creare facilmente contenuti per desktop, tablet e dispositivi mobili in un’unica posizione senza scrivere codice. Non è necessario creare pagine per ogni dispositivo: AEM regola automaticamente ogni esperienza utilizzando il contenuto. Puoi anche esportare il contenuto da AEM ad Adobe Target come offerte con il pulsante di un pulsante.

Ora abbiamo contenuti personalizzati sotto forma di offerte da AEM in Target. Target ti consente di distribuire queste offerte su scala basata su una combinazione di approcci di apprendimento automatico basati su regole e basati su intelligenza artificiale che incorporano variabili comportamentali, contestuali e offline.  Con Target puoi facilmente impostare ed eseguire attività A/B e multivariate (MVT) per determinare le offerte, i contenuti e le esperienze migliori.

**I** frammenti di esperienza rappresentano un enorme passo avanti per collegare i creatori di contenuti/esperienze ai professionisti di personalizzazione che utilizzano Target per conseguire risultati di business.

* Gli autori dell’editor di contenuti AEM hanno personalizzato i contenuti come Frammenti esperienza e relative varianti
* AEM esporta HTML con frammento esperienza in Target &#x200B;
* Target &#x200B; utilizza il markup dei frammenti esperienza AEM come offerte nelle attività
* Target fornisce HTML per i frammenti esperienza, AEM fornisce immagini di riferimento

   ![Diagramma relativo alla personalizzazione tramite Frammenti esperienza](assets/personalization-use-case-1/use-case-1-diagram.png)

**Per implementare questo scenario, devi:**

* [Integrare AEM e Adobe Target utilizzando Launch e Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM e Adobe Target con i servizi cloud precedenti](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato le integrazioni di cui sopra, consente di esplorare  [lo scenario in dettaglio](./personalization-use-case-1.md).***

## Personalizzazione tramite Compositore esperienza visivo

Gli addetti al marketing possono apportare modifiche rapide al sito web senza modificare il codice necessario per eseguire un test utilizzando il Compositore esperienza visivo di Adobe Target. Il Compositore esperienza visivo è l’interfaccia utente WYSIWYG (What you see is what you get) che consente di creare e testare facilmente esperienze e offerte personalizzate nel contesto del sito. Puoi creare esperienze e offerte per le attività di Target trascinando e rilasciando, scambiando e modificando il layout e il contenuto di una pagina web (o offerta) o di una pagina web mobile.

Il Compositore esperienza visivo è una delle funzionalità principali di Adobe Target. Il Compositore esperienza visivo consente agli addetti al marketing e ai designer di creare e modificare il contenuto mediante un’interfaccia visiva. Molte scelte di progettazione possono essere fatte senza richiedere la modifica diretta del codice. La modifica di HTML e JavaScript è possibile anche utilizzando le opzioni di modifica disponibili nel compositore.

* Il contenuto risiede in AEM e gli editor di contenuti creano e gestiscono le pagine del sito
* Target utilizza le pagine del sito ospitate da AEM per eseguire test e personalizzazione
* Target offre contenuti personalizzati
* Creazione di nuovo contenuto netto tramite Adobe Target VEC
* Si applica sia ai siti ospitati da AEM che ai siti ospitati da non AEM

   ![Personalizzazione tramite diagramma del Compositore esperienza visivo](assets/personalization-use-case-3/use-case-diagram-3.png)

**Per implementare questo scenario, devi:**

* [Integrare AEM e Adobe Target utilizzando Launch e Adobe I/O](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato l’integrazione di cui sopra, consente di esplorare in dettaglio  [lo scenario .](./personalization-use-case-3.md)***

## Personalizzazione delle esperienze di pagine web complete

L’integrazione di Adobe Experience Manager con Adobe Target consente di fornire un’esperienza personalizzata agli utenti del sito. Inoltre, ti aiuta a capire meglio quali versioni del contenuto del sito web migliorano meglio le conversioni durante uno specifico periodo di test. Ad esempio, un test A/B confronta due o più versioni del contenuto del sito web per vedere quale incrementa al meglio le conversioni, le vendite o altre metriche identificate. Un addetto al marketing può creare attività all’interno di Adobe Target per comprendere in che modo gli utenti interagiscono con il contenuto del tuo sito e in che modo influisce sulle metriche del tuo sito.

* Il contenuto risiede in AEM e gli editor di contenuti creano e gestiscono le pagine del sito
* Target utilizza le pagine del sito ospitate da AEM per eseguire test e personalizzazione
* Target offre contenuti personalizzati
* Nessun nuovo contenuto netto creato qui
* Si applica sia ai siti AEM che a quelli non AEM

   ![diagramma](assets/personalization-use-case-2/use-case-2-diagram.png)

**Per implementare questo scenario, devi:**

* [Integrare AEM e Adobe Target utilizzando Launch e Adobe I/O](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato l’integrazione di cui sopra, consente di esplorare in dettaglio  [lo scenario .](./personalization-use-case-2.md)***
