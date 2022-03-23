---
title: Flusso di lavoro AEM trigger sull’invio di moduli HTML5 - Ottenere il funzionamento del caso d’uso
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continua a compilare il modulo mobile in modalità offline e invia il modulo mobile per attivare AEM flusso di lavoro
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# Come far funzionare questo caso d&#39;uso sul tuo sistema

>[!NOTE]
>
>Affinché le risorse di esempio funzionino sul sistema, si presume che l’istanza Author e Publish di AEM sia in esecuzione rispettivamente sulle porte 4502 e 4503. Si presume anche che AEM Author sia accessibile tramite `admin`/`admin`. Se i numeri di porta o la password dell’amministratore sono stati modificati, queste risorse di esempio non funzioneranno. Dovrai creare risorse personalizzate utilizzando il codice di esempio fornito.

Per far funzionare questo caso d&#39;uso sul sistema locale, segui questi passaggi:

* Installa l&#39;istanza AEM Author sulla porta 4502 e l&#39;istanza AEM Publish sulla porta 4503
* [Segui le istruzioni specificate in sviluppo con gli utenti del servizio in AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Assicurati di creare l’utente del servizio e distribuire il bundle sull’istanza di authoring e pubblicazione di AEM.
* [Apri la configurazione osgi ](http://localhost:4503/system/console/configMgr).
* Cerca  **Filtro di riferimento Apache Sling**. Assicurati che la casella di controllo Consenti vuoto sia selezionata.
* [Distribuire il bundle AEMFormDocumentService personalizzato](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Questo bundle deve essere distribuito sulla tua istanza AEM Publish. Questo bundle ha il codice per generare PDF interattivo dal modulo mobile.
* [Scarica e decomprimi le risorse correlate a questo articolo.](assets/offline-pdf-submission-assets.zip) Otterrai quanto segue
   * **offline-submit-profile.zip** - Questo pacchetto AEM contiene il profilo personalizzato che consente di scaricare il pdf interattivo nel file system locale. Distribuisci questo pacchetto sulla tua istanza di AEM Publish.
   * **xdp-form-and-workflow.zip** - Questo pacchetto AEM contiene XDP, flusso di lavoro di esempio, modulo di avvio configurato su node content/pdfsubmit. Distribuisci questo pacchetto sia sull’istanza Author che Publish di AEM.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - Questo è il pacchetto AEM che fa la maggior parte del lavoro. Questo bundle contiene il servlet montato su `/bin/startworkflow`. Questo servlet salva i dati del modulo inviati in `/content/pdfsubmissions` nodo AEM repository. Distribuisci questo bundle sia sull&#39;istanza Author che Publish di AEM.
* [Anteprima del modulo mobile](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Compila diversi campi, quindi fai clic sul pulsante sulla barra degli strumenti per scaricare il PDF interattivo.
* Compila il PDF scaricato utilizzando Acrobat e premi il pulsante di invio.
* Dovresti ricevere un messaggio di successo
* Accedi all’istanza di AEM Author come amministratore
* [Controlla la casella in entrata AEM](http://localhost:4502/aem/inbox)
* È necessario disporre di un elemento di lavoro per esaminare PDF inviato

>[!NOTE]
>
>Invece di inviare il servlet PDF al servlet in esecuzione sull’istanza di pubblicazione, alcuni clienti hanno implementato il servlet nel contenitore servlet come Tomcat. Tutto dipende dalla topologia con cui il cliente si trova a suo agio.per questo tutorial utilizzeremo il servlet distribuito sull&#39;istanza di pubblicazione per gestire gli invii pdf.
