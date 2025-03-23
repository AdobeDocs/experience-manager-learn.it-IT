---
title: Eventi di journaling e AEM
description: Scopri come recuperare il set iniziale di eventi AEM dal diario ed esplorare i dettagli di ogni evento.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 280
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
exl-id: 33eb0757-f0ed-4c2d-b8b9-fa6648e87640
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---

# Eventi di journaling e AEM

Scopri come recuperare il set iniziale di eventi AEM dal diario ed esplorare i dettagli di ogni evento.

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

Il journal è un metodo pull per utilizzare gli eventi AEM e un journal è un elenco ordinato di eventi. Utilizzando l’API di Adobe I/O Events Journaling, puoi recuperare gli eventi AEM dal diario ed elaborarli nell’applicazione. Questo approccio consente di gestire gli eventi in base a una frequenza specifica ed elaborarli in modo efficiente in blocco. Per informazioni approfondite, incluse considerazioni essenziali come periodi di conservazione, impaginazione e altro ancora, consulta la [funzione di diario](https://developer.adobe.com/events/docs/guides/journaling_intro/).

All&#39;interno del progetto Adobe Developer Console, la registrazione di ogni evento viene abilitata automaticamente per il journaling, consentendo un&#39;integrazione perfetta.

In questo esempio, l&#39;utilizzo di un&#39;_applicazione Web ospitata_ fornita da Adobe consente di recuperare il primo batch di eventi AEM dal giornale di registrazione senza dover configurare l&#39;applicazione. Questa applicazione Web fornita da Adobe è ospitata su [Glitch](https://glitch.com/), una piattaforma nota per offrire un ambiente basato su Web che favorisce la creazione e la distribuzione di applicazioni Web. Tuttavia, se preferisci, è anche disponibile l’opzione per utilizzare la tua applicazione.

## Prerequisiti

Per completare questa esercitazione, è necessario:

- Ambiente AEM as a Cloud Service con [evento AEM abilitato](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Progetto Adobe Developer Console configurato per gli eventi AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Accedere all’applicazione web

Per accedere all’applicazione web fornita da Adobe, effettua le seguenti operazioni:

- Verificare che sia possibile accedere a [Glitch - applicazione Web ospitata](https://indigo-speckle-antler.glitch.me/) in una nuova scheda del browser.

  ![Glitch - applicazione Web ospitata](../assets/examples/journaling/glitch-hosted-web-application.png)

## Raccogli dettagli progetto Adobe Developer Console

Per recuperare gli eventi AEM dal giornale di registrazione, sono necessarie credenziali quali _ID organizzazione IMS_, _ID client_ e _Token di accesso_. Per raccogliere queste credenziali, effettua le seguenti operazioni:

- In [Adobe Developer Console](https://developer.adobe.com), passa al progetto e fai clic per aprirlo.

- Nella sezione **Credenziali**, fai clic sul collegamento **OAuth Server-to-Server** per aprire la scheda **Dettagli credenziali**.

- Fai clic sul pulsante **Genera token di accesso** per generare il token di accesso.

  ![Progetto Adobe Developer Console - Genera token di accesso](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Copia il **token di accesso generato**, **ID CLIENT** e **ID ORGANIZZAZIONE**. Ne avrai bisogno più avanti in questa esercitazione.

  ![Credenziali copia progetto Adobe Developer Console](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Ogni registrazione di evento viene automaticamente abilitata per la registrazione nel journal. Per ottenere l&#39;_endpoint API di inserimento nel journal univoco_ della registrazione dell&#39;evento, fare clic sulla scheda evento sottoscritta agli eventi di AEM. Dalla scheda **Dettagli registrazione**, copia l&#39;ENDPOINT API UNIVOCO PER IL JOURNAL ****.

  ![Scheda Eventi progetto Adobe Developer Console](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## Carica giornale di registrazione eventi di AEM

Per semplificare, questa applicazione Web in hosting recupera solo il primo batch di eventi AEM dal giornale di registrazione. Si tratta dei più vecchi eventi disponibili nel giornale di registrazione. Per ulteriori dettagli, vedere [primo batch di eventi](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- Nell&#39;applicazione Web [Glitch - ospitato](https://indigo-speckle-antler.glitch.me/), immettere l&#39;**ID organizzazione IMS**, l&#39;**ID client** e il **Token di accesso** copiati in precedenza dal progetto Adobe Developer Console e fare clic su **Invia**.

- In caso di esito positivo, il componente tabella visualizza i dati del giornale di registrazione eventi di AEM.

  ![Dati diario eventi AEM](../assets/examples/journaling/load-journal.png)

- Per visualizzare il payload completo dell’evento, fai doppio clic sulla riga. Puoi vedere che i dettagli dell’evento AEM contengono tutte le informazioni necessarie per elaborare l’evento nel webhook. Ad esempio, il tipo di evento (`type`), l&#39;origine evento (`source`), l&#39;ID evento (`event_id`), l&#39;ora evento (`time`) e i dati evento (`data`).

  ![Payload evento AEM completo](../assets/examples/journaling/complete-journal-data.png)

## Risorse aggiuntive

- [Il codice sorgente del webhook Glitch](https://glitch.com/edit/#!/indigo-speckle-antler) è disponibile come riferimento. Si tratta di una semplice applicazione React che utilizza [componenti Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) per eseguire il rendering dell&#39;interfaccia utente.

- [API di Adobe I/O Events Journaling](https://developer.adobe.com/events/docs/guides/api/journaling_api/) fornisce informazioni dettagliate sull&#39;API, ad esempio il primo, il successivo e l&#39;ultimo batch di eventi, la paginazione e altro ancora.
