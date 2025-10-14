---
title: Integrazione di Adobe Experience Manager con Adobe Target tramite tag e Adobe Developer
description: Procedura dettagliata su come integrare Adobe Experience Manager con Adobe Target utilizzando tag e Adobe Developer
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 663
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 2%

---

# Utilizzo dei tag tramite Adobe Developer Console

## Prerequisiti

* [Istanza di creazione e pubblicazione AEM](./implementation.md#set-up-aem) in esecuzione rispettivamente sulle porte localhost 4502 e 4503
* **Experience Cloud**
   * Accesso al Adobe Experience Cloud delle organizzazioni - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud con le seguenti soluzioni
      * [Raccolta dati](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe Developer Console](https://developer.adobe.com/console/)

     >[!NOTE]
     >È necessario disporre delle autorizzazioni per sviluppare, approvare, Publish, gestire le estensioni e gestire gli ambienti nella raccolta dati. Se non riesci a completare nessuna di queste operazioni perché le opzioni dell’interfaccia utente non sono disponibili, rivolgiti al tuo amministratore Experience Cloud per richiedere l’accesso. Per ulteriori informazioni sulle autorizzazioni per i tag, [consulta la documentazione](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html?lang=it).

* **Estensioni browser Chrome**
   * Adobe Experience Cloud Debugger(https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

## Utenti interessati

Per questa integrazione, devono essere coinvolti i seguenti tipi di pubblico e, per eseguire alcune attività, potrebbe essere necessario l’accesso amministrativo.

* Sviluppatore
* Amministratore AEM
* Amministratore Experience Cloud

## Introduzione

AEM offre un’integrazione pronta all’uso con i tag. Questa integrazione consente agli amministratori dell’AEM di configurare facilmente i tag tramite un’interfaccia di facile utilizzo, riducendo in tal modo il livello di sforzo e il numero di errori durante la configurazione di questi due strumenti. Inoltre, l’aggiunta dell’estensione Adobe Target ai tag ci aiuterà a utilizzare tutte le funzioni di Adobe Target nelle pagine web dell’AEM.

In questa sezione verranno descritti i seguenti passaggi dell’integrazione:

* Tag
   * Creare una proprietà di tag
   * Aggiunta dell’estensione Target
   * Creare un elemento dati
   * Crea una regola di pagina
   * Ambienti di configurazione
   * Costruire e pubblicare
* AEM
   * Creazione di un Cloud Service
   * Creare

### Tag

#### Creare una proprietà di tag

Una proprietà è un contenitore che si riempie con estensioni, regole, elementi dati e librerie durante la distribuzione di tag nel sito.

1. Accedi alle tue organizzazioni [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
1. Accedi con il tuo Adobe ID e accertati di essere nell’organizzazione corretta.
1. Dal commutatore della soluzione, fai clic su **Experience Platform**, quindi sulla sezione **Raccolta dati** e seleziona **Tag**.

![Experience Cloud - tag](assets/using-launch-adobe-io/exc-cloud-launch.png)

1. Accertati di essere nell’organizzazione corretta, quindi procedi con la creazione di una proprietà di tag.
   ![Experience Cloud - tag](assets/using-launch-adobe-io/launch-create-property.png)

   *Per ulteriori informazioni sulla creazione delle proprietà, vedere [Creare una proprietà](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=it#create-or-configure-a-property) nella documentazione del prodotto.*
1. Fai clic sul pulsante **Nuova proprietà**
1. Specifica un nome per la proprietà (ad esempio, *esercitazione su Target AEM*)
1. Come dominio, immetti *localhost.com* poiché è il dominio in cui è in esecuzione il sito demo WKND. Anche se il campo &#39;*Dominio*&#39; è obbligatorio, la proprietà tags funzionerà in qualsiasi dominio in cui è implementato. Lo scopo principale di questo campo è precompilare le opzioni di menu nel Generatore di regole.
1. Fai clic sul pulsante **Salva**.

   ![tag - Nuova proprietà](assets/using-launch-adobe-io/exc-launch-property.png)

1. Apri la proprietà appena creata e fai clic sulla scheda Estensioni.

#### Aggiunta dell’estensione Target

L&#39;estensione Adobe Target supporta implementazioni lato client tramite Target JavaScript SDK per il moderno Web, `at.js`. I clienti che usano ancora la libreria Target precedente, `mbox.js`, [devono effettuare l&#39;aggiornamento ad at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html?lang=it) per utilizzare i tag.

L’estensione Target è costituita da due parti principali:

* La configurazione dell’estensione, che gestisce le impostazioni della libreria principale
* Azioni di regola per eseguire le operazioni seguenti:
   * Carica Target (at.js)
   * Aggiungi parametri a tutte le mbox
   * Aggiungi parametri a mbox globale
   * Attiva mbox globale

1. In **Estensioni** puoi visualizzare l&#39;elenco delle estensioni già installate per la proprietà dei tag. ([Adobe Launch Core Extension](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) è installato per impostazione predefinita)
2. Fai clic sull&#39;opzione **Catalogo estensioni** e cerca Target nel filtro.
3. Seleziona la versione più recente di Adobe Target at.js e fai clic sull&#39;opzione **Installa**.
   ![Tag - Nuova proprietà](assets/using-launch-adobe-io/launch-target-extension.png)

4. Fai clic sul pulsante **Configura** e noterai la finestra di configurazione con le credenziali dell&#39;account Target importate e la versione at.js per questa estensione.
   ![Destinazione - Configurazione estensione](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Quando Target viene distribuito tramite codici di incorporamento di tag asincroni, è necessario codificare un frammento pre-hiding sulle pagine prima dei codici di incorporamento dei tag per gestire lo sfarfallio del contenuto. Ulteriori informazioni sul frammento pre-hiding saranno disponibili in seguito. Puoi scaricare il frammento pre-hiding [qui](assets/using-launch-adobe-io/prehiding.js)

5. Fai clic su **Salva** per completare l&#39;aggiunta dell&#39;estensione Target alla proprietà dei tag. Ora dovresti essere in grado di visualizzare l&#39;estensione Target elencata nell&#39;elenco delle estensioni **Installate**.

6. Ripeti i passaggi precedenti per cercare l’estensione &quot;Experience Cloud ID Service&quot; e installarla.
   ![Estensione - Servizio ID Experience Cloud](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Ambienti di configurazione

1. Fai clic sulla scheda **Ambiente** per la proprietà del sito per visualizzare l&#39;elenco degli ambienti creati per la proprietà del sito. Per impostazione predefinita, abbiamo un’istanza creata per lo sviluppo, la gestione temporanea e la produzione.

![Elemento Dati - Nome Pagina](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Costruire e pubblicare

1. Fai clic sulla scheda **Pubblicazione** per la proprietà del sito e creiamo una libreria per generare e distribuire le modifiche (elementi dati, regole) in un ambiente di sviluppo.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publish: le modifiche apportate dallo sviluppo a un ambiente di staging.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Eseguire l&#39;opzione **Build per staging**.
4. Una volta completata la compilazione, esegui **Approve for Publishing**, che sposta le modifiche da un ambiente di staging a un ambiente di produzione.
   ![Gestione temporanea in produzione](assets/using-launch-adobe-io/build-staging.png)
5. Infine, esegui l&#39;opzione **Genera e Publish alla produzione** per inviare le modifiche alla produzione.
   ![Genera e Publish in produzione](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Concedi all&#39;integrazione Adobe Developer l&#39;accesso a specifiche aree di lavoro con il ruolo [&#x200B; appropriato per consentire a un team centrale di apportare modifiche basate su API solo a poche aree di lavoro](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html?lang=it).

1. Crea l’integrazione IMS nell’AEM utilizzando le credenziali di Adobe Developer. (01:12 alle 03:55)
2. In Raccolta dati, crea una proprietà. (ha coperto [sopra](#create-launch-property))
3. Utilizzando l’integrazione IMS del passaggio 1, crea l’integrazione dei tag per importare la proprietà tags.
4. In AEM, mappa l’integrazione dei tag su un sito utilizzando la configurazione del browser. (05:28 alle 06:14)
5. Convalida manualmente l’integrazione. (06:15 alle 06:33)
6. Utilizzo del plug-in del browser Adobe Experience Cloud Debugger. (06:51 alle 07:22)

A questo punto, hai integrato correttamente [AEM con Adobe Target utilizzando i tag](./using-aem-cloud-services.md#integrating-aem-target-options) come descritto nell&#39;opzione 1.

Per utilizzare le offerte dei frammenti di esperienza AEM per potenziare le attività di personalizzazione, procedi al capitolo successivo e integra l’AEM con Adobe Target utilizzando i servizi cloud precedenti.
