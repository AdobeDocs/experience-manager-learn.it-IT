---
title: Acquisizione dei commenti del flusso di lavoro in Forms Workflow adattivo
seo-title: Acquisizione dei commenti del flusso di lavoro in Forms Workflow adattivo
description: Acquisizione dei commenti del flusso di lavoro in AEM flusso di lavoro
seo-description: Acquisizione dei commenti del flusso di lavoro in AEM flusso di lavoro
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---


# Acquisizione dei commenti del flusso di lavoro in Forms Workflow adattivo{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Si applica solo a  AEM Forms 6.4. In  AEM Forms 6.5, utilizzare la funzione delle variabili per ottenere questo caso d’uso]

Una richiesta comune è la possibilità di includere i commenti immessi dal revisore attività in un messaggio e-mail. In  AEM Forms 6.4 non è disponibile alcun meccanismo per acquisire i commenti inseriti dall&#39;utente e includerli nell&#39;e-mail.

Per soddisfare questo requisito, viene fornito un pacchetto OSGi di esempio che può essere utilizzato per acquisire commenti e memorizzarli come proprietà di metadati del flusso di lavoro.

La schermata seguente mostra come utilizzare il passaggio del processo in [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) per acquisire i commenti e memorizzarli come proprietà di metadati. Il nome della classe Java che deve essere utilizzata nella fase di processo è &quot;Capture Workflow Comments&quot; (Commenti flusso di lavoro di acquisizione). È necessario passare il nome della proprietà dei metadati che contiene i commenti. Nella schermata seguente, managerComments è la proprietà di metadati che memorizzerà i commenti.

![workflowcomments1](assets/workflowcomments1.gif)

Per testare questa funzionalità sul sistema, attenetevi alla seguente procedura:
* [Verificare che il passaggio del processo nel flusso di lavoro sia configurato per l&#39;utilizzo dei commenti del flusso di lavoro di acquisizione](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Distribuzione del bundle Developingwithservice](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Distribuire il bundle](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) SetValue. Questo pacchetto contiene il codice di esempio per acquisire i commenti e memorizzarlo come proprietà di metadati

* [Scaricate e decomprimete le risorse correlate a questo articolo nel ](assets/capturecomments.zip) sistema di fileLe risorse contengono il modello di flusso di lavoro e un esempio di modulo adattivo.

* Importare due file ZIP in AEM utilizzando il gestore pacchetti

* [Visualizzare l&#39;anteprima del modulo individuando questo URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Compilare i campi del modulo e inviare il modulo

* [Controlla la casella in entrata AEM](http://localhost:4502/aem/inbox)

* Aprire l&#39;attività dalla inbox e inviare il modulo. Inserite alcuni commenti quando richiesto.

I commenti vengono memorizzati nella proprietà dei metadati denominata managerComments in crx. Per verificare che il login dei commenti venga crx come amministratore. Le istanze del flusso di lavoro sono memorizzate nel seguente percorso

/var/workflow/instance/server0

Selezionare l&#39;istanza del flusso di lavoro appropriata e verificare se la proprietà managerCommenti è presente nel nodo di metadati.

