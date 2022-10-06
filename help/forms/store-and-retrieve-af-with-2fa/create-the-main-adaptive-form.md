---
title: Creare il modulo adattivo principale
description: Crea i moduli adattivi per acquisire le informazioni dei richiedenti e il modulo adattivo per recuperare il modulo adattivo salvato
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Creare il modulo adattivo principale

Il modulo **StoreAFWithAttachments** è il modulo adattivo principale. Questo modulo adattivo è il punto di ingresso del caso d’uso. In questo modulo vengono acquisiti i dettagli utente, compreso il numero di cellulare. Questo modulo ha anche la possibilità di aggiungere alcuni allegati. Quando si fa clic sul pulsante Save and Exit , il codice lato server viene eseguito per memorizzare i dati del modulo nel database e viene generato un ID applicazione univoco che viene presentato all’utente per la custodia sicura. Questo ID applicazione viene utilizzato per recuperare il numero di cellulare associato all&#39;applicazione.

![modulo di applicazione principale](assets/6552.JPG)

Questo modulo è associato a **bootboxjs540,storeAFWithAttachments** librerie client create in precedenza nel corso e un flusso di lavoro AEM che viene attivato all’invio del modulo.


* I moduli di esempio sono basati su [modello di modulo adattivo personalizzato](assets/custom-template-with-page-component.zip) che deve essere importato in AEM affinché il rendering dei moduli di esempio sia corretto.

* Completato [Modulo StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) può essere scaricato e importato nell’istanza AEM.

* La [Flusso di lavoro AEM associato a questo modulo](assets/workflow-model-store-af-with-attachments.zip) è necessario importarlo nell’istanza AEM affinché il modulo funzioni.
