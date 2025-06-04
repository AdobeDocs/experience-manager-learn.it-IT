---
title: Creare un progetto di codice
description: Crea un progetto di codice per Edge Delivery Services, modificabile tramite l’editor universale.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '287'
ht-degree: 100%

---

# Creazione di un progetto di codice di Edge Delivery Services

Per creare siti web AEM per Edge Delivery Services e per l’editor universale, utilizza il [modello standard di progetto XWalk AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk) di Adobe. Questo modello crea un nuovo progetto di codice che contiene i CSS e JavaScript utilizzati per creare l’esperienza del sito web. Questo modello crea un nuovo archivio GitHub e lo carica con il codice e la configurazione standard di Adobe, fornendo una solida base per il progetto del sito web AEM.

Ricorda che [i siti web AEM distribuiti da Edge Delivery Services](https://experienceleague.adobe.com/it/docs/experience-manager-learn/sites/edge-delivery-services/overview) hanno solo il codice lato client (browser). Il codice del sito web non viene eseguito nei servizi di authoring o di pubblicazione di AEM.

![Nuovo progetto di Edge Delivery Services](./assets/1-new-project/new-project.png)

Segui i [passaggi dettagliati descritti nella documentazione](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) per creare un progetto di codice di Edge Delivery Services il cui contenuto è modificabile nell’editor universale.  Di seguito è riportato un elenco riepilogativo dei passaggi, inclusi i valori utilizzati in questo tutorial.

1. **Configurare un account GitHub.** Se stai creando un progetto per la tua organizzazione, assicurati che questa disponga di un account GitHub e di esserne membro.
2. **Crea un nuovo progetto di codice** utilizzando il [modello di progetto standard XWalk AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Installa l’app AEM Code Sync di GitHub** e concedi l’accesso all’archivio. Puoi trovare l’[app qui](https://github.com/apps/aem-code-sync).
4. **Configura`fstab.yaml`** del nuovo progetto in modo che punti al servizio AEM Author corretto.

   * Per sperimentare, puoi utilizzare ambienti AEM as a Cloud Service inferiori (staging o sviluppo), tuttavia le implementazioni di siti web reali devono essere configurate per utilizzare un servizio di produzione AEM.

5. **Modifica il`paths.json`** del nuovo progetto per mappare il percorso del servizio AEM Author sulla directory principale del tuo sito web.

Questo archivio Git viene clonato nel capitolo [ambiente di sviluppo locale](https://experienceleague.adobe.com/it/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) e dove viene sviluppato il codice.
