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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1132'
ht-degree: 2%

---

# Definizione dei modelli di frammenti di contenuto {#content-fragment-models}

In questo capitolo scopri come modellare il contenuto e creare uno schema con **Modelli per frammenti di contenuto**. Scoprirai i diversi tipi di dati che possono essere utilizzati per definire uno schema come parte del modello.

In questo capitolo vengono creati due modelli semplici, **Team** e **Persona**. La **Team** il modello dati ha nome, nome breve e descrizione e fa riferimento al **Persona** modello dati, che ha nome completo, dettagli bio, immagine profilo e elenco occupazioni.

Puoi anche creare un tuo modello seguendo i passaggi di base e modificando i rispettivi passaggi come le query GraphQL e React App code o semplicemente seguendo i passaggi descritti in questi capitoli.

## Prerequisiti {#prerequisites}

Si tratta di un tutorial in più parti e si presume che un [AEM’ambiente di authoring è disponibile](./overview.md#prerequisites)

## Obiettivi {#objectives}

* Crea un nuovo modello di frammento di contenuto.
* Identifica i tipi di dati disponibili e le opzioni di convalida per la creazione di modelli.
* Comprendere come il modello per frammento di contenuto definisce **entrambi** lo schema dati e il modello di creazione per un frammento di contenuto.

## Creare una nuova configurazione di progetto

Una configurazione di progetto contiene tutti i modelli di frammento di contenuto associati a un particolare progetto e fornisce un mezzo per organizzare i modelli. È necessario creare almeno un progetto **prima** creazione di un nuovo modello per frammenti di contenuto.

1. Accedi al AEM **Autore** ambiente (es. `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. Dalla schermata iniziale AEM, passa a **Strumenti** > **Generale** > **Browser di configurazione**.

   ![Passa al browser di configurazione](assets/content-fragment-models/navigate-config-browser.png)
1. Fai clic su **Crea**.
1. Nella finestra di dialogo risultante inserisci:

   * Titolo*: **Progetto personale**
   * Nome*: **progetto personale** (preferisci utilizzare tutte le lettere minuscole utilizzando i trattini per separare le parole. Questa stringa influenzerà l&#39;endpoint GraphQL univoco su cui le applicazioni client eseguiranno le richieste.)
   * Controlla **Modelli per frammenti di contenuto**
   * Controlla **Query persistenti GraphQL**

   ![Configurazione del progetto personale](assets/content-fragment-models/my-project-configuration.png)

## Creare modelli di frammenti di contenuto

Quindi, crea due modelli per un **Team** e **Persona**.

### Creare il modello persona

Crea un nuovo modello per un **Persona**, che è il modello dati che rappresenta una persona che fa parte di un team.

1. Dalla schermata iniziale AEM, passa a **Strumenti** > **Generale** > **Modelli per frammenti di contenuto**.

   ![Passa a Modelli di frammenti di contenuto](assets/content-fragment-models/navigate-cf-models.png)

1. Passa a **Progetto personale** cartella.
1. Tocca **Crea** nell&#39;angolo in alto a destra per visualizzare **Crea modello** procedura guidata.
1. Per **Titolo modello** immetti: **Persona** e toccare **Crea**.

   Tocca **Apri** nella finestra di dialogo risultante, per aprire il modello appena creato.

1. Trascina e rilascia una **Testo a riga singola** sul pannello principale. Immetti le seguenti proprietà nel **Proprietà** scheda:

   * **Etichetta campo**: **Nome completo**
   * **Nome proprietà**: `fullName`
   * Controlla **Obbligatorio**

   ![Campo proprietà Nome completo](assets/content-fragment-models/full-name-property-field.png)

   La **Nome proprietà** definisce il nome della proprietà persistente da AEM. La **Nome proprietà** definisce anche le **key** nome di questa proprietà come parte dello schema dati. Questo **key** viene utilizzato quando i dati dei frammenti di contenuto sono esposti tramite API GraphQL.

1. Tocca **Tipi di dati** trascina e rilascia una **Testo a più righe** campo sotto **Nome completo** campo . Immetti le seguenti proprietà:

   * **Etichetta campo**: **Biografia**
   * **Nome proprietà**: `biographyText`
   * **Tipo predefinito**: **Rich Text**

1. Fai clic sul pulsante **Tipi di dati** trascina e rilascia una **Riferimento contenuto** campo . Immetti le seguenti proprietà:

   * **Etichetta campo**: **Immagine profilo**
   * **Nome proprietà**: `profilePicture`
   * **Percorso directory principale**: `/content/dam`

   Durante la configurazione della **Percorso radice** puoi fare clic sul pulsante **cartella** per visualizzare un modale per selezionare il percorso. Questo limiterà le cartelle che gli autori possono utilizzare per compilare il percorso. `/content/dam` è la directory principale in cui vengono memorizzate tutte le risorse AEM (immagini, video e altri frammenti di contenuto).

1. Aggiungi una convalida al **Riferimento immagine** in modo che solo i tipi di contenuto **Immagini** può essere utilizzato per compilare il campo.

   ![Limita alle immagini](assets/content-fragment-models/picture-reference-content-types.png)

1. Fai clic sul pulsante **Tipi di dati** trascina e rilascia una **Enumerazione**  tipo di dati sotto la **Riferimento immagine** campo . Immetti le seguenti proprietà:

   * **Rendering come**: **Caselle di controllo**
   * **Etichetta campo**: **Occupazione**
   * **Nome proprietà**: `occupation`

1. Aggiungi diversi **Opzioni** utilizzando **Aggiungi un’opzione** pulsante . Utilizza lo stesso valore per **Etichetta opzione** e **Valore opzione**:

   **Artista**, **Influencer**, **Fotografo**, **Viaggiatore**, **Scrittore**, **YouTuber**

1. Il finale **Persona** Il modello deve essere simile al seguente:

   ![Modello a persona finale](assets/content-fragment-models/final-author-model.png)

1. Fai clic su **Salva** per salvare le modifiche.

### Crea modello team

Crea un nuovo modello per un **Team**, che è il modello dati per un team di persone. Il modello Team farà riferimento al modello Persona per rappresentare i membri del team.

1. In **Progetto personale** cartella, tocca **Crea** nell&#39;angolo in alto a destra per visualizzare **Crea modello** procedura guidata.
1. Per **Titolo modello** immetti: **Team** e toccare **Crea**.

   Tocca **Apri** nella finestra di dialogo risultante, per aprire il modello appena creato.

1. Trascina e rilascia una **Testo a riga singola** sul pannello principale. Immetti le seguenti proprietà nel **Proprietà** scheda:

   * **Etichetta campo**: **Titolo**
   * **Nome proprietà**: `title`
   * Controlla **Obbligatorio**

1. Tocca **Tipi di dati** trascina e rilascia una **Testo a riga singola** sul pannello principale. Immetti le seguenti proprietà nel **Proprietà** scheda:

   * **Etichetta campo**: **Nome breve**
   * **Nome proprietà**: `shortName`
   * Controlla **Obbligatorio**
   * Controlla **Univoco**
   * Sotto **Tipo di convalida** > scegli **Personalizzato**
   * Sotto **Regex di convalida personalizzato** > enter `^[a-z0-9\-_]{5,40}$` - in questo modo è possibile immettere solo valori alfanumerici minuscoli e trattini compresi tra 5 e 40 caratteri.

   La `shortName` La proprietà ci fornirà un modo per eseguire query su un singolo team in base a un percorso abbreviato. La **Univoco** l’impostazione assicura che il valore sia sempre univoco per ogni frammento di contenuto di questo modello.

1. Tocca **Tipi di dati** trascina e rilascia una **Testo a più righe** campo sotto **Nome breve** campo . Immetti le seguenti proprietà:

   * **Etichetta campo**: **Descrizione**
   * **Nome proprietà**: `description`
   * **Tipo predefinito**: **Rich Text**

1. Fai clic sul pulsante **Tipi di dati** trascina e rilascia una **Riferimento frammento** campo . Immetti le seguenti proprietà:

   * **Rendering come**: **Campo multiplo**
   * **Etichetta campo**: **Membri del team**
   * **Nome proprietà**: `teamMembers`
   * **Modelli di frammenti di contenuto consentiti**: Utilizza l’icona della cartella per selezionare la **Persona** modello.

1. Il finale **Team** Il modello deve essere simile al seguente:

   ![Modello del team finale](assets/content-fragment-models/final-team-model.png)

1. Fai clic su **Salva** per salvare le modifiche.

1. Ora dovresti disporre di due modelli da cui lavorare:

   ![Due modelli](assets/content-fragment-models/two-new-models.png)

## Pubblica configurazione progetto e modelli di frammenti di contenuto

Al momento della revisione e della verifica, pubblica il `Project Configuration` &amp; `Content Fragment Model`

1. Dalla schermata iniziale AEM, passa a **Strumenti** > **Generale** > **Browser di configurazione**.

1. Tocca la casella di controllo accanto a **Progetto personale** e toccare **Pubblica**

   ![Pubblica configurazione progetto](assets/content-fragment-models/publish-project-config.png)

1. Dalla schermata iniziale AEM, passa a **Strumenti** > **Generale** > **Modelli per frammenti di contenuto**.

1. Passa a **Progetto personale** cartella.

1. Tocca **Persona** e **Team** modelli e tocca **Pubblica**

   ![Pubblicare modelli di frammenti di contenuto](assets/content-fragment-models/publish-content-fragment-model.png)

## Congratulazioni! {#congratulations}

Congratulazioni, hai appena creato i tuoi primi modelli di frammenti di contenuto!

## Passaggi successivi {#next-steps}

Nel capitolo successivo, [Authoring di modelli di frammenti di contenuto](author-content-fragments.md), puoi creare e modificare un nuovo frammento di contenuto basato su un modello di frammento di contenuto. Inoltre, verrà illustrato come creare varianti di frammenti di contenuto.

## Documentazione correlata

* [Modelli per frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

