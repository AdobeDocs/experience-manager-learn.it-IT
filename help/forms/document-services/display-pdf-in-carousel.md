---
title: Visualizzare più documenti pdf
description: Scorri più documenti pdf in un modulo adattivo.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
source-git-commit: cb5b3eb77a57fa8a2918710b7dbcd1b0a58b74bd
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 4%

---

# Visualizzare più documenti pdf in un carosello

Un caso d’uso comune consiste nella visualizzazione di più documenti PDF al compilatore da esaminare prima di inviare il modulo.

Per eseguire questo caso d’uso abbiamo utilizzato il [API di incorporamento Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Una demo dal vivo di questo campione può essere sperimentata qui.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

Sono stati eseguiti i seguenti passaggi per completare l’integrazione

## Creare un componente personalizzato per visualizzare più documenti PDF

È stato creato un componente personalizzato (pdf-carosello) per scorrere i documenti pdf

## Libreria client

È stata creata una libreria client per visualizzare i PDF utilizzando l’API di incorporamento di Adobe PDF. I PDF da visualizzare sono specificati nei componenti pdf-carosello.

## Creare un modulo adattivo

Crea un modulo adattivo basato su alcune schede (Questo esempio ha 3 schede) Aggiungi alcuni componenti modulo adattivo nelle prime due schede Aggiungi il componente carosello pdf nella terza scheda Configura il componente pdf-carosello come mostrato nella schermata sottostante
![pdf-carosello](assets/pdf-carousel-af-component.png)

**Incorpora la chiave API di PDF** - Questa è la chiave che è possibile utilizzare per incorporare il pdf. Questa chiave funziona solo con localhost. Puoi creare [la tua chiave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associarlo ad un altro dominio.

**Specificare i documenti PDF** - Qui è possibile specificare i documenti pdf che si desidera visualizzare nel carosello.


## Distribuire l&#39;esempio sul server

Per eseguire il test sul server locale, segui i passaggi seguenti:

1. [Importare la libreria client](assets/pdf-carousel-client-lib.zip) nell&#39;istanza AEM locale [utilizzo del gestore dei pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importare il componente carosello pdf](assets/pdf-carousel-component.zip) nell&#39;istanza AEM locale [utilizzo del gestore dei pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importare il modulo adattivo ](assets/adaptive-form-pdf-carousel.zip) nell&#39;istanza AEM locale [utilizzo del gestore dei pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importare i pdf di esempio da visualizzare](assets/pdf-carousel-sample-documents.zip) nell&#39;istanza AEM locale [utilizzo del collegamento di caricamento del file di risorse](http://localhost:4502/assets.html/content/dam)
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Passare alla scheda Documenti da esaminare. Dovresti visualizzare tre documenti PDF nel componente carosello .
