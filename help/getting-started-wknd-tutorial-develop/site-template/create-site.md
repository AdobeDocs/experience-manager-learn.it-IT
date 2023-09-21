---
title: Creare un sito | Creazione rapida di siti AEM
description: Scopri come utilizzare la procedura guidata di creazione del sito per generare un nuovo sito web. Il modello di sito standard fornito da Adobe è un punto di partenza per il nuovo sito.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
source-git-commit: 0c6294ac468ad4ead041a68f381c6781a5c29b44
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 1%

---

# Creare un sito {#create-site}

Come parte della Creazione Rapida dei Siti, utilizza la Creazione guidata Siti in Adobe Experience Manager, AEM, per generare un nuovo sito web. Il modello di sito standard fornito da Adobe viene utilizzato come punto di partenza per il nuovo sito.

## Prerequisiti {#prerequisites}

I passaggi descritti in questo capitolo si svolgono in un ambiente Adobe Experience Manager as a Cloud Service. Assicurati di disporre di accesso amministrativo all’ambiente AEM. Si consiglia di utilizzare un [Programma sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) e [Ambiente di sviluppo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) al completamento di questa esercitazione.

[Programma di produzione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) anche per questo tutorial è possibile utilizzare gli ambienti; tuttavia, assicurati che le attività di questo tutorial non influiscano sul lavoro eseguito sugli ambienti di destinazione, poiché distribuisce contenuto e codice nell’ambiente AEM di destinazione.

Il [SDK AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html) può essere utilizzato per parti di questa esercitazione. Aspetti di questo tutorial che si basano su servizi cloud, come [distribuzione di temi con la pipeline front-end di Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html), non può essere eseguita con l’SDK per AEM.

Rivedi [documentazione sull’onboarding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) per ulteriori dettagli.

## Obiettivo {#objective}

1. Scopri come generare un nuovo sito con la Creazione guidata.
1. Comprendere il ruolo dei modelli di sito.
1. Esplora il sito AEM generato.

## Accedi a Adobe Experience Manager Author {#author}

Come primo passo, accedi all’ambiente as a Cloud Service dell’AEM. Gli ambienti AEM sono suddivisi tra **Servizio Author** e un **Servizio di pubblicazione**.

* **Servizio Author** : in cui il contenuto del sito viene creato, gestito e aggiornato. In genere solo gli utenti interni hanno accesso a **Servizio Author** e si trova dietro una schermata di accesso.
* **Servizio di pubblicazione** : ospita il sito web live. Si tratta del servizio che verrà visualizzato dagli utenti finali ed è generalmente disponibile al pubblico.

La maggior parte del tutorial si svolge utilizzando **Servizio Author**.

1. Passa a Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Accedi con il tuo account personale o aziendale/scolastico.
1. Assicurati che nel menu sia selezionata l’organizzazione corretta e fai clic su **Experience Manager**.

   ![Home Experience Cloud](assets/create-site/experience-cloud-home-screen.png)

