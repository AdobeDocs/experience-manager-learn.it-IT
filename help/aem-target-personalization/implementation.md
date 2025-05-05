---
title: Integrazione di AEM Sites con Adobe Target
description: Un articolo che illustra come configurare Adobe Experience Manager con Adobe Target per diversi scenari.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# Integrazione di AEM Sites con Adobe Target

In questa sezione verrà illustrato come configurare Adobe Experience Manager Sites con Adobe Target per diversi scenari. In base allo scenario e ai requisiti organizzativi.

* **Aggiungi libreria JavaScript di Adobe Target (richiesta per tutti gli scenari)**
Per i siti ospitati su AEM, puoi aggiungere librerie Target al tuo sito utilizzando, [tag in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=it). I tag offrono un modo semplice di implementare e gestire tutti i tag necessari per fornire ai clienti esperienze personalizzate significative.
* **Aggiungi i Cloud Service Adobe Target (necessari per lo scenario Frammenti esperienza)**
Per i clienti AEM che desiderano utilizzare le offerte dei frammenti di esperienza per creare un’attività in Adobe Target, dovranno integrare Adobe Target con AEM utilizzando i Cloud Service legacy. Questa integrazione è necessaria per inviare i frammenti di esperienza dall’AEM a Target come offerte HTML/JSON e per mantenere la sincronizzazione con l’AEM. *Questa integrazione è necessaria per implementare lo scenario 1.*

## Prerequisiti

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*si consiglia l&#39;ultimo Service Pack*)
   * Scarica i pacchetti del sito di riferimento WKND dell’AEM
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componenti di base](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Livello dati digitale](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Accesso al Adobe Experience Cloud delle organizzazioni - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fornito con le seguenti soluzioni
      * [Raccolta dati](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console Adobi I/O](https://console.adobe.io)

* **Ambiente**
   * Java 1.8 o Java 11 (solo AEM 6.5+)
   * Apache Maven (3.3.9 o successivo)
   * Chrome

>[!NOTE]
>
> Al cliente devono essere forniti la raccolta dati e l&#39;Adobe I/O da [Supporto Adobe](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) o contattare l&#39;amministratore di sistema

### Configurare AEM{#set-up-aem}

Per completare questa esercitazione, è necessario che l’istanza di authoring e pubblicazione di AEM sia completa. L&#39;istanza di authoring è in esecuzione su `http://localhost:4502` e l&#39;istanza di pubblicazione su `http://localhost:4503`. Per ulteriori informazioni, vedere: [Configurare un ambiente di sviluppo AEM locale](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Configurare le istanze di AEM Author e Publish

1. Ottieni una copia del file JAR Quickstart di [AEM e una licenza.](https://helpx.adobe.com/it/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Crea nel computer una struttura di cartelle come quella riportata di seguito:
   ![Struttura cartella](assets/implementation/aem-setup-1.png)
3. Rinomina il file jar Quickstart in `aem-author-p4502.jar` e inseriscilo sotto la directory `/author`. Aggiungere il file `license.properties` sotto la directory `/author`.
   ![Istanza Autore AEM](assets/implementation/aem-setup-author.png)
4. Creare una copia del file jar Quickstart, rinominarlo in `aem-publish-p4503.jar` e posizionarlo sotto la directory `/publish`. Aggiungere una copia del file `license.properties` sotto la directory `/publish`.
   ![Istanza Publish AEM](assets/implementation/aem-setup-publish.png)
5. Fare doppio clic sul file `aem-author-p4502.jar` per installare l&#39;istanza Autore. Verrà avviata l&#39;istanza di authoring, in esecuzione sulla porta 4502 del computer locale.
6. Effettuate l&#39;accesso utilizzando le credenziali riportate di seguito e, una volta effettuato l&#39;accesso, verrete indirizzati alla schermata della home page del AEM.
nome utente: **admin**
password: **admin**
   ![Istanza Publish AEM](assets/implementation/aem-author-home-page.png)
7. Fare doppio clic sul file `aem-publish-p4503.jar` per installare un&#39;istanza Publish. Puoi notare una nuova scheda che si apre nel browser per l’istanza Publish, in esecuzione sulla porta 4503 e visualizzando la home page di WeRetail. Per questa esercitazione utilizziamo il sito di riferimento WKND e installiamo i pacchetti sull’istanza di authoring.
8. Passa a AEM Author nel browser Web all&#39;indirizzo `http://localhost:4502`. Nella schermata iniziale dell&#39;AEM, passare a *[Strumenti > Distribuzione > Pacchetti](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Scarica e carica i pacchetti per AEM (elencati sopra in *[Prerequisiti > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Dopo aver installato i pacchetti in AEM Author, selezionare ogni pacchetto caricato in AEM Package Manager e selezionare **Altro > Replica** per assicurarsi che i pacchetti vengano distribuiti in AEM Publish.
11. A questo punto, hai installato correttamente il sito di riferimento WKND e tutti i pacchetti aggiuntivi necessari per questa esercitazione.

[CAPITOLO SUCCESSIVO](./using-launch-adobe-io.md): nel prossimo capitolo, integrerai i tag con AEM.
