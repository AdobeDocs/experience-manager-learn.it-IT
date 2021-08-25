---
title: Integrazione di Adobe Experience Manager con Adobe Target tramite Experience Platform Launch ed Adobe I/O
seo-title: Integrating Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
description: Procedura dettagliata su come integrare Adobe Experience Manager con Adobe Target utilizzando Experience Platform Launch ed Adobe I/O
seo-description: Step by step walk-through on how to integrate Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1064'
ht-degree: 2%

---


# Utilizzo di Adobe Experience Platform Launch tramite Adobe I/O Console

## Prerequisiti

* [AEM creare e pubblicare ](./implementation.md#set-up-aem) instancerunning rispettivamente sulle porte localhost 4502 e 4503
* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console Adobe I/O](https://console.adobe.io)

      >[!NOTE]
      >Devi disporre delle autorizzazioni per sviluppare, approvare, pubblicare, gestire le estensioni e gestire gli ambienti in Launch. Se non riesci a completare nessuna di queste operazioni perché le opzioni dell’interfaccia utente non sono disponibili, contatta l’amministratore di Experience Cloud per richiedere l’accesso. Per ulteriori informazioni sulle autorizzazioni di Launch, [consulta la documentazione](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).


* **Plug-in browser**
   * Debugger Adobe Experience Cloud ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Interruttore Launch e DTM ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Utenti coinvolti

Per questa integrazione, è necessario coinvolgere i seguenti tipi di pubblico ed eseguire alcune attività, potrebbe essere necessario l’accesso amministrativo.

* Developer (Sviluppatore)
* Amministratore AEM
* Amministratore Experience Cloud

## Introduzione

AEM offre un’integrazione standard con il Experience Platform Launch. Questa integrazione consente agli amministratori AEM di configurare facilmente il Experience Platform Launch tramite un’interfaccia di facile utilizzo, riducendo così il livello di impegno e il numero di errori durante la configurazione di questi due strumenti. E solo aggiungendo l’estensione Adobe Target al Experience Platform Launch potrai utilizzare tutte le funzioni di Adobe Target nelle pagine web AEM.

In questa sezione tratteremo i seguenti passaggi di integrazione:

* Lancio
   * Creare una proprietà Launch
   * Aggiunta di un’estensione Target
   * Creare un elemento dati
   * Creare una regola di pagina
   * Ambienti di configurazione
   * Creare e pubblicare
* AEM
   * Creare un Cloud Service
   * Crea

### Lancio

#### Creare una proprietà Launch

Una proprietà è un contenitore che si riempie con estensioni, regole, elementi dati e librerie durante la distribuzione di tag sul sito.

1. Passa alle organizzazioni [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
2. Accedi utilizzando il tuo Adobe ID e assicurati di essere nell&#39;organizzazione corretta.
3. Dal commutatore della soluzione, fai clic su **Launch**, quindi seleziona il pulsante **Vai a Launch** .

   ![Experience Cloud - Launch](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Assicurati di essere nell&#39;organizzazione corretta e quindi procedi con la creazione di una proprietà Launch.
   ![Experience Cloud - Launch](assets/using-launch-adobe-io/launch-create-property.png)

   *Per ulteriori informazioni sulla creazione delle proprietà, consulta  [Creare una ](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) proprietà nella documentazione del prodotto.*
5. Fai clic sul pulsante **Nuova proprietà**
6. Specifica un nome per la proprietà (ad esempio, *AEM tutorial di Target*)
7. Come dominio, immetti *localhost.com* poiché si tratta del dominio su cui è in esecuzione il sito demo WKND. Anche se il campo &#39;*Dominio*&#39; è obbligatorio, la proprietà Launch funzionerà su qualsiasi dominio in cui è implementato. Lo scopo principale di questo campo è precompilare le opzioni di menu nel Generatore di regole.
8. Fare clic sul pulsante **Salva**.

   ![Launch - Nuova proprietà](assets/using-launch-adobe-io/exc-launch-property.png)

9. Apri la proprietà appena creata e fai clic sulla scheda Estensioni .

#### Aggiunta di un’estensione Target

L&#39;estensione Adobe Target supporta implementazioni lato client tramite SDK JavaScript di Target per il moderno Web, `at.js`. I clienti che usano ancora una libreria Target precedente, `mbox.js`, [devono effettuare l’aggiornamento ad at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) per utilizzare Launch.

L’estensione Target è costituita da due parti principali:

* La configurazione dell&#39;estensione, che gestisce le impostazioni della libreria principale
* Azioni di regola per eseguire le seguenti operazioni:
   * Caricare Target (at.js)
   * Aggiungi parametri a tutte le mbox
   * Aggiungi parametri a mbox globale
   * Attiva mbox globale

1. Alla voce **Estensioni**, puoi visualizzare l’elenco delle estensioni già installate per la proprietà Launch. ([Estensione core Experience Platform Launch](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) è installato per impostazione predefinita)
2. Fai clic sull&#39;opzione **Catalogo estensioni** e cerca Target nel filtro.
3. Seleziona la versione più recente di Adobe Target at.js e fai clic sull’opzione **Installa** .
   ![Launch -New, proprietà](assets/using-launch-adobe-io/launch-target-extension.png)

4. Fai clic sul pulsante **Configura** e puoi notare la finestra di configurazione con le credenziali dell’account Target importate e la versione at.js per questa estensione.
   ![Target - Configurazione dell&#39;estensione](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Quando Target viene distribuito tramite codici di incorporamento Launch asincroni, è necessario codificare un frammento pre-hiding sulle pagine prima dei codici di incorporamento Launch per gestire lo sfarfallio del contenuto. Ulteriori informazioni sul frammento pre-hiding in un secondo momento. Puoi scaricare il frammento pre-hiding [qui](assets/using-launch-adobe-io/prehiding.js)

5. Fai clic su **Salva** per completare l&#39;aggiunta dell&#39;estensione Target alla proprietà Launch e dovresti ora essere in grado di visualizzare l&#39;estensione Target elencata nell&#39;elenco delle estensioni **Installed** .

6. Ripeti i passaggi precedenti per cercare l’estensione &quot;Experience Cloud ID Service&quot; e installarla.
   ![Estensione - Servizio Experience Cloud ID](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Ambienti di configurazione

1. Fai clic sulla scheda **Ambiente** per la proprietà del sito e puoi visualizzare l’elenco dell’ambiente che viene creato per la proprietà del sito. Per impostazione predefinita, è stata creata una singola istanza per sviluppo, staging e produzione.

![Elemento dati - Nome pagina](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Creare e pubblicare

1. Fai clic sulla scheda **Pubblicazione** per la proprietà del sito, creiamo una libreria da generare e distribuiamo le nostre modifiche (elementi dati, regole) in un ambiente di sviluppo.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Pubblica le modifiche da Sviluppo a un ambiente di staging.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Esegui l&#39;opzione **Genera per staging**.
4. Una volta completata la build, esegui **Approva per la pubblicazione**, che sposta le modifiche da un ambiente di staging a un ambiente di produzione.
   ![Staging to Production](assets/using-launch-adobe-io/build-staging.png)
5. Infine, esegui l&#39;opzione **Genera e pubblica in produzione** per inviare le modifiche alla produzione.
   ![Creare e pubblicare in produzione](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Concedi all’integrazione di Adobe I/O l’accesso a specifiche aree di lavoro con il ruolo [appropriato per consentire a un team centrale di apportare modifiche basate su API solo in alcune aree di lavoro](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Crea l’integrazione IMS in AEM utilizzando le credenziali di Adobe I/O. (da 01:12 a 03:55)
2. In Experience Platform Launch, crea una proprietà. (coperto [sopra](#create-launch-property))
3. Utilizzando l’integrazione IMS dal passaggio 1, crea l’integrazione di Experience Platform Launch per importare la proprietà Launch.
4. In AEM, mappa l’integrazione del Experience Platform Launch a un sito utilizzando la configurazione del browser. (da 05:28 a 06:14)
5. Convalida l’integrazione manualmente. (da 06:15 a 06:33)
6. Utilizzo del plug-in del browser Launch/DTM. (da 06:34 a 06:50)
7. Utilizzo del plug-in del browser Adobe Experience Cloud Debugger. (da 06:51 a 07:22)

A questo punto, hai integrato con successo [AEM con Adobe Target utilizzando Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) come descritto nell&#39;Opzione 1.

Per utilizzare AEM offerte di Frammenti esperienza per abilitare le attività di personalizzazione, puoi passare al capitolo successivo e integrare AEM con Adobe Target utilizzando i servizi cloud legacy.
