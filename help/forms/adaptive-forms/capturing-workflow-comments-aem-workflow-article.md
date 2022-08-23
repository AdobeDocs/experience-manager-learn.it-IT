---
title: Catturare i commenti del flusso di lavoro nel Forms Workflow adattivo
description: Acquisizione dei commenti del flusso di lavoro in AEM flusso di lavoro
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# Catturare i commenti del flusso di lavoro nel Forms Workflow adattivo{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Si applica solo ad AEM Forms 6.4. In AEM Forms 6.5, utilizza la funzione variabili per ottenere questo caso d’uso]

Una richiesta comune è la possibilità di includere in un messaggio e-mail i commenti inseriti dal revisore attività. In AEM Forms 6.4 non esiste un meccanismo preconfigurato per acquisire i commenti inseriti dall’utente e includerli nell’e-mail.

Per soddisfare questo requisito, viene fornito un bundle OSGi di esempio che può essere utilizzato per acquisire commenti e memorizzarli come proprietà dei metadati del flusso di lavoro.

La schermata seguente mostra come utilizzare il passaggio del processo in [Flusso di lavoro AEM](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) per acquisire i commenti e archiviarli come proprietà di metadati. Il &quot;Capture Workflow Comments&quot; è il nome della classe java che deve essere utilizzata nel passaggio del processo. È necessario passare il nome della proprietà metadati che conterrà i commenti. Nella schermata seguente, managerComments è la proprietà dei metadati che memorizzerà i commenti.

![workflowcommenti1](assets/workflowcomments1.gif)

Per testare questa funzionalità sul sistema, segui i seguenti passaggi:
* [Assicurati che il passaggio del processo nel flusso di lavoro sia configurato per utilizzare i commenti del flusso di lavoro di acquisizione](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Distribuzione del bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Distribuzione del bundle SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo bundle contiene il codice di esempio per acquisire i commenti e memorizzarli come proprietà di metadati

* [Scarica e decomprimi le risorse correlate a questo articolo nel file system](assets/capturecomments.zip) Le risorse contengono il modello di flusso di lavoro e il modulo adattivo di esempio.

* Importare i 2 file zip in AEM utilizzando il gestore dei pacchetti

* [Visualizzare l’anteprima del modulo sfogliando questo URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Compila i campi del modulo e invia il modulo

* [Controlla la tua casella in entrata AEM](http://localhost:4502/aem/inbox)

* Apri l’attività dalla casella in entrata e invia il modulo. Inserisci alcuni commenti quando richiesto.

I commenti verranno memorizzati nella proprietà metadati denominata managerComments in crx. Per verificare i commenti, accedi a crx come amministratore. Le istanze del flusso di lavoro vengono memorizzate nel seguente percorso

/var/workflow/instances/server0

Selezionare l&#39;istanza di flusso di lavoro appropriata e controllare la proprietà managerComments nel nodo di metadati.
