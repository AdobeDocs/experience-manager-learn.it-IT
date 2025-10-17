---
title: Creare un sito | Creazione rapida di siti AEM
description: Scopri come utilizzare la procedura guidata per la creazione di siti per generare un nuovo sito web. Il modello per siti standard fornito da Adobe è un punto di partenza per il nuovo sito.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7496
thumbnail: KT-7496.jpg
doc-type: Tutorial
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
duration: 198
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '959'
ht-degree: 100%

---

# Creare un sito {#create-site}

Come parte della creazione rapida dei siti, utilizza la procedura guidata per la creazione di siti in Adobe Experience Manager, AEM, per generare un nuovo sito web. Il modello per siti standard fornito da Adobe viene utilizzato come punto di partenza per il nuovo sito.

## Prerequisiti {#prerequisites}

I passaggi in questo capitolo si svolgono in un ambiente Adobe Experience Manager as a Cloud Service. Assicurati di disporre dell’accesso amministrativo all’ambiente AEM. Per completare questo tutorial, si consiglia di utilizzare un [programma sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html?lang=it) e un [ambiente di sviluppo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=it).

Per questo tutorial è possibile utilizzare anche gli ambienti del [programma di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=it); tuttavia, assicurati che le rispettive attività non influiscano sul lavoro eseguito sugli ambienti di destinazione, poiché tale tutorial distribuisce il contenuto e il codice nell’ambiente AEM di destinazione.

È possibile utilizzare [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=it) per alcune parti di questo tutorial. Gli aspetti di questo tutorial che si basano su servizi cloud, come [la distribuzione di temi con la pipeline front-end di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=it), non possono essere eseguiti su AEM SDK.

Per ulteriori dettagli, rivedi la [documentazione di onboarding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=it).

## Obiettivo {#objective}

1. Scopri come utilizzare la procedura guidata per la creazione dei siti per generarne un nuovo.
1. Informazioni sul ruolo dei modelli di sito.
1. Esplora il sito AEM generato.

## Accedere all’authoring di Adobe Experience Manager {#author}

Come primo passo, accedi all’ambiente AEM as a Cloud Service. Gli ambienti AEM sono suddivisi tra un **servizio di authoring** e un **servizio di pubblicazione**.

* **Servizio di authoring**: in cui il contenuto del sito viene creato, gestito e aggiornato. In genere solo gli utenti interni hanno accesso al **servizio di authoring**, che è protetto da una schermata di accesso.
* **Servizio di pubblicazione**: ospita il sito web attivo. Si tratta del servizio che verrà visualizzato dagli utenti finali ed è generalmente disponibile al pubblico.

La maggior parte del tutorial si svolgerà utilizzando il **servizio di authoring**.

1. Passa ad Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Accedi con il tuo account personale o aziendale/scolastico.
1. Assicurati che nel menu sia selezionata l’organizzazione corretta e fai clic su **Experience Manager**.

   ![Home di Experience Cloud](assets/create-site/experience-cloud-home-screen.png)

