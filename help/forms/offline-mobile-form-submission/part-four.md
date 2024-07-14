---
title: Attivare il flusso di lavoro AEM all’invio di moduli HTM5 - Come ottimizzare il caso d’uso
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare il flusso di lavoro AEM
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
duration: 90
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---

# Come far funzionare questo caso d’uso sul sistema

>[!NOTE]
>
>Affinché le risorse di esempio funzionino sul sistema, si presume che l’istanza di AEM Author e Publish sia in esecuzione rispettivamente sulle porte 4502 e 4503. Si presume inoltre che l&#39;autore AEM sia accessibile tramite `admin`/`admin`. Se i numeri di porta o la password amministratore sono stati modificati, queste risorse di esempio non funzioneranno. Dovrai creare risorse personalizzate utilizzando il codice di esempio fornito.

Per utilizzare questo caso d’uso nel sistema locale, effettua le seguenti operazioni:

* Installa l’istanza di AEM Author sulla porta 4502 e l’istanza di AEM Publish sulla porta 4503
* [Segui le istruzioni specificate in sviluppo con l&#39;utente del servizio in AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Assicurati di creare l’utente del servizio e implementa il bundle nell’istanza AEM Author e Publish.
* [Aprire la configurazione osgi](http://localhost:4503/system/console/configMgr).
* Cerca **Filtro referrer Apache Sling**. Assicurati che la casella di controllo Consenti vuoto sia selezionata.
* [Distribuisci il bundle AEMFormDocumentService personalizzato](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Questo bundle deve essere distribuito nell&#39;istanza Publish AEM. Questo bundle ha il codice per generare PDF interattivi da un modulo mobile.
* [Scarica e decomprimi le risorse correlate a questo articolo.](assets/offline-pdf-submission-assets.zip) Riceverai quanto segue
   * **offline-submit-profile.zip** - Questo pacchetto AEM contiene il profilo personalizzato che ti consente di scaricare il pdf interattivo nel file system locale. Distribuisci questo pacchetto nell’istanza Publish dell’AEM.
   * **xdp-form-and-workflow.zip** - Questo pacchetto AEM contiene XDP, flusso di lavoro di esempio, modulo di avvio configurato su contenuto nodo/pdfsubmissions. Distribuisci questo pacchetto sia sull’istanza AEM Author che su quella Publish.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - Questo è il bundle AEM che esegue la maggior parte del lavoro. Questo bundle contiene il servlet montato su `/bin/startworkflow`. Questo servlet salva i dati del modulo inviato sotto il nodo `/content/pdfsubmissions` nell’archivio AEM. Distribuisci questo bundle sia sull’istanza AEM Author che su quella Publish.
* [Anteprima modulo mobile](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Compila diversi campi e fai clic sul pulsante sulla barra degli strumenti per scaricare il PDF interattivo.
* Compila il PDF scaricato utilizzando Acrobat e premi il pulsante Invia.
* Dovresti ricevere un messaggio di successo
* Accedi all’istanza di authoring dell’AEM come amministratore
* [Controlla la casella in entrata AEM](http://localhost:4502/aem/inbox)
* Dovresti avere un elemento lavoro per rivedere il PDF inviato

>[!NOTE]
>
>Invece di inviare il PDF al servlet in esecuzione sull’istanza Publish, alcuni clienti hanno distribuito il servlet nel relativo contenitore, ad esempio Tomcat. Dipende tutto dalla topologia con cui il cliente ha familiarità.ai fini di questo tutorial utilizzeremo il servlet distribuito sull’istanza Publish per gestire gli invii pdf.
