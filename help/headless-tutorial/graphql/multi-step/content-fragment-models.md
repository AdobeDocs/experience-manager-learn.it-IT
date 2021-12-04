---
title: Definizione dei modelli di frammenti di contenuto - Guida introduttiva a AEM Headless - GraphQL
description: Guida introduttiva ad Adobe Experience Manager (AEM) e GraphQL. Scopri come modellare il contenuto e creare uno schema con Modelli per frammenti di contenuto in AEM. Rivedi i modelli esistenti e crea un nuovo modello. Scopri i diversi tipi di dati che possono essere utilizzati per definire uno schema.
version: Cloud Service
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 1%

---

# Definizione dei modelli di frammenti di contenuto {#content-fragment-models}

In questo capitolo scopri come modellare il contenuto e creare uno schema con **Modelli per frammenti di contenuto**. Esaminerete i modelli esistenti e creerete un nuovo modello. Inoltre, verranno illustrati i diversi tipi di dati che possono essere utilizzati per definire uno schema come parte del modello.

In questo capitolo verrà creato un nuovo modello per un **Collaboratore**, che è il modello dati per gli utenti che creano contenuti per riviste e avventure come parte del marchio WKND.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti e si presume che i passaggi descritti in [Configurazione rapida](../quick-setup/local-sdk.md) sono state completate.

## Obiettivi {#objectives}

* Crea un nuovo modello di frammento di contenuto.
* Identifica i tipi di dati disponibili e le opzioni di convalida per la creazione di modelli.
* Comprendere come il modello per frammento di contenuto definisce **entrambi** lo schema dati e il modello di creazione per un frammento di contenuto.

## Panoramica modello frammento di contenuto {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

Il video precedente offre una panoramica di alto livello sull’utilizzo dei modelli di frammenti di contenuto.

>[!CAUTION]
>
> Il video precedente mostra la creazione del **Collaboratore** modello con il nome `Contributors`. Quando esegui i passaggi nel tuo ambiente, accertati che il titolo utilizzi il singolo modulo: `Contributor` senza **s**. La denominazione del modello per frammenti di contenuto determina le chiamate API GraphQL che verranno eseguite in seguito nell’esercitazione.

## Inspect, il modello di frammento di contenuto avventuroso

Nel capitolo precedente sono stati modificati e visualizzati su un’applicazione esterna diversi frammenti di contenuto Avventures. Esaminiamo il modello per frammenti di contenuto di avventura per comprendere lo schema dei dati sottostanti di questi frammenti.

