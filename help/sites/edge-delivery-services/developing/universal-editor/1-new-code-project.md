---
title: Creare un progetto di codice
description: Crea un progetto di codice per Edge Delivery Services, modificabile tramite l’Editor universale.
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
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# Creazione di un progetto di codice Edge Delivery Services

Per creare siti Web AEM per Edge Delivery Services e Universal Editor, utilizzare il [modello di progetto AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk) di Adobe. Questo modello crea un nuovo progetto di codice che contiene i file CSS e JavaScript utilizzati per creare l’esperienza del sito web. Questo modello crea un nuovo archivio GitHub e lo carica con il codice e la configurazione standard di Adobe, fornendo una solida base per il progetto del sito web AEM.

Ricorda che [i siti Web AEM consegnati da Edge Delivery Services](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview) hanno solo codice lato client (browser). Il codice del sito web non viene eseguito nei servizi Author o Publish di AEM.

![Nuovo progetto Edge Delivery Services](./assets/1-new-project/new-project.png)

Segui i [passaggi dettagliati descritti nella documentazione](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) per creare un progetto di codice Edge Delivery Services il cui contenuto è modificabile in Universal Editor.  Di seguito è riportato un elenco riepilogativo dei passaggi, inclusi i valori utilizzati in questa esercitazione.

1. **Configura un account GitHub.** Se stai creando un progetto per la tua organizzazione, assicurati che questa disponga di un account GitHub e di essere membro.
2. **Crea un nuovo progetto di codice** utilizzando il [modello di progetto XWalk standard di AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Installa l&#39;app GitHub AEM Code Sync** e concedi l&#39;accesso all&#39;archivio. Puoi trovare l&#39;[app qui](https://github.com/apps/aem-code-sync).
4. **Configura`fstab.yaml`** del nuovo progetto in modo che punti al servizio AEM Author corretto.

   * Per fare delle prove, puoi utilizzare ambienti AEM as a Cloud Service inferiori (Stage o Dev), tuttavia le implementazioni di siti web reali devono essere configurate per utilizzare un servizio di produzione AEM.

5. **Modifica il`paths.json`** del nuovo progetto per mappare il percorso del servizio AEM Author alla directory principale del tuo sito Web.

Questo archivio Git viene clonato nel capitolo [Local Development Environment](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) e dove viene sviluppato il codice.
