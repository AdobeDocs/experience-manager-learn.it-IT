---
title: Integrazione di Adobe Experience Manager con Adobe Target
seo-title: An article covering different ways to integrate Adobe Experience Manager(AEM) with Adobe Target for delivering personalized content.
description: Un articolo che illustra come configurare Adobe Experience Manager con Adobe Target per diversi scenari.
seo-description: An article covering how to set up Adobe Experience Manager with Adobe Target for different scenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 5%

---

# Integrazione di Adobe Experience Manager con Adobe Target

In questa sezione discuteremo come configurare Adobe Experience Manager con Adobe Target per scenari diversi. In base allo scenario e ai requisiti organizzativi.

* **Aggiungi libreria JavaScript di Adobe Target (richiesta per tutti gli scenari)**
Per i siti ospitati su AEM, puoi aggiungere librerie Target al tuo sito utilizzando: [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Launch offre un modo semplice di implementare e gestire tutti i tag necessari per fornire ai clienti esperienze personalizzate.
* **Aggiungi gli Cloud Services Adobe Target (necessari per lo scenario Frammenti esperienza)**
Per i clienti AEM che desiderano utilizzare le offerte Frammento esperienza per creare un’attività in Adobe Target, è necessario integrare Adobe Target con AEM utilizzando i Cloud Services legacy. Questa integrazione è necessaria per inviare i frammenti esperienza da AEM a Target come offerte HTML/JSON e per mantenere le offerte sincronizzate con AEM. 
*Questa integrazione è necessaria per implementare lo scenario 1.*

## Prerequisiti

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*si consiglia l&#39;ultimo Service Pack*)
   * Scaricare AEM pacchetti del sito di riferimento WKND
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componenti core](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Livello dati digitale](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console Adobe I/O](https://console.adobe.io)

* **di authoring**
   * Java 1.8 o Java 11 (solo AEM 6.5+)
   * Apache Maven (3.3.9 o successivo)
   * Chrome

>[!NOTE]
>
> Il cliente deve essere fornito con Experience Platform Launch ed Adobe I/O da [Supporto Adobe](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) o rivolgiti all’amministratore di sistema

### Imposta AEM{#set-up-aem}

Per completare questa esercitazione è necessario AEM’istanza di authoring e pubblicazione. L’istanza dell’autore è in esecuzione `http://localhost:4502` e pubblica l&#39;istanza in esecuzione su `http://localhost:4503`. Per ulteriori informazioni, consulta: [Configurare un ambiente di sviluppo AEM locale](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Configurare le istanze di authoring e pubblicazione di AEM

1. Ottieni una copia del [AEM Quickstart Jar e una licenza.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Crea una struttura di cartelle sul computer come segue:
   ![Struttura delle cartelle](assets/implementation/aem-setup-1.png)
3. Rinomina il jar Quickstart in `aem-author-p4502.jar` e posizionarlo sotto il `/author` directory. Aggiungi il `license.properties` sotto il `/author` directory.
   ![Istanza di authoring di AEM](assets/implementation/aem-setup-author.png)
4. Crea una copia del jar Quickstart, rinominalo in `aem-publish-p4503.jar` e posizionarlo sotto il `/publish` directory. Aggiungi una copia del `license.properties` sotto il `/publish` directory.
   ![AEM Publish Instance](assets/implementation/aem-setup-publish.png)
5. Fai doppio clic sul pulsante `aem-author-p4502.jar` per installare l&#39;istanza Author. Verrà avviata l&#39;istanza di authoring, in esecuzione sulla porta 4502 del computer locale.
6. Accedi utilizzando le credenziali riportate di seguito e dopo l&#39;accesso, vieni indirizzato alla schermata Home page AEM.
username : **admin**
password : **admin**
   ![AEM Publish Instance](assets/implementation/aem-author-home-page.png)
7. Fai doppio clic sul pulsante `aem-publish-p4503.jar` per installare un&#39;istanza di pubblicazione. Puoi notare una nuova scheda aperta nel browser per l’istanza di pubblicazione, che viene eseguita sulla porta 4503 e visualizza la home page di WeRetail. Stiamo utilizzando il sito di riferimento WKND per questa esercitazione e installiamo i pacchetti sull&#39;istanza dell&#39;autore.
8. Passa ad AEM Author nel browser web all’indirizzo `http://localhost:4502`. Nella schermata iniziale AEM, passa a *[Strumenti > Implementazione > Pacchetti](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Scarica e carica i pacchetti per AEM (elencati sopra in *[Prerequisiti > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Dopo aver installato i pacchetti su AEM Author, seleziona ogni pacchetto caricato in AEM Package Manager e seleziona **Altro > Replica** per garantire che i pacchetti siano distribuiti su AEM Publish.
11. A questo punto, hai installato correttamente il sito di riferimento WKND e tutti i pacchetti aggiuntivi richiesti per questa esercitazione.

[CAPITOLO SUCCESSIVO](./using-launch-adobe-io.md): Nel capitolo successivo, integrerai Launch con AEM.
