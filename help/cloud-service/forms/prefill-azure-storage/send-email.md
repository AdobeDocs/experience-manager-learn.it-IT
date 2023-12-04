---
title: Invia messaggio tramite SendGrid
description: Attiva un messaggio di posta elettronica con un collegamento al modulo salvato
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-8474
duration: 41
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Invia posta elettronica

Dopo il salvataggio dei dati del modulo nell’archiviazione BLOB di Azure, viene inviato all’utente un messaggio e-mail con un collegamento al modulo salvato. Questo messaggio di posta elettronica viene inviato utilizzando l&#39;API REST di SendGrid.

Il file swagger, il modello di dati del modulo e la configurazione dei servizi cloud necessari per inviare e-mail vengono forniti come parte delle risorse dell’articolo.

Sarà necessario creare un account SendGrid, un modello dinamico che possa acquisire i dati acquisiti nel modulo adattivo.


Di seguito è riportato lo snippet di codice html utilizzato nel modello dinamico. Il valore del parametro blobID viene passato utilizzando il servizio richiama del modello dati del modulo.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


