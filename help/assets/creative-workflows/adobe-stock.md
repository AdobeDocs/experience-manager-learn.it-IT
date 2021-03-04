---
title: Utilizzo di risorse Adobe Stock con AEM Assets
description: 'AEM consente agli utenti di cercare, visualizzare in anteprima, salvare e concedere in licenza le risorse Adobe Stock direttamente da AEM. Le organizzazioni possono ora integrare il piano Adobe Stock Enterprise con AEM Assets per garantire che le risorse concesse in licenza siano ora ampiamente disponibili per i loro progetti creativi e di marketing, con le potenti funzionalità di gestione delle risorse di AEM. '
feature: Adobe Stock
version: 6.4, 6.5
topic: Gestione dei contenuti
role: Professionista
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 6%

---


# Utilizzo di Adobe Stock con AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 consente agli utenti di cercare, visualizzare in anteprima, salvare e concedere in licenza le risorse Adobe Stock direttamente da AEM. Le organizzazioni possono ora integrare il piano Adobe Stock Enterprise con AEM Assets per garantire che le risorse concesse in licenza siano ora ampiamente disponibili per i loro progetti creativi e di marketing, con le potenti funzionalità di gestione delle risorse di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>L’integrazione richiede un [piano aziendale Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 con almeno Service Pack 2 implementato. Per informazioni dettagliate sul service pack AEM 6.4, consulta le seguenti [note sulla versione](https://helpx.adobe.com/it/experience-manager/6-4/release-notes/sp-release-notes.html).

L’integrazione di Adobe Stock e AEM Assets consente agli autori dei contenuti e agli esperti di marketing di concedere facilmente la licenza e utilizzare facilmente le risorse Stock a scopo creativo o di marketing. Puoi eseguire una ricerca delle risorse Stock utilizzando Omni Search, aggiungendo il filtro della posizione come Adobe Stock o navigando nella navigazione principale di AEM Assets e facendo clic sull’icona dell’interfaccia utente di Google Stock Coral.

## Funzionalità

### Ricerca e salvataggio

* Esegui la ricerca delle risorse di Adobe Stock senza uscire dall’area di lavoro di AEM.
* Salva le risorse Adobe Stock per l’anteprima, senza concedere la licenza per la risorsa.
* Possibilità di concedere in licenza e salvare risorse Adobe Stock in AEM Assets
* Possibilità di cercare risorse simili da Adobe Stock nell’interfaccia utente di AEM Assets
* Visualizzare una risorsa selezionata da Ricerca Stock nel sito web Adobe Stock di AEM Assets
* I file delle risorse concesse in licenza sono contrassegnati da un badge blu con licenza per facilitarne l’identificazione

### Metadati risorsa

* Le risorse concesse in licenza vengono memorizzate in AEM Assets. Le proprietà della risorsa contengono i metadati Stock sotto una scheda separata dei metadati della risorsa
* Possibilità di aggiungere riferimenti di licenza ai metadati delle risorse

### Profilo Stock di risorsa

* Un utente può selezionare il profilo Adobe Stock in *Utente > Preferenze > Configurazione stock*
* È possibile aggiungere riferimenti obbligatori e facoltativi alla finestra Asset Licensing.
* Possibilità di scegliere la lingua preferita per la finestra Asset Licensing in base all&#39;area geografica.

### Filtro

* Un utente può filtrare le risorse in base a Tipo di risorsa, Orientamento e Vista simili
* Il tipo di risorsa include Foto, Illustrazioni, Vettori, Video, Modelli, 3D, Premium, Editorial
* L&#39;orientamento include orizzontale, verticale e quadrato.
* Per visualizzare un filtro simile è necessario il numero di file Adobe Stock

### Controllo accesso

* Gli amministratori possono fornire autorizzazioni a determinati utenti/gruppi per concedere in licenza le risorse stock durante la configurazione del servizio cloud di Adobe Stock.
* Se un utente/gruppo specifico non dispone dell’autorizzazione per concedere in licenza le risorse di stock, la funzionalità *Ricerca risorse di Stock / Licenza risorse* verrà disabilitata.

## Impostare Adobe Stock con AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 consente agli utenti di cercare, visualizzare in anteprima, salvare e concedere in licenza le risorse Adobe Stock direttamente da AEM. Questo video illustra una breve descrizione di come configurare Adobe Stock con AEM Assets tramite la console Adobe I/O.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Per la configurazione del servizio di Adobe Stock Cloud, è necessario selezionare il percorso della risorsa di PROD143 e di Ambiente e Licenza su /content/dam. Il campo Ambiente verrà rimosso nella prossima versione di AEM e il percorso della risorsa con licenza fa parte di una delle prossime funzionalità e il supporto per questo campo verrà introdotto nella prossima versione di AEM.

>[!NOTE]
>
>L’integrazione richiede un [piano Adobe Stock aziendale](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 con almeno [Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) implementato. Per informazioni dettagliate sul service pack AEM 6.4, consulta le seguenti [note sulla versione](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). Per configurare l’integrazione, è inoltre necessario disporre delle autorizzazioni di amministratore per [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager.

### Installazione {#installations}

* Per AEM 6.4, devi installare il [Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) di AEM, quindi reinstallare il file cq-dam-stock-integration-content-1.0.4.zip.
* Assicurati di disporre delle autorizzazioni di amministratore su [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager per configurare l&#39;integrazione.

#### Configurazione di Adobe IMS tramite la console Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Crea una configurazione account tecnico Adobe IMS in **Strumenti > Sicurezza**
2. Seleziona la *Soluzione cloud* come *Adobe Stock* e crea un nuovo certificato o riutilizza un certificato esistente per la configurazione.
3. Passa alla console Adobe I/O e crea una nuova integrazione dell’account di servizio per *Adobe Stock*.
4. Carica il certificato dal passaggio 2 all’integrazione dell’account Adobe Stock Service.
5. Scegli la configurazione di profilo Adobe Stock richiesta e completa l’integrazione del servizio.
6. Utilizza i dettagli di integrazione per completare la configurazione dell’account tecnico Adobe IMS
7. Assicurati di poter ricevere il token di accesso utilizzando l’account tecnico Adobe IMS.

![Account tecnico Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurare Adobe Stock Cloud Services {#set-up-adobe-stock-cloud-services}

1. Crea una nuova configurazione del servizio cloud per Adobe Stock in **Strumenti > Servizi cloud.**
2. Seleziona la *Configurazione Adobe IMS* creata nella sezione precedente per la configurazione *Adobe Stock Cloud*

3. Assicuratevi di selezionare **AMBIENTE** come PROD. L’ambiente di staging non è supportato e verrà rimosso nella prossima versione di AEM.
4. **Il** percorso della risorsa in licenza può essere indicato in qualsiasi directory sotto /content/dam. Il supporto delle funzioni per questo campo verrà aggiunto nella prossima versione di AEM
5. Seleziona le impostazioni internazionali e completa la configurazione.
6. Puoi anche aggiungere utenti/gruppi al servizio Adobe Stock Cloud per abilitare l’accesso per utenti o gruppi specifici.

![Configurazione di Adobe Assets Stock](assets/screen_shot_2018-10-22at12425pm.png)

### Risorse aggiuntive

* [Piano d&#39;azione aziendale](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Note sulla versione di AEM 6.4 Service Pack 2](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Integrare AEM e Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [API di integrazione della console Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentazione API di Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)