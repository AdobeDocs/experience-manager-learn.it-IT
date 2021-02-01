---
title: Definizione dei modelli di frammento di contenuto - Guida introduttiva AEM senza titolo - GraphQL
description: Guida introduttiva ad Adobe Experience Manager (AEM) e GraphQL. Scopri come modellare il contenuto e creare uno schema con i modelli di frammenti di contenuto in AEM. Rivedete i modelli esistenti e create un nuovo modello. Scopri i diversi tipi di dati utilizzabili per definire uno schema.
sub-product: assets
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
translation-type: tm+mt
source-git-commit: 8c5b425e6dcf23cbef042097f17db9e51bdf63c9
workflow-type: tm+mt
source-wordcount: '950'
ht-degree: 1%

---


# Definizione dei modelli di frammento di contenuto {#content-fragment-models}

In questo capitolo viene illustrato come modellare il contenuto e creare uno schema con **Modelli di frammenti di contenuto**. Verranno esaminati i modelli esistenti e verrà creato un nuovo modello. Verranno inoltre illustrati i diversi tipi di dati che è possibile utilizzare per definire uno schema come parte del modello.

In questo capitolo verrà creato un nuovo modello per un **Collaboratore**, che è il modello dati per gli utenti che creano contenuti di rivista e avventura come parte del marchio WKND.

## Prerequisiti {#prerequisites}

Si tratta di un&#39;esercitazione con più parti e si presume che i passaggi descritti in [Quick Setup](./setup.md) siano stati completati.

## Obiettivi {#objectives}

* Creare un nuovo modello di frammento di contenuto.
* Identificare i tipi di dati disponibili e le opzioni di convalida per la creazione di modelli.
* Comprendere in che modo il modello di frammento di contenuto definisce **sia** lo schema di dati che il modello di creazione per un frammento di contenuto.

## Panoramica del modello di frammento di contenuto {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

Questo video offre una panoramica di alto livello sull’utilizzo dei modelli di frammenti di contenuto.

##  Inspect il modello di frammento di contenuto di avventura

Nel capitolo precedente diversi frammenti di contenuto Avventures sono stati modificati e visualizzati su un&#39;applicazione esterna. Esaminare il modello di frammento di contenuto di avventura per comprendere lo schema di dati sottostante di questi frammenti.

