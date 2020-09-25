---
title: Utilizzo di  risorse Adobe Stock con  AEM Assets
description: 'AEM offre agli utenti la possibilità di effettuare ricerche, visualizzare in anteprima, salvare e concedere in licenza  risorse Adobe Stock direttamente da AEM. Adesso, le organizzazioni possono integrare  piano Adobe Stock Enterprise con  AEM Assets per garantire che le risorse su licenza siano ora ampiamente disponibili per i loro progetti creativi e di marketing, con le potenti funzionalità di gestione delle risorse di AEM. '
feature: creative-cloud-integration
topics: authoring, collaboration, operations, sharing, metadata, images, stock
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 6%

---


# Utilizzo  Adobe Stock con  AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 consente agli utenti di effettuare ricerche, visualizzare in anteprima, salvare e concedere in licenza  risorse Adobe Stock direttamente da AEM. Adesso, le organizzazioni possono integrare  piano Adobe Stock Enterprise con  AEM Assets per garantire che le risorse su licenza siano ora ampiamente disponibili per i loro progetti creativi e di marketing, con le potenti funzionalità di gestione delle risorse di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>L&#39;integrazione richiede un piano [Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) Enterprise  e AEM 6.4 con almeno Service Pack 2 implementato. Per AEM informazioni sul service pack 6.4, consultate queste note [sulla](https://helpx.adobe.com/it/experience-manager/6-4/release-notes/sp-release-notes.html)versione.

&#39;integrazione di Adobe Stock e  AEM Assets consente agli autori e agli esperti di marketing dei contenuti di ottenere facilmente la licenza e di utilizzare le risorse stock a scopi creativi o di marketing. Potete eseguire una ricerca di risorse Stock utilizzando Omni Search, aggiungendo il filtro posizione come  Adobe Stock o navigando  navigazione principale di AEM Assets e facendo clic sull’icona dell’interfaccia utente  Adobe Stock Coral.

## Funzionalità

### Ricerca e salvataggio

* Effettuate  ricerca delle risorse Adobe Stock senza uscire AEM&#39;area di lavoro.
* Salvate  risorse Adobe Stock per l’anteprima, senza concedere la licenza per la risorsa.
* Possibilità di concedere in licenza e salvare  risorse Adobe Stock in  AEM Assets
* Possibilità di cercare risorse simili da  Adobe Stock all’interno  interfaccia utente di AEM Assets
* Visualizzare una risorsa selezionata da Stock Search in  AEM Assets  sito Web Adobe Stock
* I file di risorse con licenza sono contrassegnati con un badge blu per facilitarne l’identificazione

### Metadati risorsa

* La risorsa con licenza viene memorizzata in  AEM Assets. Le proprietà delle risorse contengono i metadati Stock in una scheda di metadati di una risorsa separata
* Possibilità di aggiungere riferimenti di licenza ai metadati delle risorse

### Profilo risorsa

* Un utente può selezionare  profilo Adobe Stock in *Utente > Preferenze personali > Configurazione Stock*
* È possibile aggiungere riferimenti obbligatori e facoltativi alla finestra Licenze risorse.
* Possibilità di scegliere la lingua preferita per la finestra Licenze risorse in base alla regione.

### Filtro

* Un utente può filtrare le risorse stock in base a Tipo di risorsa, Orientamento e Vista simili
* Il tipo di risorsa include Foto, Illustrazioni, Vettori, Video, Modelli, 3D, Premium, Editorial
* L&#39;orientamento include orizzontale, verticale e quadrato.
* Visualizza filtro simile richiede  numero file Adobe Stock

### Controllo accesso

* Gli amministratori possono concedere autorizzazioni a determinati utenti/gruppi per concedere in licenza le risorse stock al momento della configurazione  servizio cloud Adobe Stock.
* Se un utente/gruppo specifico non dispone dell’autorizzazione per la licenza delle risorse stock, la funzionalità di ricerca delle risorse *Stock / licenza* delle risorse risulterebbe disabilitata.

