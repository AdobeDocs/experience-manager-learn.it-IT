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
duration: 173
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 1%

---

# Integrazione di AEM Sites con Adobe Target

In questa sezione verrà illustrato come configurare Adobe Experience Manager Sites con Adobe Target per diversi scenari. In base allo scenario e ai requisiti organizzativi.

* **Aggiungi libreria JavaScript di Adobe Target (richiesta per tutti gli scenari)**
Per i siti in hosting su AEM, puoi aggiungere librerie Target al sito utilizzando, [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Launch offre un modo semplice di implementare e gestire tutti i tag necessari per fornire ai clienti esperienze personalizzate.
* **Aggiungi i Cloud Service Adobe Target (obbligatorio per lo scenario Frammenti esperienza)**
Per i clienti AEM che desiderano utilizzare le offerte dei frammenti di esperienza per creare un’attività in Adobe Target, dovranno integrare Adobe Target con AEM utilizzando i Cloud Service legacy. Questa integrazione è necessaria per inviare i frammenti di esperienza dall’AEM a Target come offerte HTML/JSON e per mantenere la sincronizzazione con l’AEM. *Questa integrazione è necessaria per attuare lo scenario 1.*

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
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console Adobi I/O](https://console.adobe.io)

* **Ambiente**
   * Java 1.8 o Java 11 (solo AEM 6.5+)
   * Apache Maven (3.3.9 o successivo)
   * Chrome

>[!NOTE]
>
> È necessario fornire al cliente Experience Platform Launch e Adobe I/O da [Supporto Adobe](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) o contattare l&#39;amministratore di sistema

### Configurare AEM{#set-up-aem}

Per completare questa esercitazione, è necessario che l’istanza di authoring e pubblicazione di AEM sia completa. L’istanza di authoring è in esecuzione su `http://localhost:4502` e l’istanza di pubblicazione è in esecuzione su `http://localhost:4503`. Per ulteriori informazioni, consulta: [Configurare un ambiente locale di sviluppo AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Configurare le istanze di authoring e pubblicazione AEM

1. Ottieni una copia di [JAR Quickstart per AEM e una licenza.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Crea nel computer una struttura di cartelle come quella riportata di seguito:
   ![Struttura delle cartelle](assets/implementation/aem-setup-1.png)
3. Rinomina il file jar Quickstart in `aem-author-p4502.jar` e posizionalo sotto il `/author` directory. Aggiungi il `license.properties` file sotto `/author` directory.
   ![Istanza autore AEM](assets/implementation/aem-setup-author.png)
4. Crea una copia del file jar Quickstart e rinominalo in `aem-publish-p4503.jar` e posizionalo sotto il `/publish` directory. Aggiungi una copia del `license.properties` file sotto `/publish` directory.
   ![Istanza pubblicazione AEM](assets/implementation/aem-setup-publish.png)
5. Fai doppio clic su `aem-author-p4502.jar` per installare l’istanza di authoring. Verrà avviata l&#39;istanza di authoring, in esecuzione sulla porta 4502 del computer locale.
6. Effettuate l&#39;accesso utilizzando le credenziali riportate di seguito e, una volta effettuato l&#39;accesso, verrete indirizzati alla schermata della home page del AEM.
nome utente : **admin**
password: **admin**
   ![Istanza pubblicazione AEM](assets/implementation/aem-author-home-page.png)
7. Fai doppio clic su `aem-publish-p4503.jar` per installare un’istanza Publish. Puoi notare una nuova scheda che si apre nel browser per l’istanza Publish, in esecuzione sulla porta 4503 e visualizzando la home page di WeRetail. Per questa esercitazione utilizziamo il sito di riferimento WKND e installiamo i pacchetti sull’istanza di authoring.
8. Vai a AEM Author nel browser Web all’indirizzo `http://localhost:4502`. Nella schermata iniziale dell’AEM, passa a *[Strumenti > Implementazione > Pacchetti](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Scaricare e caricare i pacchetti per AEM (elencati sopra in *[Prerequisiti > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Dopo aver installato i pacchetti in AEM Author, seleziona ogni pacchetto caricato in AEM Package Manager e fai clic su **Altro > Replica** per garantire che i pacchetti vengano distribuiti in Pubblicazione AEM.
11. A questo punto, hai installato correttamente il sito di riferimento WKND e tutti i pacchetti aggiuntivi necessari per questa esercitazione.

[CAPITOLO SUCCESSIVO](./using-launch-adobe-io.md): nel prossimo capitolo, integrerai Launch con AEM.
