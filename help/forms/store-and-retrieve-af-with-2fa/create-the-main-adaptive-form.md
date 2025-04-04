---
title: Creare il modulo adattivo principale
description: Creare i moduli adattivi per acquisire le informazioni sui richiedenti e i moduli adattivi per recuperare il modulo adattivo salvato
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 41
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Creare il modulo adattivo principale

Il modulo **StoreAFWithAttachments** è il modulo adattivo principale. Questo modulo adattivo è il punto di ingresso per il caso d’uso. In questo modulo vengono acquisiti i dettagli utente, incluso il numero di cellulare. Questo modulo consente inoltre di aggiungere alcuni allegati. Quando si fa clic sul pulsante Salva ed esci, viene eseguito il codice lato server per memorizzare i dati del modulo nel database e viene generato un ID applicazione univoco che viene presentato all’utente per la conservazione sicura. Questo ID applicazione viene utilizzato per recuperare il numero di cellulare associato all’applicazione.

![modulo applicazione principale](assets/6552.JPG)

Questo modulo è associato a **bootboxjs540,storeAFWithAttachments** librerie client create in precedenza nel corso e a un flusso di lavoro AEM che viene attivato all&#39;invio del modulo.


* I moduli di esempio si basano su [modello di modulo adattivo personalizzato](assets/custom-template-with-page-component.zip) che deve essere importato in AEM affinché i moduli di esempio vengano riprodotti correttamente.

* Il [Modulo StoreAfWithAttachments completato](assets/store-af-with-attachments-form.zip) può essere scaricato e importato nell&#39;istanza AEM.

* Il flusso di lavoro di [AEM associato a questo modulo](assets/workflow-model-store-af-with-attachments.zip) deve essere importato nell&#39;istanza di AEM affinché il modulo funzioni.


## Passaggi successivi

[Creare il modulo recuperando il modulo salvato](./retrieve-saved-form.md)