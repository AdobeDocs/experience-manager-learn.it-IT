---
title: Creare modelli per frammenti di contenuto | OpenAPI AEM Headless - Parte 1
description: Introduzione a Adobe Experience Manager (AEM) e OpenAPI. Scopri come modellare il contenuto e creare uno schema con i modelli per frammenti di contenuto in AEM. Rivedi i modelli esistenti e crea un modello. Scopri i diversi tipi di dati utilizzabili per definire uno schema.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 430
source-git-commit: a3d2a88232cae941647464be8e215a47c85bc0ab
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 3%

---

# Creare modelli di frammenti di contenuto

In questo capitolo, scopri come modellare il contenuto e creare uno schema con **Modelli per frammenti di contenuto** e i diversi tipi di dati che definiscono un modello per frammenti di contenuto.

In questa esercitazione verranno creati due modelli semplici: **Team** e **Persona**. Il modello dati **Team** contiene nome, nome breve e descrizione e fa riferimento al modello dati **Person**, che contiene nome completo, dettagli bio, immagine profilo e elenco occupazioni.

## Obiettivi

* Creare un modello per frammenti di contenuto.
* Esplora i tipi di dati disponibili e le opzioni di convalida per la creazione di modelli.
* Scopri come i modelli per frammenti di contenuto definiscono **sia** lo schema di dati che il modello di authoring per un frammento di contenuto.

## Creare una configurazione di progetto

Una configurazione di progetto contiene tutti i modelli per frammenti di contenuto associati a un particolare progetto e fornisce un mezzo per organizzare i modelli. Crea almeno un progetto **prima** della creazione del modello per frammenti di contenuto.

1. Accedi all&#39;ambiente AEM **Author** (es. `https://author-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com/`)
1. Dalla schermata iniziale di AEM, passa a **Strumenti** > **Generale** > **Browser configurazioni**.
1. Fai clic su **Crea** nella barra delle azioni superiore e immetti i seguenti dettagli di configurazione:
   * Titolo: **Progetto**
   * Nome: **progetto personale**
   * Modelli per frammenti di contenuto: **Selezionati**

   ![Configurazione progetto personale](assets/1/create-configuration.png)

1. Seleziona **Crea** per creare la configurazione del progetto.

## Creare modelli di frammenti di contenuto

Quindi, crea modelli per frammenti di contenuto per un **Team** e una **Persona**. Questi fungono da modelli di dati, o schemi, che rappresentano un team e una persona che fa parte di un team e definiscono l’interfaccia che consente agli autori di creare e modificare frammenti di contenuto in base a questi modelli.

### Creare il modello per frammenti di contenuto della persona

Crea un modello per frammenti di contenuto per una **persona**, che è il modello di dati o lo schema, che rappresenta una persona che fa parte di un team.

1. Dalla schermata iniziale di AEM, passa a **Strumenti** > **Generale** > **Modelli per frammenti di contenuto**.
1. Passare alla cartella **Progetto personale**.
1. Seleziona **Crea** nell&#39;angolo superiore destro per visualizzare la procedura guidata **Crea modello**.
1. Crea un modello per frammenti di contenuto con le seguenti proprietà:

   * Titolo modello: **Persona**
   * Abilita modello: **Selezionato**

   Seleziona **Crea**. Nella finestra di dialogo risultante, seleziona **Apri**, per generare il modello.

1. Trascina e rilascia un elemento **Testo su riga singola** nel pannello principale. Immetti le seguenti proprietà nella scheda **Proprietà**:

   * Etichetta Campo: **Nome Completo**
   * Nome proprietà: `fullName`
   * Seleziona **Obbligatorio**

   **Nome proprietà** definisce il nome della proprietà in cui è memorizzato il valore creato in AEM. Il nome **proprietà** definisce anche il nome **chiave** per questa proprietà come parte dello schema dei dati e viene utilizzato come chiave nella risposta JSON quando il frammento di contenuto viene distribuito tramite le OpenAPI di AEM.

1. Seleziona la scheda **Tipi di dati** e trascina un campo **Testo su più righe** sotto il campo **Nome completo**. Immetti le seguenti proprietà:

   * Etichetta Campo: **Biografia**
   * Nome proprietà: `biographyText`
   * Tipo predefinito: **Rich Text**

