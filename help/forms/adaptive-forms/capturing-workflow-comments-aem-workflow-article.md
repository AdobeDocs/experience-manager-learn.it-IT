---
title: Acquisizione di commenti sul flusso di lavoro in un Forms Workflow adattivo
description: Acquisizione di commenti sul flusso di lavoro AEM
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 83
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Acquisizione di commenti sul flusso di lavoro in un Forms Workflow adattivo{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Applicabile solo ad AEM Forms 6.4. In AEM Forms 6.5 utilizza la funzione delle variabili per ottenere questo caso d’uso]

Una richiesta comune è la possibilità di includere in un messaggio e-mail i commenti immessi dal revisore dell’attività. In AEM Forms 6.4 non esiste un meccanismo predefinito per acquisire i commenti inseriti dall’utente e includerli nelle e-mail.

Per soddisfare questo requisito, viene fornito un bundle OSGi di esempio che può essere utilizzato per acquisire commenti e memorizzarli come proprietà dei metadati del flusso di lavoro.

La schermata seguente mostra come utilizzare il passaggio del processo in [Flusso di lavoro AEM](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) per acquisire commenti e memorizzarli come proprietà di metadati. &quot;Commenti del flusso di lavoro di acquisizione&quot; è il nome della classe Java che deve essere utilizzata nel passaggio del processo. È necessario trasmettere il nome della proprietà dei metadati che conterrà i commenti. Nella schermata seguente, managerComments è la proprietà dei metadati che memorizzerà i commenti.

![workflowcommenti1](assets/workflowcomments1.gif)

Per testare questa funzionalità sul sistema, attieniti alla seguente procedura:
* [Assicurati che il passaggio del processo nel flusso di lavoro sia configurato per utilizzare l’opzione Acquisisci commenti del flusso di lavoro](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Distribuire il bundle Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Distribuire il bundle SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Questo bundle contiene il codice di esempio per acquisire i commenti e memorizzarli come proprietà di metadati

* [Scarica e decomprimi nel file system le risorse correlate a questo articolo](assets/capturecomments.zip) Le risorse contengono un modello di flusso di lavoro e un modulo adattivo di esempio.

* Importare i 2 file zip in AEM utilizzando Gestione pacchetti

* [Visualizza l’anteprima del modulo sfogliando questo URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Compila i campi modulo e invia il modulo

* [Controlla la casella in entrata AEM](http://localhost:4502/aem/inbox)

* Apri l’attività dalla casella in entrata e invia il modulo. Immetti alcuni commenti quando richiesto.

I commenti vengono memorizzati nella proprietà dei metadati denominata `managerComments` nell’archivio AEM. Per verificare la presenza di commenti, accedi a crx come amministratore. Le istanze del flusso di lavoro vengono memorizzate nel percorso seguente:

`/var/workflow/instances/server0`

Seleziona l’istanza di flusso di lavoro appropriata e verifica la presenza di property managerComments nel nodo dei metadati.