1. Dal menu **AEM Start** passare a **Strumenti** > **Risorse** > **Modelli di frammenti di contenuto**.

   ![Passa a Modelli di frammenti di contenuto](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Andate nella cartella **WKND Site** e passate il puntatore del mouse sul modello di frammento di contenuto **Adventure**, quindi fate clic sull&#39;icona **Edit** (matita) per aprire il modello.

   ![Aprire il modello di frammento di contenuto di avventura](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Viene aperto l&#39; **Editor modello frammento di contenuto**. Osservare che i campi che definiscono il modello di avventura includono **Tipi di dati** diversi, come **Testo su riga singola**, **Testo su più righe**, **Enumerazione** e **Riferimento contenuto**.

1. Nella colonna a destra dell&#39;editor sono elencati i **Tipi di dati** disponibili che definiscono i campi del modulo utilizzati per la creazione di frammenti di contenuto.

1. Selezionare il campo **Titolo** nel pannello principale. Nella colonna di destra, fare clic sulla scheda **Proprietà**:

   ![Proprietà titolo avventura](assets/content-fragment-models/adventure-title-properties-tab.png)

   Osservare il campo **Nome proprietà** è impostato su `adventureTitle`. Definisce il nome della proprietà persistente da AEM. Il nome **Property Name** definisce anche il nome **key** di questa proprietà come parte dello schema di dati. Questa **chiave** verrà utilizzata quando i dati del frammento di contenuto saranno esposti tramite le API GraphQL.

   >[!CAUTION]
   >
   > La modifica del **nome proprietà** di un campo **dopo che i frammenti di contenuto sono derivati dal modello ha effetti a valle.** I valori dei campi nei frammenti esistenti non verranno più utilizzati come riferimento e lo schema di dati esposto da GraphQL verrà modificato, interessando le applicazioni esistenti.

1. Scorrete verso il basso nella scheda **Proprietà** e visualizzate il menu a discesa **Tipo di convalida**.

   ![Opzioni di convalida disponibili](assets/content-fragment-models/validation-options-available.png)

   Le convalide dei moduli forniti sono disponibili per **E-mail** e **URL**. È inoltre possibile definire una convalida **Custom** utilizzando un&#39;espressione regolare.

1. Fare clic su **Annulla** per chiudere l&#39;Editor modello frammento di contenuto.

## Creare un modello collaboratore

Quindi, create un nuovo modello per un **Collaboratore**, che è il modello dati per gli utenti che creano contenuti di rivista e avventura come parte del marchio WKND.

1. Fare clic su **Crea** nell&#39;angolo in alto a destra per visualizzare la procedura guidata **Crea modello**.
1. Per **Titolo modello** immettere: **Collaboratore** e fare clic su **Crea**

   ![Creazione guidata modello frammento di contenuto](assets/content-fragment-models/content-fragment-model-wizard.png)

   Fare clic su **Apri** per aprire il modello appena creato.

1. Trascinare un elemento **Testo su riga singola** sul pannello principale. Immettere le seguenti proprietà nella scheda **Proprietà**:

   * **Etichetta** campo:  **Nome completo**
   * **Nome proprietà**: `fullName`
   * Controlla **Obbligatorio**

   ![Campo proprietà Nome completo](assets/content-fragment-models/full-name-property-field.png)

1. Fare clic sulla scheda **Tipi di dati**, quindi trascinare e rilasciare un campo **Testo su più righe** sotto il campo **Nome completo**. Immettere le proprietà seguenti:

   * **Etichetta** campo:  **Biografia**
   * **Nome proprietà**: `biographyText`
   * **Tipo** predefinito:  **Rich Text**

1. Fare clic sulla scheda **Tipi di dati** e trascinare un campo **Riferimento contenuto**. Immettere le proprietà seguenti:

   * **Etichetta** campo:  **Riferimento immagine**
   * **Nome proprietà**: `pictureReference`
   * **Percorso directory principale**: `/content/dam/wknd`

   Durante la configurazione di **Percorso principale** è possibile fare clic sull&#39;icona **cartella** per visualizzare un modale per selezionare il percorso. In questo modo verranno limitate le cartelle che gli autori possono utilizzare per compilare il percorso.

   ![Percorso directory principale configurato](assets/content-fragment-models/root-path-configure.png)

1. Aggiungere una convalida alla **Picture Reference** in modo che sia possibile utilizzare solo i tipi di contenuto di **Images** per compilare il campo.

   ![Limita a immagini](assets/content-fragment-models/picture-reference-content-types.png)

1. Fare clic sulla scheda **Tipi di dati**, quindi trascinare e rilasciare un tipo di dati **Enumeration** sotto il campo **Picture Reference**. Immettere le proprietà seguenti:

   * **Etichetta** campo:  **Occupazione**
   * **Nome proprietà**: `occupation`

1. Aggiungete diverse **opzioni** utilizzando il pulsante **Aggiungi un&#39;opzione**. Utilizzare lo stesso valore per **Etichetta opzione** e **Valore opzione**:

   **Artista**,  **Influenziatore**,  **Fotografo**,  **Viaggiatore**,  **Scrittore**,  **YouTuber**

   ![Valori delle opzioni di occupazione](assets/content-fragment-models/occupation-options-values.png)

1. Il modello finale **Collaboratore** deve essere simile al seguente:

   ![Modello collaboratore finale](assets/content-fragment-models/final-contributor-model.png)

1. Fare clic su **Salva** per salvare le modifiche.

## Abilita modello collaboratore

I modelli di frammento di contenuto vengono impostati per impostazione predefinita allo stato **Bozza** al momento della prima creazione. Questo consente agli utenti di definire meglio il modello di frammento di contenuto **prima**, consentendo agli autori di utilizzarlo. Ricordare che la modifica di **Property Name** di un campo nel modello modifica lo schema di dati sottostante e può avere effetti a valle significativi sui frammenti esistenti e sulle applicazioni esterne. Si consiglia di pianificare con attenzione la convenzione di denominazione utilizzata per i campi **Nome proprietà**.

1. Tenere presente che il modello **Collaboratore** è attualmente in stato **Bozza**.

1. Abilita il **modello collaboratore** posizionando il puntatore del mouse sulla scheda e facendo clic sull&#39;icona **Abilita**:

   ![Abilita modello collaboratore](assets/content-fragment-models/enable-contributor-model.png)

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato il tuo primo modello di frammento di contenuto!

## Passaggi successivi {#next-steps}

Nel capitolo successivo, [Creazione di modelli di frammenti di contenuto](author-content-fragments.md), verrà creato e modificato un nuovo frammento di contenuto basato su un modello di frammento di contenuto. Verrà inoltre illustrato come creare varianti di frammenti di contenuto.