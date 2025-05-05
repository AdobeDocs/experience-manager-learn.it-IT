---
title: Definizione dei modelli per frammenti di contenuto - Guida introduttiva ad AEM Headless - GraphQL
description: Introduzione a Adobe Experience Manager (AEM) e GraphQL. Scopri come modellare il contenuto e creare uno schema con i modelli per frammenti di contenuto in AEM. Rivedi i modelli esistenti e crea un modello. Scopri i diversi tipi di dati utilizzabili per definire uno schema.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
duration: 228
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1110'
ht-degree: 2%

---

# Definizione dei modelli per frammenti di contenuto {#content-fragment-models}

In questo capitolo, scopri come modellare il contenuto e creare uno schema con **Modelli per frammenti di contenuto**. Scopri i diversi tipi di dati che possono essere utilizzati per definire uno schema come parte del modello.

Vengono creati due modelli semplici: **Team** e **Persona**. Il modello dati **Team** contiene nome, nome breve e descrizione e fa riferimento al modello dati **Person**, che contiene nome completo, dettagli bio, immagine profilo e elenco occupazioni.

Puoi anche creare un modello personalizzato seguendo i passaggi di base e regolare i rispettivi passaggi, come le query GraphQL e il codice dell’app React, oppure semplicemente seguire i passaggi descritti in questi capitoli.

## Prerequisiti {#prerequisites}