1. Sotto **Cloud Manager** click **Launch**.
1. Passa il puntatore del mouse sul programma che desideri utilizzare e fai clic sul pulsante **Programma Cloud Manager** icona.

   ![Icona del programma Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. Nel menu principale fai clic su **Ambienti** per visualizzare gli ambienti con provisioning.

1. Individua l’ambiente da utilizzare e fai clic su **URL autore**.

   ![Accesso all’authoring di sviluppo](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Si consiglia di utilizzare un **Sviluppo** ambiente per questa esercitazione.

1. Viene avviata una nuova scheda per l’AEM **Servizio Author**. Clic **Accedi con un Adobe** e dovresti aver effettuato l’accesso automaticamente con le stesse credenziali di Experience Cloud.

1. Dopo il reindirizzamento e l’autenticazione, ora viene visualizzata la schermata iniziale dell’AEM.

   ![Schermata iniziale AEM](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Problemi di accesso all’Experience Manager? Rivedi [documentazione sull’onboarding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## Scarica il modello di sito di base

Un modello di sito rappresenta un punto di partenza per un nuovo sito. Un modello di sito include alcuni temi di base, modelli di pagina, configurazioni e contenuti di esempio. Spetta esattamente allo sviluppatore il compito di includere nel modello di sito. L’Adobe fornisce un **Modello di sito di base** per accelerare le nuove implementazioni.

1. Apri una nuova scheda del browser e passa al progetto Modello di sito di base su GitHub: [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). Il progetto è open-source ed è concesso in licenza per essere utilizzato da chiunque.
1. Clic **Versioni** e passare alla [versione più recente](https://github.com/adobe/aem-site-template-standard/releases/più recente).
1. Espandi **Risorse** e scarica il file zip del modello:

   ![File ZIP modello sito di base](assets/create-site/template-basic-zip-file.png)

   Questo file zip viene utilizzato nell’esercizio successivo.

   >[!NOTE]
   >
   > Questo tutorial è scritto utilizzando la versione **1.1.0.** del modello del sito di base. Quando si avvia un nuovo progetto per l’utilizzo in produzione, si consiglia sempre di utilizzare la versione più recente.

## Crea un nuovo sito

Quindi, genera un nuovo sito utilizzando il Modello del sito dell&#39;esercizio precedente.

1. Ritorno nell’ambiente AEM. Dalla schermata iniziale dell’AEM, vai a **Sites**.
1. Nell’angolo in alto a destra fai clic su **Crea** > **Sito (modello)**. Verrà visualizzata la **Creazione guidata sito**.
1. Sotto **Seleziona un modello di sito** fai clic su **Importa** pulsante.

   Carica **.zip** file modello scaricato dall&#39;esercizio precedente.

1. Seleziona la **Modello di base per sito AEM** e fai clic su **Successivo**.

   ![Seleziona modello di sito](assets/create-site/select-site-template.png)

1. Sotto **Dettagli sito** > **Titolo sito** Invio `WKND Site`.

   In un’implementazione reale, &quot;Sito WKND&quot; verrebbe sostituito dal nome del brand dell’azienda o dell’organizzazione. In questo tutorial, stiamo simulando la creazione di un sito per un brand di lifestyle fittizio &quot;WKND&quot;.

1. Sotto **Nome sito** Invio `wknd`.

   ![Dettagli modello sito](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Se utilizzi un ambiente AEM condiviso, aggiungi un identificatore univoco alla sezione **Nome sito**. Ad esempio `wknd-site-johndoe`. In questo modo più utenti possono completare la stessa esercitazione senza conflitti.

1. Clic **Crea** per generare il sito. Clic **Fine** nel **Completato** AEM al termine della creazione del sito web.

## Esplora il nuovo sito

1. Passa alla console AEM Sites, se non è già presente.
1. Una nuova **Sito WKND** è stato generato. Includerà una struttura del sito con una gerarchia multilingue.
1. Apri **Inglese** > **Home** selezionando la pagina e facendo clic sul pulsante **Modifica** nella barra dei menu:

   ![Gerarchia siti WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. Il contenuto iniziale è già stato creato e sono disponibili diversi componenti da aggiungere a una pagina. Sperimenta questi componenti per un’idea delle funzionalità. Scopri le nozioni di base di un componente nel prossimo capitolo.

   ![Pagina iniziale](assets/create-site/start-home-page.png)

   *Contenuto di esempio fornito dal modello del sito*

## Congratulazioni. {#congratulations}

Congratulazioni, hai appena creato il tuo primo sito AEM!

### Passaggi successivi {#next-steps}

Utilizza l’editor pagina in Adobe Experience Manager, AEM, per aggiornare il contenuto del sito in [Creare contenuti e pubblicare](author-content-publish.md) capitolo. Scopri come configurare i Componenti atomici per aggiornare il contenuto. Scopri la differenza tra gli ambienti di authoring e pubblicazione di AEM e come pubblicare gli aggiornamenti sul sito live.
