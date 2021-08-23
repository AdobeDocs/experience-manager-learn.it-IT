---
title: Creare un sito
seo-title: Guida introduttiva ad AEM Sites - Creare un sito
description: Utilizzare la Creazione guidata sito in Adobe Experience Manager, AEM, per generare un nuovo sito Web. Il modello di sito standard fornito da Adobe viene utilizzato come punto di partenza per il nuovo sito.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Gestione dei contenuti
feature: Componenti core, Editor pagina
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---


# Creare un sito {#create-site}

>[!CAUTION]
>
> Le funzionalità di creazione rapida dei siti mostrate qui saranno rilasciate nella seconda metà del 2021. La relativa documentazione è disponibile a scopo di anteprima.

Questo capitolo illustra la creazione di un nuovo sito in Adobe Experience Manager. Il modello di sito standard, fornito da Adobe, viene utilizzato come punto iniziale.

## Prerequisiti {#prerequisites}

I passaggi descritti in questo capitolo si svolgeranno in un ambiente Adobe Experience Manager as a Cloud Service . Assicurati di disporre dell&#39;accesso amministrativo all&#39;ambiente AEM. Al completamento di questa esercitazione, si consiglia di utilizzare un [programma sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) e un [ambiente di sviluppo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html).

Per ulteriori informazioni, consulta la [documentazione di onboarding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) .

## Obiettivo {#objective}

1. Scopri come utilizzare la Creazione guidata sito per generare un nuovo sito.
1. Comprendere il ruolo dei modelli di sito.
1. Esplora il sito AEM generato.

## Accedere ad Adobe Experience Manager Author {#author}

Come primo passo, accedi al tuo AEM come ambiente di Cloud Service. Gli ambienti AEM sono suddivisi tra **Author Service** e **Publish Service**.

* **Servizio authoring** : per creare, gestire e aggiornare il contenuto del sito. In genere solo gli utenti interni hanno accesso al **Servizio authoring** ed è dietro una schermata di accesso.
* **Servizio pubblicazione** : ospita il sito web live. Questo è il servizio che gli utenti finali vedranno ed è generalmente disponibile al pubblico.

La maggior parte dell’esercitazione verrà eseguita utilizzando il **Servizio authoring**.

1. Passa a Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Accedi utilizzando il tuo account personale o un account aziendale/scuola.
1. Assicurati che nel menu sia selezionata l&#39;organizzazione corretta e fai clic su **Experience Manager**.

   ![Pagina principale di Experience Cloud](assets/create-site/experience-cloud-home-screen.png)

1. In **Cloud Manager** fai clic su **Launch**.
1. Passa il puntatore del mouse sul programma da utilizzare e fai clic sull&#39;icona **Programma Cloud Manager** .

   ![Icona del programma Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. Nel menu principale fai clic su **Ambienti** per visualizzare gli ambienti di provisioning.

1. Trova l’ambiente da utilizzare e fai clic su **URL autore**.

   ![Accedere all&#39;autore di sviluppo](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Per questa esercitazione, si consiglia di utilizzare un ambiente **Sviluppo** .

1. Verrà avviata una nuova scheda al AEM **Servizio authoring**. Fai clic su **Accedi con Adobe** e devi aver effettuato l&#39;accesso automaticamente con le stesse credenziali Experienci Cloud.

1. Dopo essere stato reindirizzato e autenticato, è ora possibile visualizzare la schermata iniziale AEM.

   ![AEM schermata Start](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Hai problemi ad accedere ad Experience Manager? Rivedi la [documentazione di onboarding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## Scarica il modello di sito di base

Un modello di sito fornisce un punto di partenza per un nuovo sito. Un modello di sito include alcuni temi di base, modelli di pagina, configurazioni e contenuti di esempio. Esattamente ciò che è incluso nel modello di sito dipende dallo sviluppatore. Adobe fornisce un **Modello di sito di base** per accelerare le nuove implementazioni.

1. Apri una nuova scheda del browser e passa al progetto Modello di sito di base su GitHub: [https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic). Il progetto è open-source e concesso in licenza per essere utilizzato da chiunque.
1. Fai clic su **Rilasci** e passa alla [versione più recente](https://github.com/adobe/aem-site-template-basic/releases/latest).
1. Espandi il menu a discesa **Risorse** e scarica il file zip del modello:

   ![ZIP modello sito di base](assets/create-site/template-basic-zip-file.png)

   Questo file zip verrà utilizzato nell&#39;esercizio successivo.

   >[!NOTE]
   >
   > Questa esercitazione viene scritta utilizzando la versione **5.0.0** del modello di sito di base. Se si avvia un nuovo progetto, si consiglia sempre di utilizzare la versione più recente.

## Crea un nuovo sito

Quindi, genera un nuovo sito utilizzando il modello di sito dell&#39;esercizio precedente.

1. Torna all’ambiente AEM. Dalla schermata iniziale AEM passare a **Sites**.
1. Nell&#39;angolo in alto a destra fai clic su **Crea** > **Sito (modello)**. Verrà visualizzata la **Creazione guidata sito**.
1. In **Seleziona un modello di sito** fai clic sul pulsante **Importa** .

   Carica il file modello **.zip** scaricato dall&#39;esercizio precedente.

1. Seleziona il **Modello di sito di base AEM** e fai clic su **Avanti**.

   ![Seleziona modello di sito](assets/create-site/select-site-template.png)

1. In **Dettagli sito** > **Titolo sito** immetti `WKND Site`.
1. Sotto **Nome sito** immetti `wknd`.

   ![Dettagli del modello del sito](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Se utilizzi un ambiente AEM condiviso, aggiungi un identificatore univoco al **Nome sito**. Esempio `wknd-johndoe`. In questo modo, più utenti potranno completare la stessa esercitazione senza conflitti.

1. Fare clic su **Crea** per generare il sito. Al termine della creazione del sito Web, fai clic su **Fine** nella finestra di dialogo **Success** .

## Esplora il nuovo sito

1. Se non è già presente, passa alla console AEM Sites .
1. È stato generato un nuovo **sito WKND**. Includerà una struttura del sito con una gerarchia multilingue.
1. Apri la pagina **Inglese** > **Home** selezionando la pagina e facendo clic sul pulsante **Modifica** nella barra dei menu:

   ![Gerarchia del sito WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. Il contenuto iniziale è già stato creato e diversi componenti possono essere aggiunti a una pagina. Sperimenta con questi componenti per avere un&#39;idea della funzionalità. Scoprirai le nozioni di base di un componente nel capitolo successivo.

   ![Pagina iniziale iniziale](assets/create-site/start-home-page.png)

   *Contenuto di esempio fornito dal modello di sito*

## Congratulazioni! {#congratulations}

Congratulazioni, avete appena creato il vostro primo sito AEM!

### Passaggi successivi {#next-steps}

Utilizza l’editor pagina in Adobe Experience Manager, AEM, per aggiornare il contenuto del sito nel capitolo [Creazione di contenuti e pubblicazione](author-content-publish.md) . Scopri come configurare i componenti atomici per l’aggiornamento dei contenuti. Scopri la differenza tra un ambiente di authoring e uno di pubblicazione AEM e come pubblicare gli aggiornamenti sul sito live.
