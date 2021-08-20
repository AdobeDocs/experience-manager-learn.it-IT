---
title: Creare il modulo adattivo principale
description: Crea i moduli adattivi per acquisire le informazioni dei richiedenti e il modulo adattivo per recuperare il modulo adattivo salvato
feature: Moduli adattivi
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Sviluppo
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 1%

---


# Creare il modulo adattivo principale

Il modulo **StoreAFWithAttachments** è il modulo adattivo principale. Questo modulo adattivo è il punto di ingresso del caso d’uso. In questo modulo vengono acquisiti i dettagli utente, compreso il numero di cellulare. Questo modulo ha anche la possibilità di aggiungere alcuni allegati. Quando si fa clic sul pulsante Save and Exit , il codice lato server viene eseguito per memorizzare i dati del modulo nel database e viene generato un ID applicazione univoco che viene presentato all’utente per la custodia sicura. Questo ID applicazione verrà utilizzato per recuperare il numero di cellulare associato all&#39;applicazione.

![modulo di applicazione principale](assets/6552.JPG)

Questo modulo è associato alle librerie client **bootboxjs540,storeAFWithAttachments** create in precedenza nel corso e a un flusso di lavoro AEM che viene attivato all’invio del modulo.


* I moduli di esempio si basano su [modello di modulo adattivo personalizzato](assets/custom-template-with-page-component.zip) che è necessario importare in AEM affinché il rendering dei moduli di esempio sia corretto.

* Il [StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip) completato può essere scaricato e importato nella tua istanza AEM.

* Affinché il modulo funzioni, è necessario importare nell’istanza AEM il flusso di lavoro [AEM associato a questo modulo](assets/workflow-model-store-af-with-attachments.zip).



