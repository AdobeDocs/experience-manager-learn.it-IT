---
title: Giornale di registrazione ed eventi AEM
description: Scopri come recuperare il set iniziale di eventi AEM dal diario ed esplorare i dettagli di ogni evento.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 144
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# Giornale di registrazione ed eventi AEM

Scopri come recuperare il set iniziale di eventi AEM dal diario ed esplorare i dettagli di ogni evento.

Il journal è un metodo pull per utilizzare gli eventi AEM e un journal è un elenco ordinato di eventi. Utilizzando l’API di inserimento nel journal degli eventi di Adobe I/O, puoi recuperare gli eventi AEM dal journal ed elaborarli nell’applicazione. Questo approccio consente di gestire gli eventi in base a una frequenza specifica ed elaborarli in modo efficiente in blocco. Consulta la sezione [Inserimento nel journal](https://developer.adobe.com/events/docs/guides/journaling_intro/) per informazioni approfondite, incluse considerazioni essenziali come i periodi di conservazione, l’impaginazione e altro ancora.

All’interno del progetto Adobe Developer Console, ogni registrazione degli eventi viene abilitata automaticamente per il journaling, consentendo un’integrazione perfetta.

In questo esempio, utilizzando un Adobe fornito _applicazione web in hosting_ consente di recuperare il primo batch di eventi AEM dal giornale di registrazione senza dover configurare l’applicazione. Questa applicazione web fornita dall’Adobe è ospitata su [Glitch](https://glitch.com/), piattaforma nota per offrire un ambiente basato su Web in grado di favorire la creazione e l&#39;implementazione di applicazioni Web. Tuttavia, se preferisci, è anche disponibile l’opzione per utilizzare la tua applicazione.

## Prerequisiti

Per completare questa esercitazione, è necessario:

- Ambiente as a Cloud Service AEM con [Evento AEM abilitato](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Progetto della console Adobe Developer configurato per gli eventi AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Accedere all’applicazione web

Per accedere all’applicazione web fornita dall’Adobe, effettua le seguenti operazioni:

- Verifica di poter accedere a [Glitch - applicazione web in hosting](https://indigo-speckle-antler.glitch.me/) in una nuova scheda del browser.

  ![Glitch - applicazione web in hosting](../assets/examples/journaling/glitch-hosted-web-application.png)

## Raccogli i dettagli del progetto della console Adobe Developer

Per recuperare gli eventi AEM dal diario, le credenziali come _ID organizzazione IMS_, _ID client_, e _Token di accesso_ sono obbligatori. Per raccogliere queste credenziali, effettua le seguenti operazioni:

- In [Console Adobe Developer](https://developer.adobe.com), accedi al progetto e fai clic su per aprirlo.

- Sotto **Credenziali** , fare clic sul pulsante **OAuth Server-to-Server** collegamento per aprire **Dettagli delle credenziali** scheda.

- Fai clic su **Genera token di accesso** per generare il token di accesso.

  ![Progetto della console Adobe Developer - Genera token di accesso](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Copia il **Token di accesso generato**, **ID CLIENT**, e **ID ORGANIZZAZIONE**. Ne avrai bisogno più avanti in questa esercitazione.

  ![Credenziali copia progetto della console Adobe Developer](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Ogni registrazione di evento viene automaticamente abilitata per la registrazione nel journal. Per ottenere _endpoint API di inserimento nel journal univoco_ della registrazione all’evento, fai clic sulla scheda dell’evento abbonata a Eventi AEM. Dalla sezione **Dettagli registrazione** , copia il **ENDPOINT API UNIVOCO PER INSERIMENTO NEL JOURNAL**.

  ![Scheda Eventi progetto della console Adobe Developer](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## Carica giornale di registrazione eventi AEM

Per semplificare, questa applicazione web in hosting recupera solo il primo batch di eventi AEM dal giornale. Si tratta dei più vecchi eventi disponibili nel giornale di registrazione. Per ulteriori dettagli, consulta [primo batch di eventi](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- In [Glitch - applicazione web in hosting](https://indigo-speckle-antler.glitch.me/), immetti il **ID organizzazione IMS**, **ID client**, e **Token di accesso** hai copiato in precedenza dal progetto Adobe Developer Console e fai clic su **Invia**.

- In caso di esito positivo, il componente tabella visualizza i dati del giornale di registrazione eventi AEM.

  ![Dati diario eventi AEM](../assets/examples/journaling/load-journal.png)

- Per visualizzare il payload completo dell’evento, fai doppio clic sulla riga. Puoi vedere che i dettagli dell’evento AEM contengono tutte le informazioni necessarie per elaborare l’evento nel webhook. Ad esempio, il tipo di evento (`type`), origine evento (`source`), id evento (`event_id`), ora evento (`time`), e dati evento (`data`).

  ![Payload completo evento AEM](../assets/examples/journaling/complete-journal-data.png)

## Risorse aggiuntive

- [Codice sorgente del webhook Glitch](https://glitch.com/edit/#!/indigo-speckle-antler) è disponibile come riferimento. È una semplice applicazione React che utilizza [Spettro di reazione Adobe](https://react-spectrum.adobe.com/react-spectrum/index.html) componenti per il rendering dell’interfaccia utente.

- [API di inserimento nel journal degli eventi di Adobe I/O](https://developer.adobe.com/events/docs/guides/api/journaling_api/) fornisce informazioni dettagliate sull’API, come primo, successivo e ultimo batch di eventi, impaginazione e altro ancora.
