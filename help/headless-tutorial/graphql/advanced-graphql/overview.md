---
title: Concetti avanzati di AEM senza testa - GraphQL
description: Un tutorial end-to-end che illustra i concetti avanzati delle API di Adobe Experience Manager (AEM) GraphQL.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1076'
ht-degree: 1%

---

# Concetti avanzati di AEM senza testa

Questa esercitazione completa continua la [esercitazione di base](../multi-step/overview.md) che ha trattato i fondamenti di Adobe Experience Manager (AEM) Headless e GraphQL. L’esercitazione avanzata illustra gli aspetti approfonditi dell’utilizzo dei modelli di frammenti di contenuto, dei frammenti di contenuto e delle query persistenti AEM GraphQL, incluso l’utilizzo delle query persistenti GraphQL in un’applicazione client.

## Prerequisiti

Completa il [configurazione rapida per AEM as a Cloud Service](../quick-setup/cloud-service.md) per configurare l&#39;ambiente as a Cloud Service AEM.

Si consiglia vivamente di completare il precedente [esercitazione di base](../multi-step/overview.md) e [serie video](../video-series/modeling-basics.md) tutorial prima di procedere con questa esercitazione avanzata. Anche se puoi completare l’esercitazione utilizzando un ambiente AEM locale, questa esercitazione copre solo il flusso di lavoro per AEM as a Cloud Service.

>[!CAUTION]
>
>Se non hai accesso a AEM ambiente as a Cloud Service, puoi completare [Configurazione rapida AEM Headless tramite SDK locale](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html). Tuttavia, è importante notare che alcune pagine dell’interfaccia utente del prodotto, come la navigazione nei frammenti di contenuto, sono diverse.



## Obiettivi

Questa esercitazione tratta i seguenti argomenti:

* Crea modelli di frammenti di contenuto utilizzando le regole di convalida e tipi di dati più avanzati, ad esempio segnaposto delle schede, riferimenti a frammenti nidificati, oggetti JSON e tipi di dati Data e ora.
* Creazione di frammenti di contenuto durante l’utilizzo di riferimenti a frammenti e contenuti nidificati e configurazione di criteri per la gestione dell’authoring dei frammenti di contenuto.
* Esplora AEM funzionalità API di GraphQL utilizzando query GraphQL con variabili e direttive.
* Permetti alle query GraphQL con parametri in AEM e impara a utilizzare i parametri di controllo della cache con query persistenti.
* Integra le richieste di query persistenti nell’app WKND GraphQL React di esempio utilizzando l’SDK JavaScript senza intestazione AEM.

## Panoramica dei concetti avanzati di AEM Headless

Il video seguente fornisce una panoramica di alto livello dei concetti trattati in questa esercitazione. L’esercitazione include la definizione di modelli di frammenti di contenuto con tipi di dati più avanzati, la nidificazione di frammenti di contenuto e la persistenza di query GraphQL in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/340035?quality=12&learn=on)

