---
title: 'Attivare il flusso di lavoro AEM all’invio del modulo HTML5: come ottimizzare il caso d’uso'
description: Distribuire le risorse di esempio sul sistema locale
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
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

* [Distribuire il bundle di sviluppo con l&#39;utente del servizio](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
Aggiungi la seguente voce nel servizio User Mapper di Apache Sling Service utilizzando configMgr

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* È possibile archiviare gli invii del modulo in una cartella diversa specificando il nome della cartella nella configurazione delle credenziali del server AEM utilizzando [configMgr](http://localhost:4502/system/console/configMg). Se modifichi la cartella, assicurati di creare un modulo di avvio nella cartella per attivare il flusso di lavoro **ReviewSubmittedPDF**

![config-author](assets/author-config.png)
* [Importa l&#39;xdp di esempio e il pacchetto del flusso di lavoro utilizzando Gestione pacchetti](assets/xdp-form-and-workflow.zip).


## Distribuisci le seguenti risorse nell’istanza di pubblicazione

* [Installare il bundle MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* Specifica il nome utente/password per l&#39;istanza di authoring e una posizione **esistente nel tuo archivio AEM** per archiviare i dati inviati nelle credenziali del server AEM utilizzando [configMgr](http://localhost:4503/system/console/configMgr). Puoi lasciare invariato l’URL dell’endpoint sul server del flusso di lavoro AEM. Endpoint che estrae e memorizza i dati dall&#39;invio nel nodo specificato.
  ![publish-config](assets/publish-config.png)

* [Distribuire il bundle di sviluppo con l&#39;utente del servizio](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
* [Aprire la configurazione osgi](http://localhost:4503/system/console/configMgr).
* Cerca **Filtro referrer Apache Sling**. Assicurati che la casella di controllo Consenti vuoto sia selezionata.


## Testare la soluzione

* Accedi all’istanza di authoring
* [Modifica le proprietà avanzate di w9.xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp). Assicurati che l’URL di invio e il profilo di rendering siano impostati correttamente come mostrato di seguito.
  ![xdp-advanced-properties](assets/mobile-form-properties.png)

* Publish il w9.xdp
* Accedi all’istanza di pubblicazione
* [Anteprima del modulo w9](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* Compila alcuni campi del modulo e invia il modulo
* Accedi all’istanza di authoring dell’AEM come amministratore
* [Controlla la casella in entrata AEM](http://localhost:4502/aem/inbox)
* Dovresti avere un elemento lavoro per rivedere il PDF inviato

>[!NOTE]
>
>Invece di inviare il PDF al servlet in esecuzione sull’istanza Publish, alcuni clienti hanno distribuito il servlet nel relativo contenitore, ad esempio Tomcat. Tutto dipende dalla topologia di cui il cliente ha familiarità.Ai fini di questo tutorial utilizzeremo il servlet distribuito sull’istanza Publish per gestire gli invii dei moduli.
