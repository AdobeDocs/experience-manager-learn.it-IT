---
title: Acquisizione dei commenti del flusso di lavoro nel flusso di lavoro Moduli adattivi
seo-title: Acquisizione dei commenti del flusso di lavoro nel flusso di lavoro Moduli adattivi
description: Acquisizione dei commenti del flusso di lavoro in AEM Workflow
seo-description: Acquisizione dei commenti del flusso di lavoro in AEM Workflow
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: Flusso di lavoro
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
topic: Sviluppo
role: Developer (Sviluppatore)
level: Esperienza
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# Acquisizione dei commenti del flusso di lavoro in Flusso di lavoro per moduli adattivi{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Si applica solo a AEM Forms 6.4. In AEM Forms 6.5, utilizza la funzione variabili per ottenere questo caso d’uso]

Una richiesta comune è la possibilità di includere in un messaggio e-mail i commenti inseriti dal revisore attività. In AEM Forms 6.4 non esiste un meccanismo preconfigurato per acquisire i commenti inseriti dall’utente e includerli nell’e-mail.

Per soddisfare questo requisito, viene fornito un bundle OSGi di esempio che può essere utilizzato per acquisire commenti e memorizzarli come proprietà dei metadati del flusso di lavoro.

La schermata seguente mostra come utilizzare il passaggio del processo in [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) per acquisire i commenti e memorizzarli come proprietà dei metadati. Il &quot;Capture Workflow Comments&quot; è il nome della classe java che deve essere utilizzata nel passaggio del processo. È necessario passare il nome della proprietà metadati che conterrà i commenti. Nella schermata seguente, managerComments è la proprietà dei metadati che memorizzerà i commenti.

![workflowcommenti1](assets/workflowcomments1.gif)

Per testare questa funzionalità sul sistema, segui i seguenti passaggi:
* [Assicurati che il passaggio del processo nel flusso di lavoro sia configurato per utilizzare i commenti del flusso di lavoro di acquisizione](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Distribuzione del bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Distribuisci il bundle SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo bundle contiene il codice di esempio per acquisire i commenti e memorizzarli come proprietà di metadati

* [Scarica e decomprimi le risorse correlate a questo articolo nel ](assets/capturecomments.zip) file systemLe risorse contengono il modello di flusso di lavoro e un esempio di Modulo adattivo.

* Importa i 2 file zip in AEM utilizzando il gestore di pacchetti

* [Visualizzare l’anteprima del modulo sfogliando questo URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Compila i campi del modulo e invia il modulo

* [Controlla la casella in entrata AEM](http://localhost:4502/aem/inbox)

* Apri l’attività dalla casella in entrata e invia il modulo. Inserisci alcuni commenti quando richiesto.

I commenti verranno memorizzati nella proprietà metadati denominata managerComments in crx. Per verificare i commenti, accedi a crx come amministratore. Le istanze del flusso di lavoro vengono memorizzate nel seguente percorso

/var/workflow/instances/server0

Selezionare l&#39;istanza di flusso di lavoro appropriata e controllare la proprietà managerComments nel nodo di metadati.

