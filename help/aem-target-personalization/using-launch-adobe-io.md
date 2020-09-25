---
title: Integrazione di Adobe Experience Manager con  Adobe Target con Experience Platform Launch e  Adobe I/O
seo-title: Integrazione di Adobe Experience Manager con  Adobe Target con Experience Platform Launch e  Adobe I/O
description: Procedura dettagliata sull'integrazione di Adobe Experience Manager con  Adobe Target con Experience Platform Launch e  Adobe I/O
seo-description: Procedura dettagliata sull'integrazione di Adobe Experience Manager con  Adobe Target con Experience Platform Launch e  Adobe I/O
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 2%

---


# Utilizzo  Adobe Experience Platform Launch tramite  console I/O Adobe

## Prerequisiti

* [AEM istanza](./implementation.md#set-up-aem) di creazione e pubblicazione eseguita rispettivamente sulle porte localhost 4502 e 4503
* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - <https://>`<yourcompany>`.experienceecCloud.adobe.com
   *  Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console I/O Adobe](https://console.adobe.io)

      >[!NOTE]
      >È necessario disporre dell&#39;autorizzazione per sviluppare, approvare, pubblicare, gestire estensioni e gestire ambienti in Launch. Se non riuscite a completare nessuno di questi passaggi perché le opzioni dell&#39;interfaccia utente non sono disponibili, rivolgetevi al vostro amministratore  Experience Cloud per richiedere l&#39;accesso. Per ulteriori informazioni sulle autorizzazioni di avvio, [consulta la documentazione](https://docs.adobelaunch.com/administration/user-permissions).


* **Plug-in browser**
   * Adobe Experience Cloud Debugger ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Launch e DTM Switch ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Utenti coinvolti

Per questa integrazione, è necessario coinvolgere i seguenti tipi di pubblico e, per eseguire alcune attività, potrebbe essere necessario disporre dell&#39;accesso amministrativo.

* Developer (Sviluppatore)
* Amministratore AEM
* Amministratore Experience Cloud 

## Introduzione

AEM offre un&#39;integrazione out-of-the-box con il Experience Platform Launch. Questa integrazione consente AEM amministratori di configurare facilmente il Experience Platform Launch tramite un&#39;interfaccia di facile utilizzo, riducendo il livello di sforzo e il numero di errori durante la configurazione di questi due strumenti. E solo aggiungendo  estensione Adobe Target al Experience Platform Launch ci aiuterà a utilizzare tutte le funzionalità di  Adobe Target sulle pagine Web AEM.

In questa sezione verranno trattati i seguenti passaggi di integrazione:

* Lancio
   * Creare una proprietà Launch
   * Aggiunta di un&#39;estensione di destinazione
   * Creazione di un elemento dati
   * Creare una regola di pagina
   * Ambienti di configurazione
   * Creazione e pubblicazione
* AEM
   * Creare un Cloud Service
   * Crea

### Lancio

#### Creare una proprietà Launch

Una proprietà è un contenitore che può essere riempito con estensioni, regole, elementi dati e librerie durante la distribuzione dei tag nel sito.

1. Passa a [Adobe Experience Cloud](https://experiencecloud.adobe.com/) organizzazione (<https://>`<yourcompany>`.experienceecCloud.adobe.com)
2. Accedete utilizzando il vostro Adobe ID  e accertatevi di essere nell&#39;organizzazione corretta.
3. Dallo switcher della soluzione, fare clic su **Avvia** , quindi selezionare il pulsante **Vai al lancio** .

   ![Experience Cloud  - Lancio](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Accertatevi di essere nell&#39;organizzazione corretta e quindi continuate a creare una proprietà Launch.
   ![Experience Cloud  - Lancio](assets/using-launch-adobe-io/launch-create-property.png)

   *Per ulteriori informazioni sulla creazione di proprietà, consultate[Creare una proprietà](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property)nella documentazione del prodotto.*
5. Fate clic sul pulsante **Nuova proprietà**
6. Immettete un nome per la proprietà (ad esempio, *AEM Esercitazione* di Target)
7. Come dominio, immettete *localhost.com* perché si tratta del dominio su cui è in esecuzione il sito demo WKND. Anche se il campo &#39;*Domain*&#39; è obbligatorio, la proprietà Launch funziona su qualsiasi dominio in cui è implementato. Lo scopo principale di questo campo è precompilare le opzioni di menu nel generatore di regole.
8. Fate clic sul pulsante **Salva** .

   ![Launch - Nuova proprietà](assets/using-launch-adobe-io/exc-launch-property.png)

9. Aprite la proprietà appena creata e fate clic sulla scheda Estensioni.

#### Aggiunta di un&#39;estensione di destinazione

L&#39;estensione Adobe Target  supporta implementazioni lato client tramite l&#39;SDK JavaScript di Target per il Web moderno `at.js`. I clienti che ancora utilizzano la libreria di Target precedente `mbox.js`devono effettuare l&#39;aggiornamento ad at.js [](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html) per utilizzare Launch.

L&#39;estensione Target è costituita da due parti principali:

* La configurazione dell&#39;estensione, che gestisce le impostazioni della libreria di base
* Regola le azioni per eseguire le seguenti operazioni:
   * Load Target (at.js)
   * Aggiungi parametri a tutte le mbox
   * Aggiungi parametri a mbox globale
   * Fire Global Mbox

1. In **Estensioni**&#x200B;è disponibile l&#39;elenco delle estensioni già installate per la proprietà Launch. ([Experience Platform Launch Estensione](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) di base installata per impostazione predefinita)
2. Fate clic sull’opzione **Catalogo** estensioni e cercate Target nel filtro.
3. Seleziona la versione più recente di  Adobe Target at.js e fai clic sull’opzione **Installa** .
   ![Launch -New, proprietà](assets/using-launch-adobe-io/launch-target-extension.png)

4. Fate clic sul pulsante **Configura** e potete vedere la finestra di configurazione con le credenziali dell&#39;account Target importate e la versione at.js per questa estensione.
   ![Destinazione - Configurazione estensione](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Quando Target viene distribuito tramite codici di incorporamento Launch asincrono, per gestire lo sfarfallio del contenuto è necessario codificare uno snippet di pre-occultamento sulle pagine prima dei codici di incorporamento Launch. Scopriremo di più sul frammento di pre-occultamento più tardi. Puoi scaricare lo snippet di pre-occultamento [qui](assets/using-launch-adobe-io/prehiding.js)

5. Fai clic su **Salva** per completare l&#39;aggiunta dell&#39;estensione Target alla proprietà Launch. Ora dovresti essere in grado di visualizzare l&#39;estensione Target elencata nell&#39;elenco delle estensioni **installate** .

6. Ripetete i passaggi descritti qui sopra per cercare l&#39;estensione &quot; Experience Cloud ID Service&quot; e installarla.
   ![Estensione -  servizio ID Experience Cloud](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Ambienti di configurazione

1. Fare clic sulla scheda **Ambiente** relativa alla proprietà del sito per visualizzare l&#39;elenco dell&#39;ambiente creato per la proprietà del sito. Per impostazione predefinita, viene creata un&#39;istanza per lo sviluppo, l&#39;area di gestione temporanea e la produzione.

![Elemento dati - Nome pagina](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Creazione e pubblicazione

1. Fate clic sulla scheda **Pubblicazione** per la proprietà del sito e create una libreria per creare e distribuire le modifiche (elementi di dati, regole) in un ambiente di sviluppo.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Pubblicare le modifiche dallo sviluppo a un ambiente di gestione temporanea.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Eseguite l&#39;opzione **Genera per gestione temporanea**.
4. Una volta completata la build, esegui **Approva per la pubblicazione**, trasferendo le modifiche da un ambiente di gestione temporanea a un ambiente di produzione.
   ![Staging alla produzione](assets/using-launch-adobe-io/build-staging.png)
5. Infine, eseguite l&#39;opzione **Genera e pubblica in produzione** per inviare le modifiche alla produzione.
   ![Creazione e pubblicazione in produzione](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> L&#39;integrazione I/O  Adobe consente di accedere a aree di lavoro selezionate con il [ruolo appropriato per consentire a un team centrale di apportare modifiche guidate dall&#39;API solo in alcune aree](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)di lavoro.

1. Creare l&#39;integrazione IMS in AEM utilizzando le credenziali  I/O del Adobe. (da 01:12 a 03:55)
2. Ad Experience Platform Launch, create una proprietà. (di [cui sopra](#create-launch-property))
3. Utilizzando l&#39;integrazione IMS dal passaggio 1, create l&#39;integrazione del Experience Platform Launch per importare la proprietà Launch.
4. In AEM, mappate l&#39;integrazione del Experience Platform Launch su un sito utilizzando la configurazione del browser. (da 05:28 a 06:14)
5. Convalida dell&#39;integrazione manualmente. (da 06:15 a 06:33)
6. Utilizzo del plug-in per browser Launch/DTM. (da 06:34 a 06:50)
7. Utilizzo del plug-in del browser Adobe Experience Cloud Debugger. (da 06:51 a 07:22)

A questo punto, avete integrato con successo [AEM con  Adobe Target utilizzando  Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) come illustrato nell&#39;opzione 1.

Per utilizzare AEM offerte relative ai frammenti esperienza per sviluppare attività di personalizzazione, puoi passare al capitolo successivo e integrare AEM con  Adobe Target utilizzando i servizi cloud precedenti.
