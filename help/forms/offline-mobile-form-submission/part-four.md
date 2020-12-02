---
title: Attivazione AEM flusso di lavoro per l’invio di moduli HTML5
seo-title: Attiva AEM flusso di lavoro per l’invio di moduli HTML5
description: Continuare a compilare il modulo per dispositivi mobili in modalità offline e inviare il modulo per dispositivi mobili per attivare AEM flusso di lavoro
seo-description: Continuare a compilare il modulo per dispositivi mobili in modalità offline e inviare il modulo per dispositivi mobili per attivare AEM flusso di lavoro
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# Ottenimento del caso di utilizzo per il sistema

>[!NOTE]
>
>Per utilizzare le risorse di esempio sul sistema, si presuppone che l’istanza AEM Author e Publish sia in esecuzione rispettivamente sulle porte 4502 e 4503. Si presume inoltre che AEM Author sia accessibile tramite `admin`/`admin`. Se i numeri di porta o la password dell&#39;amministratore sono stati modificati, queste risorse di esempio non funzioneranno. Dovrete creare risorse personalizzate utilizzando il codice di esempio fornito.

Per utilizzare questo caso di utilizzo sul sistema locale, procedere come segue:

* Installa l’istanza di AEM Author sulla porta 4502 e l’istanza di AEM Publish sulla porta 4503
* [Seguite le istruzioni specificate nello sviluppo con gli utenti del servizio in  AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Assicuratevi di creare l&#39;utente del servizio e distribuire il bundle nell&#39;istanza di AEM Author e Publish.
* [Aprite la configurazione osgi  ](http://localhost:4503/system/console/configMgr).
* Cercare il **filtro di riferimento Apache Sling**. Assicurarsi che la casella di controllo Consenti valori vuoti sia selezionata.
* [Distribuisci il pacchetto](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) AEMFormDocumentService personalizzato. Questo bundle deve essere distribuito nell’istanza AEM Publish. Questo bundle contiene il codice che consente di generare PDF interattivi dal modulo mobile.
* [Scaricate e decomprimete il file ZIP delle risorse correlate a questo articolo.](assets/offline-pdf-submission-assets.zip) Verranno fornite le seguenti informazioni
   * **offline-submit-profile.zip**  - Questo pacchetto di AEM contiene il profilo personalizzato che consente di scaricare il pdf interattivo nel file system locale. Distribuite questo pacchetto nell&#39;istanza AEM Publish.
   * **xdp-form-and-workflow.zip** : questo pacchetto AEM contiene XDP, un flusso di lavoro di esempio, un avvio configurato su nodi content/pdfinvii. Distribuite questo pacchetto sia sull&#39;istanza di AEM Author che su Publish.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - Si tratta del bundle AEM che esegue la maggior parte del lavoro. Questo bundle contiene il servlet montato su `/bin/startworkflow`. Questo servlet salva i dati del modulo inviati sotto il nodo `/content/pdfsubmissions` AEM repository. Distribuite questo bundle sia sull&#39;istanza AEM Author che su AEM Publish.
* [Anteprima del modulo mobile](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Compilate diversi campi e fate clic sul pulsante nella barra degli strumenti per scaricare il PDF interattivo.
* Compilate il PDF scaricato utilizzando  Acrobat e fate clic sul pulsante Invia.
* Dovresti ricevere un messaggio di successo
* Accedi all’istanza di AEM Author come amministratore
* [Controllare la casella in entrata AEM](http://localhost:4502/aem/inbox)
* È necessario disporre di un elemento di lavoro per esaminare il PDF inviato

>[!NOTE]
>
>Invece di inviare il PDF al servlet in esecuzione sull’istanza di pubblicazione, alcuni clienti hanno implementato il servlet nel contenitore servlet, come Tomcat. Dipende tutto dalla topologia con cui il cliente è a suo agio.per questo tutorial utilizzeremo il servlet distribuito sull&#39;istanza di pubblicazione per gestire i pdf inviati.