1. Fai clic sulla scheda **Tipi di dati** e trascina un campo **Riferimento contenuto**. Immetti le seguenti proprietà:

   * Etichetta Campo: **Immagine Profilo**
   * Nome proprietà: `profilePicture`
   * Percorso directory principale: `/content/dam`

     Durante la configurazione del **percorso principale**, puoi fare clic sull&#39;icona **cartella** per visualizzare un modale per selezionare il percorso. Questo limita le cartelle che gli autori possono utilizzare per compilare il percorso. `/content/dam` è la directory principale in cui sono archiviati tutti i file AEM Assets (immagini, video e altri frammenti di contenuto).

   * Accetta solo tipi di contenuto specifici: **Immagine**

     Aggiungi una convalida al **Riferimento immagine** in modo che solo i tipi di contenuto di **Immagini** possano essere utilizzati per compilare il campo.

   * Mostra miniatura: **Selezionata**

1. Fai clic sulla scheda **Tipi di dati** e trascina un tipo di dati **Enumerazione** sotto il campo **Riferimento immagine**. Immetti le seguenti proprietà:

   * Rendering come: **Caselle di controllo**
   * Etichetta Campo: **Occupazione**
   * Nome proprietà: `occupation`
   * Opzioni:
      * **Artista**
      * **Influencer**
      * **Fotografo**
      * **Viaggiatore**
      * **Autore**
      * **Tuber**

   Impostare sia l&#39;etichetta di opzione che il valore sullo stesso valore.

1. Il modello finale di **Persona** deve essere simile al seguente:

   ![Modello per frammenti di contenuto persona](assets/1/person-content-fragment-model.png)

1. Fai clic su **Salva** per salvare le modifiche.

### Creare il modello per frammenti di contenuto team

Crea un modello per frammenti di contenuto per un **team**, che è il modello dati per un team di persone. Il modello Team fa riferimento ai frammenti di contenuto della persona, che rappresentano i membri del team.

1. Nella cartella **Progetto**, seleziona **Crea** nell&#39;angolo superiore destro per visualizzare la procedura guidata **Crea modello**.
1. Nel campo **Titolo modello** immettere **Team** e selezionare **Crea**.

   Seleziona **Apri** nella finestra di dialogo risultante per aprire il modello appena creato.

1. Trascina e rilascia un elemento **Testo su riga singola** nel pannello principale. Immetti le seguenti proprietà nella scheda **Proprietà**:

   * Etichetta Campo: **Titolo**
   * Nome proprietà: `title`
   * Seleziona **Obbligatorio**

1. Seleziona la scheda **Tipi di dati** e trascina un campo **Testo su più righe** sotto il campo **Nome breve**. Immetti le seguenti proprietà:

   * Etichetta Campo: **Descrizione**
   * Nome proprietà: `description`
   * Tipo predefinito: **Rich Text**

1. Fai clic sulla scheda **Tipi di dati** e trascina un campo **Riferimento frammento**. Immetti le seguenti proprietà:

   * Rendering come: **Campo multiplo**
   * Numero minimo di elementi: **2**
   * Etichetta Campo: **Membri Team**
   * Nome proprietà: `teamMembers`
   * Modelli per frammenti di contenuto consentiti: utilizza l&#39;icona della cartella per selezionare il modello **Persona**.

1. Il modello **Team** finale deve essere simile al seguente:

   ![Modello finale frammento di contenuto team](assets/1/team-content-fragment-model.png)

1. Fai clic su **Salva** per salvare le modifiche.

1. Ora è necessario disporre di due modelli da utilizzare:

   ![Due modelli per frammenti di contenuto](assets/1/two-content-fragment-models.png)

## Congratulazioni.

Congratulazioni, hai appena creato i tuoi primi modelli per frammenti di contenuto.

## Passaggi successivi

Nel prossimo capitolo, [Creazione di modelli per frammenti di contenuto](2-author-content-fragments.md), puoi creare e modificare un nuovo frammento di contenuto basato su un modello per frammenti di contenuto. Scopri anche come creare varianti di Frammenti di contenuto.

## Documentazione correlata

* [Modelli per frammenti di contenuto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=it)

