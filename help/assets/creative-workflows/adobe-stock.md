---
title: Utilizzo di risorse Adobe Stock con AEM Assets
description: 'AEM consente agli utenti di cercare, visualizzare in anteprima, salvare e concedere in licenza le risorse Adobe Stock direttamente da AEM. Le organizzazioni possono ora integrare il piano Adobe Stock Enterprise con AEM Assets per garantire che le risorse concesse in licenza siano ora ampiamente disponibili per i loro progetti creativi e di marketing, con le potenti funzionalità di gestione delle risorse di AEM. '
feature: Adobe Stock
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1037'
ht-degree: 2%

---


# Utilizzo di Adobe Stock con AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 consente agli utenti di cercare, visualizzare in anteprima, salvare e concedere in licenza le risorse Adobe Stock direttamente da AEM. Le organizzazioni possono ora integrare il piano Adobe Stock Enterprise con AEM Assets per garantire che le risorse concesse in licenza siano ora ampiamente disponibili per i loro progetti creativi e di marketing, con le potenti funzionalità di gestione delle risorse di AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=12&learn=on)

>[!NOTE]
>
>L&#39;integrazione richiede un [piano Adobe Stock aziendale](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 con almeno Service Pack 2 implementato. Per AEM i dettagli del service pack 6.4, consulta le seguenti [note sulla versione](https://helpx.adobe.com/it/experience-manager/6-4/release-notes/sp-release-notes.html).

L’integrazione di Adobe Stock e AEM Assets consente agli autori e ai professionisti del marketing dei contenuti di concedere facilmente la licenza e utilizzare facilmente le risorse Stock per scopi creativi o di marketing. Puoi eseguire una ricerca delle risorse Stock utilizzando Omni Search, aggiungendo il filtro della posizione come Adobe Stock oppure navigando nella navigazione principale di AEM Assets e facendo clic sull’icona dell’interfaccia utente Coral di Adobe Stock Search.

## Funzionalità

### Ricerca e salvataggio

* Esegui la ricerca delle risorse Adobe Stock senza uscire AEM&#39;area di lavoro.
* Salva le risorse Adobe Stock per l’anteprima, senza concedere la licenza per la risorsa.
* Possibilità di concedere in licenza e salvare risorse Adobe Stock in AEM Assets
* Possibilità di cercare risorse simili da Adobe Stock nell’interfaccia utente di AEM Assets
* Visualizzare una risorsa selezionata da Stock Search nel sito web AEM Assets su Adobe Stock
* I file delle risorse concesse in licenza sono contrassegnati da un badge blu con licenza per facilitarne l’identificazione

### Metadati risorsa

* La risorsa in licenza viene memorizzata in AEM Assets. Le proprietà della risorsa contengono i metadati Stock sotto una scheda separata dei metadati della risorsa
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

* Gli amministratori possono fornire autorizzazioni a determinati utenti/gruppi per concedere in licenza le risorse stock durante la configurazione della configurazione del servizio cloud Adobe Stock.
* Se un utente/gruppo specifico non dispone dell’autorizzazione per concedere in licenza le risorse di stock, la funzionalità *Ricerca risorse di Stock / Licenza risorse* verrà disabilitata.

## Configurazione di Adobe Stock con AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 consente agli utenti di cercare, visualizzare in anteprima, salvare e concedere in licenza le risorse Adobe Stock direttamente da AEM. Questo video illustra una breve descrizione di come impostare le scorte di Adobi con AEM Assets utilizzando la console Adobe I/O.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Per la configurazione del servizio Adobe Stock Cloud, è necessario selezionare il percorso della risorsa di PROD143 Ambiente e Licenza su /content/dam. Il campo Ambiente verrà rimosso nella prossima versione di AEM e il percorso della risorsa con licenza fa parte di una prossima funzionalità e il supporto per questo campo verrà introdotto nella prossima versione di AEM.

>[!NOTE]
>
>L&#39;integrazione richiede un [piano Adobe Stock aziendale](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) e AEM 6.4 con almeno [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.property.operation=equals&amp;2_group.property.values.0_values=target-version%3Aaem%2F6-4&amp;3_group.property.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.property.operation=equals&amp;3_group.property.values.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=assort c&amp;layout=list&amp;p.offset=0&amp;p.limit=24) implementato. Per AEM i dettagli del service pack 6.4, consulta le seguenti [note sulla versione](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). Per configurare l’integrazione, è inoltre necessario disporre delle autorizzazioni di amministratore per [Console di Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager.

### Installazione {#installations}

* Per AEM 6.4, è necessario installare il [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.property.operation=equals&amp;2_group.property.values.0_values=target-version%3Aaem%2F6-4&amp;3_group.property.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.property.operation=equals&amp;3_group.property.values.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=assort c&amp;layout=list&amp;p.offset=0&amp;p.limit=24) e quindi reinstallare il file cq-dam-stock-integration-content-1.0.4.zip.
* Assicurati di disporre delle autorizzazioni di amministratore su [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) e Adobe Experience Manager per configurare l&#39;integrazione.

#### Configurazione della configurazione Adobe IMS tramite la console Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Crea un Adobe di configurazione dell’account tecnico IMS in **Strumenti > Sicurezza**
2. Seleziona la *Soluzione cloud* come *Adobe Stock* e crea un nuovo certificato o riutilizza un certificato esistente per la configurazione.
3. Passa alla console Adobe I/O e crea una nuova integrazione dell&#39;account di servizio per *Adobe Stock*.
4. Carica il certificato dal passaggio 2 all’integrazione dell’account di servizio Adobe Stock.
5. Scegli la configurazione di profilo Adobe Stock richiesta e completa l’integrazione del servizio.
6. Utilizza i dettagli di integrazione per completare la configurazione Adobe IMS Technical Account
7. Assicurati di poter ricevere il token di accesso utilizzando l’account tecnico Adobe IMS.

![Account tecnico Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurazione Cloud Services Adobe Stock {#set-up-adobe-stock-cloud-services}

1. Crea una nuova configurazione del servizio cloud per Adobe Stock in **Strumenti > Cloud Services.**
2. Seleziona la *Configurazione Adobe IMS* creata nella sezione precedente per la configurazione *Adobe Stock Cloud*

3. Assicuratevi di selezionare **AMBIENTE** come PROD. L’ambiente di staging non è supportato e verrà rimosso nella prossima versione di AEM.
4. **Il** percorso della risorsa in licenza può essere indicato in qualsiasi directory sotto /content/dam. Il supporto delle funzioni per questo campo verrà aggiunto nella prossima versione di AEM
5. Seleziona le impostazioni internazionali e completa la configurazione.
6. Puoi anche aggiungere utenti/gruppi al servizio Adobe Stock Cloud per abilitare l’accesso per utenti o gruppi specifici.

![Configurazione stock risorse di Adobe](assets/screen_shot_2018-10-22at12425pm.png)

### Risorse aggiuntive

* [Piano d&#39;azione aziendale](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Note sulla versione di AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-64/release-notes/sp-release-notes.html?lang=en)
* [Integrare AEM e Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [API di integrazione della console di Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentazione API di Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)