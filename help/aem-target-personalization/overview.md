---
title: Guida introduttiva ad AEM e Adobe Target
description: Un tutorial end-to-end che mostra come creare e offrire esperienze personalizzate utilizzando Adobe Experience Manager e Adobe Target. In questo tutorial, scoprirai anche i diversi utenti tipo coinvolti nel processo end-to-end e come collaborano tra loro
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '846'
ht-degree: 100%

---

# Integrare AEM Sites e Adobe Target {#getting-started-with-aem-target}

AEM e Target sono entrambe soluzioni potenti con funzionalità apparentemente in comune. A volte la clientela ha difficoltà a capire come e quando utilizzare questi prodotti in combinazione per fornire esperienze personalizzate. Per offrire un’esperienza ottimizzata a ogni utente finale, i diversi team all’interno dell’organizzazione dovrebbero collaborare strettamente e definire i propri ruoli.

In questo tutorial, vengono trattati tre diversi scenari per AEM e Target, che consentono di comprendere quale sia la soluzione migliore per la tua organizzazione e come i diversi team collaborano.

* Scenario 1: personalizzazione utilizzando i Frammenti di esperienza AEM
* Scenario 2: personalizzazione utilizzando il Compositore esperienza visivo
* Scenario 3: personalizzazione delle esperienze di pagina web completa

## Personalizzazione utilizzando i Frammenti di esperienza AEM {#personalization-using-aem-experience-fragment}

Per questo scenario, verranno utilizzati AEM e Target. Chiaramente, entrambi i prodotti hanno i loro punti di forza e quando si tratta di fornire esperienze personalizzate agli utenti del sito, hai bisogno di **contenuto personalizzato (contenuto da AEM)** e un **modo intelligente (Target)** per fornire questi contenuti in base a un utente specifico.

AEM ti consente di creare contenuti personalizzati, riunendo tutti i contenuti e le risorse in una posizione centrale per alimentare la tua strategia di personalizzazione. AEM ti consente di creare facilmente contenuti per desktop, tablet e dispositivi mobili in un’unica posizione senza scrivere codice. Non è necessario creare pagine per ogni dispositivo: AEM regola automaticamente ogni esperienza utilizzando i contenuti. Puoi anche esportare il contenuto da AEM ad Adobe Target sotto forma di offerte premendo un pulsante.

Ora disponi di contenuti personalizzati sotto forma di offerte da AEM in Target. Target ti consente di fornire queste offerte su larga scala in base a una combinazione di approcci di machine learning basati sulle regole e basati su IA che incorporano variabili comportamentali, contestuali e offline.  Con Target puoi facilmente configurare ed eseguire attività di test A/B e multivariato (MVT) per determinare le offerte, i contenuti e le esperienze migliori.

I **frammenti di esperienza** rappresentano un enorme passo avanti per collegare i creatori di contenuti/esperienze ai professionisti della personalizzazione che promuovono i risultati aziendali utilizzando Target.

* L’editor di contenuti di AEM crea contenuti personalizzati come Frammenti di esperienza e relative varianti
* AEM esporta l’HTML dei frammenti di esperienza in Target
* Target utilizza il markup dei frammenti esperienza di AEM come offerte nelle attività
* Target fornisce l’HTML dei frammenti di contenuto, mentre AEM le immagini di riferimento

  ![Personalizzazione utilizzando il diagramma di frammenti di esperienza](assets/personalization-use-case-1/use-case-1-diagram.png)

**Per implementare questo scenario, è necessario:**

* [Integrare AEM e Adobe Target utilizzando tag e Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM e Adobe Target utilizzando i Servizi cloud precedenti](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato le integrazioni precedenti, esplora lo [scenario nei dettagli](./personalization-use-case-1.md).***

## Personalizzazione utilizzando il Compositore esperienza visivo

I marketer possono apportare modifiche rapide al proprio sito Web senza modificare il codice per eseguire un test utilizzando il Compositore esperienza visivo di Adobe Target. Il Compositore esperienza visivo è l’interfaccia utente di WYSIWYG (What you see is what you get) che consente di creare e testare esperienze ed offerte personalizzate nel contesto del sito. Puoi creare esperienze e offerte per le attività di Target trascinando, sostituendo e modificando il layout e il contenuto di una pagina Web (o offerta) oppure di una pagina Web per dispositivi mobili.

Il Compositore esperienza visivo è una delle funzionalità principali di Adobe Target. Il Compositore esperienza visivo consente ai marketer e ai designer di creare e modificare i contenuti mediante un’interfaccia visiva. Molte scelte di progettazione possono essere eseguite senza dover modificare direttamente il codice. È inoltre possibile modificare HTML e JavaScript utilizzando le opzioni di modifica disponibili nel compositore.

* Il contenuto si trova in AEM e gli editor di contenuti creano e gestiscono le pagine del sito
* Target utilizza le pagine del sito ospitate da AEM per eseguire test e personalizzazione
* Target fornisce contenuti personalizzati
* Il nuovo contenuto netto viene creato utilizzando il Compositore esperienza visivo di Adobe Target
* Si applica sia ai siti ospitati da AEM sia a quelli non ospitati da AEM

  ![Personalizzazione utilizzando il diagramma del Compositore esperienza visivo](assets/personalization-use-case-3/use-case-diagram-3.png)

**Per implementare questo scenario, è necessario:**

* [Integrare AEM e Adobe Target utilizzando tag e Adobe I/O](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato l’integrazione precedente, esplora lo scenario [ nei dettagli.](./personalization-use-case-3.md)***

## Personalizzazione delle esperienze di pagina web completa

L’integrazione di Adobe Experience Manager con Adobe Target ti aiuta a fornire un’esperienza personalizzata agli utenti del sito. Inoltre, ti aiuta anche a capire meglio quali versioni del contenuto del sito web contribuiscono a migliorare le conversioni durante un determinato periodo di test. Ad esempio, un test A/B confronta due o più versioni del contenuto del sito web per comprendere quale fa aumentare maggiormente le conversioni, le vendite o altre metriche identificate. Un marketer può creare attività all’interno di Adobe Target per comprendere in che modo gli utenti interagiscono con il contenuto del sito e come questo influisce sulle sue metriche.

* Il contenuto si trova in AEM e gli editor di contenuti creano e gestiscono le pagine del sito
* Target utilizza le pagine del sito ospitate da AEM per eseguire test e personalizzazione
* Target fornisce contenuti personalizzati
* Qui non viene creato nessun contenuto del tutto nuovo
* Applicabile sia ai siti AEM che non AEM

  ![diagramma](assets/personalization-use-case-2/use-case-2-diagram.png)

**Per implementare questo scenario, è necessario:**

* [Integrare AEM e Adobe Target utilizzando tag e Adobe I/O](./implementation.md#integrating-aem-target-options)

***Dopo aver implementato l’integrazione precedente, esplora lo [scenario nei dettagli.](./personalization-use-case-2.md)***
