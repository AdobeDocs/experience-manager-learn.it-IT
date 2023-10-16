---
title: Distribuire le risorse di esempio
description: Distribuisci le risorse di esempio nel sistema locale predisposto per il cloud.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Distribuire le risorse di esempio

Per utilizzare questo caso d’uso nel sistema, distribuisci le seguenti risorse sul sistema locale predisposto per il cloud.

* Assicurati di aver creato tutte le configurazioni/gli account richiesti elencati nella[documento introduttivo](./introduction.md)

* [Installare il modello di modulo adattivo personalizzato e il componente pagina associato](./assets/azure-portal-template-page-component.zip)

* [Installare il modello dati del modulo SendGrid](./assets/send-grid-form-data-model.zip). Per il corretto funzionamento di questo modello dati modulo, è necessario fornire la chiave API e l’indirizzo del mittente della verifica SendGrid. Testare il modello dati modulo nell’editor modelli dati modulo

* [Installare il modello dati del modulo di Azure supportato](./assets/azure-storage-fdm.zip). Per il funzionamento del modello dati del modulo è necessario fornire le credenziali dell’account di archiviazione Azure. Verifica il modello dati del modulo nell’editor modelli dati modulo.

* [Importare il modulo adattivo di esempio](./assets/credit-applications-af.zip)
* [Importare la libreria client](./assets/client-lib.zip)
* [Visualizzare l’anteprima del modulo](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). Immettere un messaggio di posta elettronica valido e fare clic sul pulsante Salva. I dati del modulo devono essere archiviati nell’archiviazione di Azure e un messaggio di posta elettronica con un collegamento al modulo salvato verrà inviato all’indirizzo di posta elettronica specificato.
