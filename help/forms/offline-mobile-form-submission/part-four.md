---
title: Flusso di lavoro di attivazione AEM per l’invio di moduli HTML5
seo-title: Flusso di lavoro AEM trigger sull’invio di moduli HTML5
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare AEM flusso di lavoro
seo-description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare AEM flusso di lavoro
feature: Forms Mobile
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Sviluppo
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---


# Come far funzionare questo caso d&#39;uso sul tuo sistema

>[!NOTE]
>
>Affinché le risorse di esempio funzionino sul sistema, si presume che l’istanza Author e Publish di AEM sia in esecuzione rispettivamente sulle porte 4502 e 4503. Si presume anche che AEM Author sia accessibile tramite `admin`/`admin`. Se i numeri di porta o la password dell’amministratore sono stati modificati, queste risorse di esempio non funzioneranno. Dovrai creare risorse personalizzate utilizzando il codice di esempio fornito.

Per far funzionare questo caso d&#39;uso sul sistema locale, segui questi passaggi:

* Installa l&#39;istanza AEM Author sulla porta 4502 e l&#39;istanza AEM Publish sulla porta 4503
* [Segui le istruzioni specificate in sviluppo con gli utenti del servizio in AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Assicurati di creare l’utente del servizio e distribuire il bundle sull’istanza di authoring e pubblicazione di AEM.
* [Apri la configurazione osgi  ](http://localhost:4503/system/console/configMgr).
* Cerca **Filtro di riferimento Sling Apache**. Assicurati che la casella di controllo Consenti vuoto sia selezionata.
* [Distribuisci il bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) AEMFormDocumentService personalizzato. Questo bundle deve essere distribuito sull&#39;istanza AEM Publish. Questo bundle ha il codice per generare PDF interattivi dal modulo mobile.
* [Scarica e decomprimi le risorse correlate a questo articolo.](assets/offline-pdf-submission-assets.zip) Otterrai quanto segue
   * **offline-submit-profile.zip**  - Questo pacchetto di AEM contiene il profilo personalizzato che consente di scaricare il pdf interattivo nel file system locale. Distribuisci questo pacchetto sulla tua istanza di AEM Publish.
   * **xdp-form-and-workflow.zip**  - Questo pacchetto AEM contiene XDP, un flusso di lavoro di esempio, un modulo di avvio configurato sul contenuto del nodo/pdfinvii. Distribuisci questo pacchetto sia sull’istanza Author che Publish di AEM.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  - Questo è il bundle AEM che esegue la maggior parte del lavoro. Questo bundle contiene il servlet montato su `/bin/startworkflow`. Questo servlet salva i dati del modulo inviati sotto il nodo `/content/pdfsubmissions` AEM repository. Distribuisci questo bundle sia sull&#39;istanza Author che Publish di AEM.
* [Anteprima del modulo mobile](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Compila diversi campi, quindi fai clic sul pulsante sulla barra degli strumenti per scaricare il PDF interattivo.
* Compila il PDF scaricato utilizzando Acrobat e premi il pulsante Invia.
* Dovresti ricevere un messaggio di successo
* Accedi all’istanza di AEM Author come amministratore
* [Controlla la casella in entrata AEM](http://localhost:4502/aem/inbox)
* È necessario disporre di un elemento di lavoro per esaminare il PDF inviato

>[!NOTE]
>
>Invece di inviare il PDF al servlet in esecuzione sull’istanza di pubblicazione, alcuni clienti hanno implementato il servlet nel contenitore servlet come Tomcat. Tutto dipende dalla topologia con cui il cliente si trova a suo agio.per questo tutorial utilizzeremo il servlet distribuito sull&#39;istanza di pubblicazione per gestire gli invii pdf.