>[!CAUTION]
>
>In questo video (alle 2:25) viene menzionato l’installazione dell’editor di query GraphiQL tramite Gestione pacchetti per esplorare le query GraphQL. Tuttavia, nelle versioni più recenti di AEM come Cloud Service, un **Esplora risorse** viene fornito, quindi non è necessario installare il pacchetto. Vedi [Utilizzo dell&#39;IDE GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) per ulteriori informazioni.


## Configurazione del progetto

Il progetto WKND Site dispone di tutte le configurazioni necessarie, quindi puoi avviare l’esercitazione subito dopo aver completato il [configurazione rapida](../quick-setup/cloud-service.md). Questa sezione evidenzia solo alcuni passaggi importanti che puoi utilizzare per creare un tuo progetto AEM Headless.


### Rivedi la configurazione esistente

Il primo passo per avviare un nuovo progetto in AEM è la creazione della sua configurazione, come area di lavoro e per creare endpoint API GraphQL. Per rivedere o creare una configurazione, passa a **Strumenti** > **Generale** > **Browser di configurazione**.

![Passa al browser di configurazione](assets/overview/create-configuration.png)

Osserva che il `WKND Shared` la configurazione del sito è già stata creata per l’esercitazione. Per creare una configurazione per un progetto personalizzato, seleziona **Crea** nell’angolo in alto a destra e completa il modulo nella finestra modale Crea configurazione visualizzata.

![Rivedi configurazione condivisa WKND](assets/overview/review-wknd-shared-configuration.png)

### Revisione degli endpoint API di GraphQL

Successivamente, devi configurare gli endpoint API a cui inviare le query GraphQL. Per rivedere gli endpoint esistenti o crearne uno, passa a **Strumenti** > **Generale** > **GraphQL**.

![Configurare gli endpoint](assets/overview/endpoints.png)

Osserva che il `WKND Shared Endpoint` è già stato creato. Per creare un endpoint per il progetto, seleziona **Crea** nell’angolo in alto a destra e segui il flusso di lavoro .

![Rivedi endpoint condiviso WKND](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> Dopo aver salvato l’endpoint, verrà visualizzato un modale che illustra come visitare la console di sicurezza, che consente di modificare le impostazioni di sicurezza per configurare l’accesso all’endpoint. Tuttavia, le stesse autorizzazioni di sicurezza non rientrano nell’ambito di questa esercitazione. Per ulteriori informazioni, consulta la [Documentazione AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=it).

### Esamina la struttura del contenuto WKND e la cartella principale della lingua

Una struttura dei contenuti ben definita è fondamentale per il successo dell’implementazione AEM headless. È utile per la scalabilità, l’usabilità e la gestione delle autorizzazioni dei contenuti.

Una cartella principale della lingua è una cartella con un codice della lingua ISO come nome, ad esempio EN o FR. Il sistema di gestione della traduzione AEM utilizza queste cartelle per definire la lingua principale dei contenuti e delle lingue per la traduzione dei contenuti.

Vai a **Navigazione** > **Risorse** > **File**.

![Passa a File](assets/overview/files.png)

Passa a **WKND condiviso** cartella. Osserva la cartella con il titolo &quot;Inglese&quot; e il nome &quot;EN&quot;. Questa cartella è la cartella principale della lingua per il progetto WKND Site.

![Cartella inglese](assets/overview/english.png)

Per un progetto personalizzato, crea una cartella principale della lingua all’interno della configurazione. Vedi la sezione su [creazione di cartelle](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) per ulteriori dettagli.

### Assegnare una configurazione alla cartella nidificata

Infine, devi assegnare la configurazione del progetto alla cartella principale della lingua. Questa assegnazione consente di creare frammenti di contenuto basati su modelli di frammenti di contenuto definiti nella configurazione del progetto.

Per assegnare la cartella principale della lingua alla configurazione, selezionare la cartella, quindi selezionare **Proprietà** nella barra di navigazione superiore.

![Seleziona proprietà](assets/overview/properties.png)

Quindi, passa alla **Cloud Services** e seleziona l’icona della cartella nella scheda **Configurazione cloud** campo .

![Configurazione cloud](assets/overview/cloud-conf.png)

Nel modale visualizzato, seleziona la configurazione creata in precedenza per assegnare la cartella principale della lingua.

### Best practice

Di seguito sono riportate le best practice per la creazione di un progetto personalizzato in AEM:

* La gerarchia delle cartelle deve essere basata sulla localizzazione e la traduzione. In altre parole, le cartelle di lingua devono essere nidificate all&#39;interno delle cartelle di configurazione, il che consente una facile traduzione del contenuto all&#39;interno di tali cartelle di configurazione.
* La gerarchia delle cartelle deve essere mantenuta semplice e lineare. Evita di spostare o rinominare cartelle e frammenti in un secondo momento, specialmente dopo la pubblicazione per l’utilizzo live, in quanto cambia i percorsi che possono influenzare i riferimenti ai frammenti e le query GraphQL.

## Pacchetti iniziali e di soluzione

Due AEM **pacchetti** sono disponibili e possono essere installati tramite [Gestione pacchetti](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) viene utilizzato più avanti nell’esercitazione e contiene immagini e cartelle di esempio.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) contiene la soluzione completa per i capitoli da 1 a 4, inclusi nuovi modelli di frammenti di contenuto, frammenti di contenuto e query GraphQL persistenti. Utile per coloro che desiderano saltare direttamente nel [Integrazione di applicazioni client](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) capitolo.


La [React App - Tutorial avanzato - Avventure WKND](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) è disponibile per esaminare ed esplorare l’applicazione di esempio. Questa applicazione di esempio recupera il contenuto da AEM richiamando le query GraphQL persistenti ed esegue il rendering in un’esperienza coinvolgente.

## Guida introduttiva

Per iniziare a utilizzare questa esercitazione avanzata, segui questi passaggi:

1. Configurare un ambiente di sviluppo utilizzando [AEM as a Cloud Service](../quick-setup/cloud-service.md).
1. Avvia il capitolo tutorial su [Creare modelli di frammenti di contenuto](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