## Configurare  Adobe Stock con  AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 consente agli utenti di effettuare ricerche, visualizzare in anteprima, salvare e concedere in licenza  risorse Adobe Stock direttamente da AEM. Questo video illustra come impostare  stock di Adobe con  AEM Assets utilizzando  console I/O Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Per  configurazione del servizio Adobe Stock Cloud, è necessario selezionare il percorso della risorsa Ambiente di PROD143 e Licenza su /content/dam. Il campo Ambiente verrà rimosso nella prossima release AEM e il percorso della risorsa su licenza fa parte di una funzionalità in arrivo e il supporto per questo campo verrà introdotto nella prossima release AEM.

>[!NOTE]
>
>L&#39;integrazione richiede un piano [Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) Enterprise  e AEM 6.4 con almeno [Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) implementato. Per AEM informazioni sul service pack 6.4, consultate queste note [sulla](https://helpx.adobe.com/it/experience-manager/6-4/release-notes/sp-release-notes.html)versione. Per configurare l&#39;integrazione, è inoltre necessario disporre delle autorizzazioni di amministratore per [Console](https://console.adobe.io/)I/O di Adobe, per [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager.

### Installazione {#installations}

* Per AEM 6.4, è necessario installare [AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) , quindi reinstallare il file cq-dam-stock-integration-content-1.0.4.zip.
* Assicuratevi di disporre delle autorizzazioni di amministratore sulla console [I/O](https://console.adobe.io/)Adobe, su [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager per configurare l&#39;integrazione.

#### Configurazione  Adobe IMS mediante  console I/O Adobe {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Crea un account tecnico IMS  Adobe in **Strumenti > Protezione**
2. Selezionate la soluzione ** Cloud come *Adobe Stock* e create un nuovo certificato oppure riutilizzate un certificato esistente per la configurazione.
3. Andate  console I/O del Adobe e create una nuova integrazione dell&#39;account di servizio per *Adobe Stock*.
4. Caricate il certificato dal passaggio2 alla vostra integrazione con l&#39;account di servizio Adobe Stock .
5. Scegliete la configurazione di profilo Adobe Stock  richiesta e completate l&#39;integrazione con il servizio.
6. Utilizzate i dettagli di integrazione per completare la configurazione dell&#39;account tecnico IMS del Adobe 
7. Assicuratevi di poter ricevere il token di accesso utilizzando l&#39;account tecnico IMS del Adobe .

![Account tecnico Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurare  Cloud Services Adobe Stock {#set-up-adobe-stock-cloud-services}

1. Crea una nuova configurazione del servizio cloud per  Adobe Stock in **Strumenti > Cloud Services.**
2. Seleziona l&#39; *Adobe IMS Configuration* creato nella sezione precedente per la configurazione *Adobe Stock Cloud*

3. Accertatevi di selezionare **AMBIENTE** come PROD. L&#39;ambiente di gestione temporanea non è supportato e verrà rimosso nella prossima release di AEM.
4. **Il percorso** risorse concesso in licenza può essere indicato in qualsiasi directory in /content/dam. Il supporto delle funzioni per questo campo verrà aggiunto nella prossima release di AEM
5. Selezionate le impostazioni internazionali e completate la configurazione.
6. Puoi anche aggiungere utenti/gruppi al servizio Adobe Stock Cloud  per abilitare l&#39;accesso per utenti o gruppi specifici.

![Configurazione  Adobe risorse Stock](assets/screen_shot_2018-10-22at12425pm.png)

### Risorse aggiuntive

* [Piano Stock Enterprise](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Note sulla versione di AEM 6.4 Service Pack 2](https://helpx.adobe.com/it/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Integrare AEM e  Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [API di integrazione console I/O Adobe](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API Docs](https://www.adobe.io/apis/creativecloud/stock/docs.html)