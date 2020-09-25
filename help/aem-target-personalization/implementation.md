---
title: Integrazione di Adobe Experience Manager con  Adobe Target
seo-title: Un articolo che descrive diversi modi per integrare Adobe Experience Manager(AEM) con  Adobe Target per la distribuzione di contenuti personalizzati.
description: Un articolo che illustra come impostare Adobe Experience Manager con  Adobe Target per diversi scenari.
seo-description: Un articolo che illustra come impostare Adobe Experience Manager con  Adobe Target per diversi scenari.
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 4%

---


# Integrazione di Adobe Experience Manager con  Adobe Target

In questa sezione verrà illustrato come impostare Adobe Experience Manager con  Adobe Target per diversi scenari. In base allo scenario e ai requisiti organizzativi.

* **Aggiungi  libreria Adobe Target JavaScript (richiesta per tutti gli scenari)** Per i siti ospitati in AEM, puoi aggiungere librerie Target al tuo sito utilizzando [Launch](https://docs.adobe.com/content/help/en/launch/using/overview.html). Launch fornisce un modo semplice per distribuire e gestire tutti i tag necessari per fornire esperienze cliente rilevanti.
* **Aggiungete i Cloud Services Adobe Target  (necessari per lo scenario dei frammenti esperienza)** Per AEM clienti che desiderano utilizzare le offerte relative ai frammenti esperienza per creare un&#39;attività  Adobe Target, dovrete integrare  Adobe Target con AEM utilizzando i Cloud Services legacy. Questa integrazione è necessaria per inviare i frammenti esperienza da AEM a Target come offerte HTML/JSON e per mantenere le offerte sincronizzate con AEM. 
*Questa integrazione è necessaria per l&#39;implementazione dello scenario 1.*

## Prerequisiti

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (consigliato *Service Pack* più recente)
   * Download AEM pacchetti del sito di riferimento WKND
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.co-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componenti core](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Livello dati digitale](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Accesso alle organizzazioni Adobe Experience Cloud - <https://>`<yourcompany>`.experienceecCloud.adobe.com
   *  Experience Cloud fornito con le seguenti soluzioni
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console I/O Adobe](https://console.adobe.io)

* **di authoring**
   * Java 1.8 o Java 11 (solo AEM 6.5+)
   * Apache Maven (3.3.9 o successivo)
   * Effetto cromatura

>[!NOTE]
>
> È necessario effettuare il provisioning del cliente con l&#39;I/O di Experience Platform Launch e  Adobe dal supporto [di](https://helpx.adobe.com/it/contact/enterprise-support.ec.html) Adobe o contattare l&#39;amministratore di sistema

### Imposta AEM{#set-up-aem}

AEM’istanza di creazione e pubblicazione è necessaria per completare questa esercitazione. L’istanza di creazione è in esecuzione su `http://localhost:4502` e l’istanza di pubblicazione è in esecuzione su `http://localhost:4503`. Per ulteriori informazioni, vedi: [Configurare un ambiente](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)di sviluppo AEM locale.

#### Configurare le istanze di AEM Author e Publish

1. Ricevi una copia del [AEM QuickStart Jar e una licenza.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Create una struttura di cartelle sul computer come segue:
   ![Struttura delle cartelle](assets/implementation/aem-setup-1.png)
3. Rinominare l&#39;area di avvio rapido in `aem-author-p4502.jar` e posizionarla sotto la `/author` directory. Aggiungete il `license.properties` file sotto la `/author` directory.
   ![Istanza Author AEM](assets/implementation/aem-setup-author.png)
4. Create una copia del file JAR di Quickstart, rinominatelo in `aem-publish-p4503.jar` e inseritelo sotto la `/publish` directory. Aggiungete una copia del `license.properties` file sotto la `/publish` directory.
   ![AEM Publish Instance](assets/implementation/aem-setup-publish.png)
5. Fate doppio clic sul `aem-author-p4502.jar` file per installare l&#39;istanza Author. Viene avviata l’istanza di creazione, che viene eseguita sulla porta 4502 del computer locale.
6. Effettuate l&#39;accesso con le credenziali riportate di seguito e, dopo aver effettuato l&#39;accesso, verrete indirizzati alla schermata AEM pagina iniziale.
username : **admin** password: **admin**
   ![AEM Publish Instance](assets/implementation/aem-author-home-page.png)
7. Fate doppio clic sul `aem-publish-p4503.jar` file per installare un’istanza di pubblicazione. Potete notare una nuova scheda che si apre nel browser per l’istanza di pubblicazione, che viene eseguita sulla porta 4503 e viene visualizzata la home page di WeRetail. Utilizzeremo il sito di riferimento WKND per questa esercitazione e installeremo i pacchetti nell&#39;istanza di creazione.
8. Andate su AEM Author nel browser Web all&#39;indirizzo `http://localhost:4502`. Nella schermata iniziale AEM, andate a *[Strumenti > Distribuzione > Pacchetti](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Scaricate e caricate i pacchetti per AEM (elencati sopra in *[Prerequisiti > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.co-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Dopo aver installato i pacchetti in AEM Author, selezionate ogni pacchetto caricato in AEM Package Manager, quindi selezionate **Altro > Replica** per assicurarvi che i pacchetti siano distribuiti in AEM Publish.
11. A questo punto, avete installato con successo il sito di riferimento WKND e tutti i pacchetti aggiuntivi richiesti per questa esercitazione.

[CAPITOLO](./using-launch-adobe-io.md)SUCCESSIVO: Nel capitolo successivo, verrà integrato Launch con AEM.