1. Da **Inizio AEM** menu vai a **Strumenti** > **Risorse** > **Modelli per frammenti di contenuto**.

   ![Passa a Modelli di frammenti di contenuto](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Passa a **Sito WKND** e passa il puntatore del mouse sulla cartella **Avventura** Modello per frammento di contenuto e fai clic sul pulsante **Modifica** icona (matita) per aprire il modello.

   ![Apri il modello di frammento di contenuto di avventura](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Viene aperta la **Editor modello frammento di contenuto**. Osserva che i campi definiscono il modello Avventura includi diversi **Tipi di dati** like **Testo a riga singola**, **Testo a più righe**, **Enumerazione** e **Riferimento contenuto**.

1. La colonna di destra dell’editor elenca i **Tipi di dati** che definiscono i campi modulo utilizzati per la creazione di frammenti di contenuto.

1. Seleziona la **Titolo** nel pannello principale. Nella colonna di destra fai clic sul pulsante **Proprietà** scheda:

   ![Proprietà titolo avventura](assets/content-fragment-models/adventure-title-properties-tab.png)

   Osserva la **Nome proprietà** campo impostato su `adventureTitle`. Definisce il nome della proprietà persistente da AEM. La **Nome proprietà** definisce anche le **key** nome di questa proprietà come parte dello schema dati. Questo **key** viene utilizzato quando i dati dei frammenti di contenuto sono esposti tramite API GraphQL.

   >[!CAUTION]
   >
   > Modifica della **Nome proprietà** di un campo **dopo** I frammenti di contenuto sono derivati dal modello e hanno effetti a valle. I valori dei campi nei frammenti esistenti non saranno più referenziati e lo schema dei dati esposto da GraphQL cambierà, influendo sulle applicazioni esistenti.

1. Scorri verso il basso in **Proprietà** e visualizza la scheda **Tipo di convalida** a discesa.

   ![Opzioni di convalida disponibili](assets/content-fragment-models/validation-options-available.png)

   Le convalide predefinite del modulo sono disponibili per **Posta elettronica** e **URL**. È inoltre possibile definire un **Personalizzato** convalida mediante espressione regolare.

1. Fai clic su **Annulla** per chiudere l’Editor modello frammento di contenuto.

## Creare un modello per collaboratori

Quindi, crea un nuovo modello per un **Collaboratore**, che è il modello dati per gli utenti che creano contenuti per riviste e avventure come parte del marchio WKND.

1. Fai clic su **Crea** nell&#39;angolo in alto a destra per visualizzare **Crea modello** procedura guidata.
1. Per **Titolo modello** immetti: **Collaboratore** e fai clic su **Crea**

   ![Procedura guidata del modello a frammento di contenuto](assets/content-fragment-models/content-fragment-model-wizard.png)

   Fai clic su **Apri** per aprire il modello appena creato.

1. Trascina e rilascia una **Testo a riga singola** sul pannello principale. Immetti le seguenti proprietà nel **Proprietà** scheda:

   * **Etichetta campo**: **Nome completo**
   * **Nome proprietà**: `fullName`
   * Controlla **Obbligatorio**

   ![Campo proprietà Nome completo](assets/content-fragment-models/full-name-property-field.png)

1. Fai clic sul pulsante **Tipi di dati** trascina e rilascia una **Testo a più righe** campo sotto **Nome completo** campo . Immetti le seguenti proprietà:

   * **Etichetta campo**: **Biografia**
   * **Nome proprietà**: `biographyText`
   * **Tipo predefinito**: **Rich Text**

1. Fai clic sul pulsante **Tipi di dati** trascina e rilascia una **Riferimento contenuto** campo . Immetti le seguenti proprietà:

   * **Etichetta campo**: **Riferimento immagine**
   * **Nome proprietà**: `pictureReference`
   * **Percorso directory principale**: `/content/dam/wknd`

   Durante la configurazione della **Percorso radice** puoi fare clic sul pulsante **cartella** per visualizzare un modale per selezionare il percorso. Questo limiterà le cartelle che gli autori possono utilizzare per compilare il percorso.

   ![Percorso radice configurato](assets/content-fragment-models/root-path-configure.png)

1. Aggiungi una convalida al **Riferimento immagine** in modo che solo i tipi di contenuto **Immagini** può essere utilizzato per compilare il campo.

   ![Limita alle immagini](assets/content-fragment-models/picture-reference-content-types.png)

1. Fai clic sul pulsante **Tipi di dati** trascina e rilascia una **Enumerazione**  tipo di dati sotto la **Riferimento immagine** campo . Immetti le seguenti proprietà:

   * **Etichetta campo**: **Occupazione**
   * **Nome proprietà**: `occupation`

1. Aggiungi diversi **Opzioni** utilizzando **Aggiungi un’opzione** pulsante . Utilizza lo stesso valore per **Etichetta opzione** e **Valore opzione**:

   **Artista**, **Influencer**, **Fotografo**, **Viaggiatore**, **Scrittore**, **YouTuber**

   ![Valori delle opzioni di occupazione](assets/content-fragment-models/occupation-options-values.png)

1. Il finale **Collaboratore** Il modello deve essere simile al seguente:

   ![Modello collaboratore finale](assets/content-fragment-models/final-contributor-model.png)

1. Fai clic su **Salva** per salvare le modifiche.

## Attiva il modello per collaboratori

I modelli di frammento di contenuto devono essere **Abilitato** prima che gli autori di contenuti possano usarlo. È possibile **Disattiva** un modello per frammenti di contenuto, che pertanto vieta agli autori di utilizzarlo. Ricorda che la modifica della **Nome proprietà** di un campo nel modello modifica lo schema dei dati sottostanti e può avere effetti significativi a valle sui frammenti esistenti e sulle applicazioni esterne. Si consiglia di pianificare attentamente la convenzione di denominazione utilizzata per **Nome proprietà** dei campi prima di abilitare il modello di frammento di contenuto per gli utenti.

1. Assicurati che **Collaboratore** modello attualmente in un **Abilitato** stato.

   ![Modello collaboratore abilitato](assets/content-fragment-models/enable-contributor-model.png)

   Per attivare o disattivare lo stato di un modello di frammento di contenuto, passa il mouse sulla scheda e fai clic sul pulsante **Disattiva** / **Abilita** icona.

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato il tuo primo modello di frammento di contenuto!

## Passaggi successivi {#next-steps}

Nel capitolo successivo, [Authoring di modelli di frammenti di contenuto](author-content-fragments.md), puoi creare e modificare un nuovo frammento di contenuto basato su un modello di frammento di contenuto. Inoltre, verrà illustrato come creare varianti di frammenti di contenuto.
