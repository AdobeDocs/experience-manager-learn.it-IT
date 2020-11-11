---
title: Creare il modulo adattivo principale
description: Creare i moduli adattivi per acquisire le informazioni sui candidati e i moduli adattivi per recuperare il modulo adattivo salvato
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# Creare il modulo adattivo principale

Il modulo **StoreAFWithAttachments** è il modulo adattivo principale. Questo modulo adattivo è il punto di ingresso del caso d’uso. In questo modulo vengono acquisiti i dettagli utente, compreso il numero di cellulare. Questo modulo può anche aggiungere alcuni allegati. Quando si fa clic sul pulsante Save and Exit, il codice lato server viene eseguito per memorizzare i dati del modulo nel database e viene generato un ID applicazione univoco che viene presentato all&#39;utente per la sicurezza. Questo ID applicazione verrà utilizzato per recuperare il numero di cellulare associato all&#39;applicazione.

![modulo principale di applicazione](assets/6552.JPG)

Questo modulo è associato alle librerie client **bootboxjs540,storeAFWithAttachments** create in precedenza nel corso e a un flusso di lavoro AEM che viene attivato all&#39;invio del modulo.


* I moduli di esempio si basano su un modello [di modulo adattivo](assets/custom-template-with-page-component.zip) personalizzato che è necessario importare in AEM per consentire il corretto rendering dei moduli di esempio.

* È possibile scaricare e importare il modulo [](assets/store-af-with-attachments-form.zip) StoreAfWithAttachments completato nell&#39;istanza AEM.

* Affinché il modulo funzioni, è necessario importare il flusso di lavoro [AEM associato](assets/workflow-model-store-af-with-attachments.zip) all&#39;istanza AEM.