1. In **Cloud Manager** fai clic su **Avvia**.
1. Passa il puntatore del mouse sul programma che desideri utilizzare e fai clic sull’icona **Programma Cloud Manager**.

   ![Icona programma Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. Nel menu principale fai clic su **Ambienti** per visualizzare gli ambienti per i quali è stato eseguito il provisioning.

1. Individua l’ambiente da utilizzare e fai clic sull’**URL di authoring**.

   ![Accedere all’authoring di sviluppo](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Per questo tutorial si consiglia di utilizzare un ambiente di **sviluppo**.

1. Viene avviata una nuova scheda per il **servizio di authoring** di AEM. Fai clic su **Accedi con Adobe** e l’accesso avverrà automaticamente con le stesse credenziali Experience Cloud.

1. Dopo il reindirizzamento e l’autenticazione, ora viene visualizzata la schermata iniziale di AEM.

   ![Schermata iniziale di AEM](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Problemi di accesso a Experience Manager? Consulta la [documentazione sull’onboarding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=it)

## Scaricare il modello per siti di base

Un modello per siti rappresenta un punto di partenza per un nuovo sito. Un modello per siti include alcuni temi di base, modelli di pagina, configurazioni e contenuti di esempio. Spetta allo sviluppatore decidere cosa includere esattamente nel modello per siti. Adobe fornisce un **Modello per siti di base** per accelerare le nuove implementazioni.

1. Apri una nuova scheda del browser e passa al progetto Modello per siti di base su GitHub: [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). Il progetto è open-source ed è concesso in licenza per essere utilizzato da chiunque.
1. Fai clic su **Versioni** e vai alla [versione più recente](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. Espandi il menu a discesa **Risorse** e scarica il file zip del modello:

   ![File zip modello sito di base](assets/create-site/template-basic-zip-file.png)

   Questo file zip viene utilizzato nell&#39;esercizio successivo.

   >[!NOTE]
   >
   > Questa esercitazione è stata scritta con la versione **1.1.0** del Modello per siti di base. Quando si avvia un nuovo progetto per l&#39;utilizzo in produzione, si consiglia sempre di utilizzare la versione più recente.

## Creare un nuovo sito

Quindi, genera un nuovo sito utilizzando il Modello per siti dell&#39;esercizio precedente.

1. Torna all&#39;ambiente AEM. Dalla schermata iniziale di AEM, passa a **Sites**.
1. Nell&#39;angolo superiore destro fai clic su **Crea** > **Sito (Modello)**. Verrà visualizzata la **Creazione guidata sito**.
1. In **Seleziona un modello per siti** fai clic sul pulsante **Importa**.

   Carica il file modello **.zip** scaricato dall&#39;esercizio precedente.

1. Seleziona il **modello per siti AEM di base** e fai clic su **Avanti**.

   ![Selezionare un modello](assets/create-site/select-site-template.png)

1. In **Dettagli sito** > **Titolo sito** immetti `WKND Site`.

   In un&#39;implementazione reale, &quot;Sito WKND&quot; verrebbe sostituito dal nome del brand dell&#39;azienda o dell&#39;organizzazione. In questa esercitazione, stiamo simulando la creazione di un sito per un brand di lifestyle fittizio &quot;WKND&quot;.

1. In **Nome sito** immetti `wknd`.

   ![Dettagli del modello per siti](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Se utilizzi un ambiente AEM condiviso, aggiungi un identificatore univoco al **Nome sito**. Ad esempio `wknd-site-johndoe`. In questo modo più utenti possono completare la stessa esercitazione senza conflitti.

1. Fai clic su **Crea** per generare il sito. Fai clic su **Fine** nella finestra di dialogo **Operazione riuscita** al termine della creazione del sito web da parte di AEM.

## Esplorare il nuovo sito

1. Passa alla console AEM Sites, se non è già visualizzata.
1. È stato generato un nuovo **Sito WKND**. Includerà una struttura del sito con una gerarchia multilingue.
1. Apri la pagina **Inglese** > **Home** selezionando la pagina e facendo clic sul pulsante **Modifica** nella barra dei menu:

   ![Gerarchia del sito WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. Il contenuto iniziale è già stato creato e sono disponibili diversi componenti da aggiungere a una pagina. Prova questi componenti per un&#39;idea delle funzionalità. Scopri le nozioni di base di un componente nel prossimo capitolo.

   ![Pagina home iniziale](assets/create-site/start-home-page.png)

   *Contenuto di esempio fornito dal Modello per siti*

## Congratulazioni. {#congratulations}

Congratulazioni, hai appena creato il tuo primo sito AEM!

### Passaggi successivi {#next-steps}

Utilizza l&#39;Editor pagina in Adobe Experience Manager, AEM, per aggiornare il contenuto del sito nel capitolo [Creare il contenuto e pubblicare](author-content-publish.md). Scopri come configurare i Componenti atomici per aggiornare il contenuto. Scopri la differenza tra gli ambienti Crea e Pubblica di AEM e come pubblicare gli aggiornamenti sul sito live.
