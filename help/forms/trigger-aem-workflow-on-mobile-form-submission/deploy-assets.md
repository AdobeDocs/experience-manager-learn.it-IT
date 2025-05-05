---
title: Attivare il flusso di lavoro di AEM per l’invio di moduli HTML5 - Come ottimizzare il caso d’uso
description: Distribuire le risorse di esempio sul sistema locale
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 9417235f-2e8d-45c7-86eb-104478a69a19
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---

# Come far funzionare questo caso d’uso sul sistema

>[!NOTE]
>
>Affinché le risorse di esempio possano funzionare sul sistema, si presume che tu abbia accesso a un’istanza Autore AEM Forms e Publish AEM Forms.

Per utilizzare questo caso d’uso nel sistema locale, effettua le seguenti operazioni:

## Distribuisci quanto segue nell’istanza Autore AEM Forms

* [Installare il bundle MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [Distribuire il bundle di sviluppo con l&#39;utente del servizio](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=it)
Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service utilizzando configMgr

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* È possibile archiviare gli invii del modulo in una cartella diversa specificando il nome della cartella nella configurazione delle credenziali del server AEM utilizzando [configMgr](http://localhost:4502/system/console/configMg). Se modifichi la cartella, assicurati di creare un modulo di avvio nella cartella per attivare il flusso di lavoro **ReviewSubmittedPDF**

![config-author](assets/author-config.png)
* [Importa l&#39;xdp di esempio e il pacchetto del flusso di lavoro utilizzando Gestione pacchetti](assets/xdp-form-and-workflow.zip).


## Distribuisci le seguenti risorse nell’istanza di pubblicazione

* [Installare il bundle MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* Specifica il nome utente/password per l&#39;istanza di authoring e una posizione **esistente nel tuo archivio AEM** per archiviare i dati inviati nelle credenziali di AEM Server utilizzando [configMgr](http://localhost:4503/system/console/configMgr). Puoi lasciare invariato l’URL dell’endpoint su AEM Workflow Server. Endpoint che estrae e memorizza i dati dall&#39;invio nel nodo specificato.
  ![publish-config](assets/publish-config.png)

* [Distribuire il bundle di sviluppo con l&#39;utente del servizio](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=it)
* [Aprire la configurazione osgi](http://localhost:4503/system/console/configMgr).
* Cerca **Filtro referrer Apache Sling**. Assicurati che la casella di controllo Consenti vuoto sia selezionata.


## Testare la soluzione

* Accedi all’istanza di authoring
* [Modifica le proprietà avanzate di w9.xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp). Assicurati che l’URL di invio e il profilo di rendering siano impostati correttamente come mostrato di seguito.
  ![xdp-advanced-properties](assets/mobile-form-properties.png)

* Pubblicare w9.xdp
* Accedi all’istanza di pubblicazione
* [Anteprima del modulo w9](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* Compila alcuni campi del modulo e invia il modulo
* Accedi all’istanza di authoring di AEM come amministratore
* [Controlla la casella in entrata di AEM](http://localhost:4502/aem/inbox)
* Dovresti avere un elemento lavoro per rivedere il PDF inviato

>[!NOTE]
>
>Invece di inviare il PDF al servlet in esecuzione sull’istanza di pubblicazione, alcuni clienti hanno implementato il servlet nel relativo contenitore, ad esempio Tomcat. Tutto dipende dalla topologia di cui il cliente ha familiarità.Ai fini di questo tutorial utilizzeremo il servlet distribuito sull’istanza Publish per gestire gli invii dei moduli.
