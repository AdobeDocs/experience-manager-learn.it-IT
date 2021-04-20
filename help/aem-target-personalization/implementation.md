---
title: Integrazione di Adobe Experience Manager con Adobe Target
seo-title: Un articolo che descrive diversi modi per integrare Adobe Experience Manager (AEM) con Adobe Target per la distribuzione di contenuti personalizzati.
description: Un articolo che illustra come configurare Adobe Experience Manager con Adobe Target per diversi scenari.
seo-description: Un articolo che illustra come configurare Adobe Experience Manager con Adobe Target per diversi scenari.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 5%

---


# Integrazione di Adobe Experience Manager con Adobe Target

In questa sezione discuteremo come configurare Adobe Experience Manager con Adobe Target per diversi scenari. In base allo scenario e ai requisiti organizzativi.

* **Aggiungi la libreria JavaScript di Adobe Target (richiesta per tutti gli scenari)**
Per i siti in hosting su AEM, puoi aggiungere le librerie di Target al tuo sito utilizzando  [Launch](https://docs.adobe.com/content/help/en/launch/using/overview.html). Launch offre un modo semplice di implementare e gestire tutti i tag necessari per fornire ai clienti esperienze personalizzate.
* **Aggiungi i Cloud Services di Adobe Target (necessari per lo scenario Frammenti esperienza)**
Per i clienti AEM che desiderano utilizzare le offerte di Frammenti esperienza per creare un’attività all’interno di Adobe Target, è necessario integrare Adobe Target con AEM utilizzando i servizi cloud precedenti. Questa integrazione è necessaria per inviare frammenti di esperienza da AEM a Target come offerte HTML/JSON e per mantenere le offerte sincronizzate con AEM. 
*Questa integrazione è necessaria per implementare lo scenario 1.*

## Prerequisiti

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*è consigliato l&#39;ultimo Service Pack*)
   * Scarica i pacchetti del sito di riferimento WKND AEM
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componenti core](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Livello dati digitale](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud con le seguenti soluzioni
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console Adobe I/O](https://console.adobe.io)

* **di authoring**
   * Java 1.8 o Java 11 (solo AEM 6.5+)
   * Apache Maven (3.3.9 o successivo)
   * Chrome

>[!NOTE]
>
> È necessario effettuare il provisioning dei clienti con Experience Platform Launch e Adobe I/O da [Supporto Adobe](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) oppure contattare l’amministratore di sistema

### Imposta AEM{#set-up-aem}

Per completare questa esercitazione, è necessaria l’istanza di authoring e pubblicazione di AEM. L’istanza dell’autore è in esecuzione su `http://localhost:4502` e l’istanza di pubblicazione è in esecuzione su `http://localhost:4503`. Per ulteriori informazioni, consulta: [Imposta un ambiente di sviluppo AEM locale](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Configurare le istanze di authoring e pubblicazione di AEM

1. Ottieni una copia del [Jar AEM Quickstart e una licenza.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Crea una struttura di cartelle sul computer come segue:
   ![Struttura delle cartelle](assets/implementation/aem-setup-1.png)
3. Rinomina il file jar Quickstart in `aem-author-p4502.jar` e posizionalo sotto la directory `/author`. Aggiungi il file `license.properties` sotto la directory `/author`.
   ![Istanza di authoring di AEM](assets/implementation/aem-setup-author.png)
4. Crea una copia del jar Quickstart, rinominalo in `aem-publish-p4503.jar` e posizionalo sotto la directory `/publish`. Aggiungi una copia del file `license.properties` sotto la directory `/publish`.
   ![AEM Publish Instance](assets/implementation/aem-setup-publish.png)
5. Fai doppio clic sul file `aem-author-p4502.jar` per installare l’istanza Author. Verrà avviata l&#39;istanza di authoring, in esecuzione sulla porta 4502 del computer locale.
6. Accedi utilizzando le credenziali riportate di seguito e dopo l’accesso, verrai indirizzato alla schermata iniziale di AEM.
username : **admin**
password : **admin**
   ![AEM Publish Instance](assets/implementation/aem-author-home-page.png)
7. Fai doppio clic sul file `aem-publish-p4503.jar` per installare un’istanza di pubblicazione. Puoi notare una nuova scheda aperta nel browser per l’istanza di pubblicazione, che viene eseguita sulla porta 4503 e visualizza la home page di WeRetail. Per questa esercitazione utilizzeremo il sito di riferimento WKND e installeremo i pacchetti nell’istanza di authoring.
8. Passa ad AEM Author nel browser web all’indirizzo `http://localhost:4502`. Nella schermata iniziale di AEM, passa a *[Strumenti > Implementazione > Pacchetti](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Scarica e carica i pacchetti per AEM (elencati sopra in *[Prerequisiti > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Dopo aver installato i pacchetti su AEM Author, seleziona ogni pacchetto caricato in AEM Package Manager e seleziona **Altro > Replicare** per assicurarti che i pacchetti siano distribuiti su AEM Publish.
11. A questo punto, hai installato correttamente il sito di riferimento WKND e tutti i pacchetti aggiuntivi richiesti per questa esercitazione.

[CAPITOLO](./using-launch-adobe-io.md) SUCCESSIVO: Nel capitolo successivo, integrerai Launch con AEM.