Questo è un tutorial in più parti e si presume che sia disponibile un ambiente di authoring [AEM](./overview.md#prerequisites).

## Obiettivi {#objectives}

* Creare un modello per frammenti di contenuto.
* Identifica i tipi di dati disponibili e le opzioni di convalida per la creazione di modelli.
* Scopri in che modo il modello per frammenti di contenuto definisce **sia** lo schema di dati che il modello di authoring per un frammento di contenuto.

## Creare una configurazione di progetto

Una configurazione di progetto contiene tutti i modelli di Frammento di contenuto associati a un particolare progetto e fornisce un mezzo per organizzare i modelli. È necessario creare almeno un progetto **prima** di creare il modello per frammenti di contenuto.

1. Accedi all&#39;ambiente AEM **Author** (es. `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. Dalla schermata iniziale di AEM, passa a **Strumenti** > **Generale** > **Browser configurazioni**.

   ![Passa a Browser configurazioni](assets/content-fragment-models/navigate-config-browser.png)
1. Fai clic su **Crea**, nell&#39;angolo in alto a destra
1. Nella finestra di dialogo risultante, immetti:

   * Titolo*: **Progetto personale**
   * Nome*: **progetto personale** (preferisci utilizzare lettere minuscole utilizzando i trattini per separare le parole. Questa stringa influenza l&#39;endpoint GraphQL univoco per il quale le applicazioni client eseguono le richieste.)
   * Controlla **Modelli per frammenti di contenuto**
   * Controlla **Query persistenti GraphQL**

   ![Configurazione progetto personale](assets/content-fragment-models/my-project-configuration.png)

## Creare modelli di frammenti di contenuto

Quindi, crea due modelli per un **Team** e una **Persona**.

### Creare il modello della persona

Crea un modello per una **Persona**, che è il modello dati che rappresenta una persona che fa parte di un team.

1. Dalla schermata iniziale di AEM, passa a **Strumenti** > **Generale** > **Modelli per frammenti di contenuto**.

   ![Passa a modelli per frammenti di contenuto](assets/content-fragment-models/navigate-cf-models.png)

1. Passare alla cartella **Progetto personale**.
1. Tocca **Crea** nell&#39;angolo superiore destro per visualizzare la procedura guidata **Crea modello**.
1. Nel campo **Titolo modello**, immetti **Persona** e tocca **Crea**. Nella finestra di dialogo risultante, tocca **Apri** per generare il modello.

1. Trascina e rilascia un elemento **Testo su riga singola** nel pannello principale. Immetti le seguenti proprietà nella scheda **Proprietà**:

   * **Etichetta Campo**: **Nome Completo**
   * **Nome proprietà**: `fullName`
   * Seleziona **Obbligatorio**

   ![Campo proprietà Nome completo](assets/content-fragment-models/full-name-property-field.png)

   Il **Nome proprietà** definisce il nome della proprietà che viene mantenuto in AEM. Il nome **proprietà** definisce anche il nome **chiave** per questa proprietà come parte dello schema dei dati. Questa **chiave** viene utilizzata quando i dati dei frammenti di contenuto sono esposti tramite API GraphQL.

1. Tocca la scheda **Tipi di dati** e trascina un campo **Testo su più righe** sotto il campo **Nome completo**. Immetti le seguenti proprietà:

   * **Etichetta Campo**: **Biografia**
   * **Nome proprietà**: `biographyText`
   * **Tipo predefinito**: **Testo formattato**

1. Fai clic sulla scheda **Tipi di dati** e trascina un campo **Riferimento contenuto**. Immetti le seguenti proprietà:

   * **Etichetta Campo**: **Immagine Profilo**
   * **Nome proprietà**: `profilePicture`
   * **Percorso principale**: `/content/dam`

   Durante la configurazione del **percorso principale**, puoi fare clic sull&#39;icona **cartella** per visualizzare un modale per selezionare il percorso. Questo limita le cartelle che gli autori possono utilizzare per compilare il percorso. `/content/dam` è la directory principale in cui sono archiviati tutti i file AEM Assets (immagini, video e altri frammenti di contenuto).

1. Aggiungi una convalida al **Riferimento immagine** in modo che solo i tipi di contenuto di **Immagini** possano essere utilizzati per compilare il campo.

   ![Limita alle immagini](assets/content-fragment-models/picture-reference-content-types.png)

1. Fai clic sulla scheda **Tipi di dati** e trascina un tipo di dati **Enumerazione** sotto il campo **Riferimento immagine**. Immetti le seguenti proprietà:

   * **Rendering come**: **Caselle di controllo**
   * **Etichetta Campo**: **Occupazione**
   * **Nome proprietà**: `occupation`

1. Aggiungi diverse **opzioni** utilizzando il pulsante **Aggiungi un&#39;opzione**. Usa lo stesso valore per **Etichetta opzione** e **Valore opzione**:

   **Artista**, **Influencer**, **Fotografo**, **Viaggiatore**, **Autore**, **YouTuber**

1. Il modello finale di **Persona** deve essere simile al seguente:

   ![Modello persona finale](assets/content-fragment-models/final-author-model.png)

1. Fai clic su **Salva** per salvare le modifiche.

### Creare il modello di team

Crea un modello per un **Team**, che è il modello dati per un team di persone. Il modello Team fa riferimento al modello Persona per rappresentare i membri del team.

1. Nella cartella **Progetto**, tocca **Crea** nell&#39;angolo superiore destro per visualizzare la procedura guidata **Crea modello**.
1. Nel campo **Titolo modello**, immetti **Team** e tocca **Crea**.

   Tocca **Apri** nella finestra di dialogo risultante per aprire il modello appena creato.

1. Trascina e rilascia un elemento **Testo su riga singola** nel pannello principale. Immetti le seguenti proprietà nella scheda **Proprietà**:

   * **Etichetta Campo**: **Titolo**
   * **Nome proprietà**: `title`
   * Seleziona **Obbligatorio**

1. Tocca la scheda **Tipi di dati** e trascina un elemento **Testo su riga singola** nel pannello principale. Immetti le seguenti proprietà nella scheda **Proprietà**:

   * **Etichetta Campo**: **Nome Breve**
   * **Nome proprietà**: `shortName`
   * Seleziona **Obbligatorio**
   * Seleziona **Univoco**
   * In **Tipo di convalida** > scegli **Personalizzato**
   * In **Regex convalida personalizzata** > immetti `^[a-z0-9\-_]{5,40}$`. In questo modo è possibile immettere solo valori alfanumerici minuscoli e trattini da 5 a 40 caratteri.

   La proprietà `shortName` consente di eseguire query su un singolo team in base a un percorso abbreviato. L&#39;impostazione **Unique** assicura che il valore sia sempre univoco per ogni frammento di contenuto di questo modello.

1. Tocca la scheda **Tipi di dati** e trascina un campo **Testo su più righe** sotto il campo **Nome breve**. Immetti le seguenti proprietà:

   * **Etichetta Campo**: **Descrizione**
   * **Nome proprietà**: `description`
   * **Tipo predefinito**: **Testo formattato**

1. Fai clic sulla scheda **Tipi di dati** e trascina un campo **Riferimento frammento**. Immetti le seguenti proprietà:

   * **Rendering come**: **Campo multiplo**
   * **Etichetta Campo**: **Membri Team**
   * **Nome proprietà**: `teamMembers`
   * **Modelli per frammenti di contenuto consentiti**: utilizza l&#39;icona della cartella per selezionare il modello **Person**.

1. Il modello **Team** finale deve essere simile al seguente:

   ![Modello team finale](assets/content-fragment-models/final-team-model.png)

1. Fai clic su **Salva** per salvare le modifiche.

1. Ora è necessario disporre di due modelli da utilizzare:

   ![Due modelli](assets/content-fragment-models/two-new-models.png)

## Pubblica configurazione progetto e modelli per frammenti di contenuto

Al momento della revisione e della verifica, pubblicare `Project Configuration` e `Content Fragment Model`

1. Dalla schermata iniziale di AEM, passa a **Strumenti** > **Generale** > **Browser configurazioni**.

1. Tocca la casella di controllo accanto a **Il mio progetto** e tocca **Pubblica**

   ![Pubblica configurazione progetto](assets/content-fragment-models/publish-project-config.png)

1. Dalla schermata iniziale di AEM, passa a **Strumenti** > **Generale** > **Modelli per frammenti di contenuto**.

1. Passare alla cartella **Progetto personale**.

1. Tocca i modelli **Persona** e **Team** e tocca **Pubblica**

   ![Pubblica modelli per frammenti di contenuto](assets/content-fragment-models/publish-content-fragment-model.png)

## Congratulazioni. {#congratulations}

Congratulazioni, hai appena creato i tuoi primi modelli per frammenti di contenuto.

## Passaggi successivi {#next-steps}

Nel prossimo capitolo, [Creazione di modelli per frammenti di contenuto](author-content-fragments.md), puoi creare e modificare un nuovo frammento di contenuto basato su un modello per frammenti di contenuto. Scopri anche come creare varianti di Frammenti di contenuto.

## Documentazione correlata

* [Modelli per frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=it)

