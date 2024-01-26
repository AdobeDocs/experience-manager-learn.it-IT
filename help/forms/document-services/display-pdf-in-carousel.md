---
title: Visualizzare più documenti PDF
description: Consente di scorrere più documenti PDF in un modulo adattivo.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 84
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 1%

---

# Visualizzare più documenti PDF in un carosello

Un caso d’uso comune prevede la visualizzazione di più documenti PDF alla casella di compilazione del modulo, da esaminare prima dell’invio del modulo.

Per eseguire questo caso d’uso abbiamo utilizzato [API di incorporamento Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Qui puoi trovare una demo live di questo esempio.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

Per completare l’integrazione sono stati eseguiti i seguenti passaggi

## Creare un componente personalizzato per visualizzare più documenti PDF

È stato creato un componente personalizzato (pdf-carosello) per scorrere i documenti pdf

## Libreria client

È stata creata una libreria client per visualizzare i PDF mediante l’API di incorporamento di Adobe PDF. I PDF da visualizzare sono specificati nei componenti pdf-carosello.

## Creare un modulo adattivo

Creare un modulo adattivo basato su alcune schede (questo esempio contiene 3 schede) Aggiungere alcuni componenti del modulo adattivo nelle prime due schede Aggiungere il componente Carosello pdf nella terza scheda Configurare il componente Carosello pdf come mostrato nella schermata seguente
![pdf-carosello](assets/pdf-carousel-af-component.png)

**Incorpora chiave API PDF** : questa è la chiave che puoi utilizzare per incorporare il PDF. Questa chiave funziona solo con localhost. Puoi creare [la tua chiave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associarlo ad un altro dominio.

**Specifica documenti PDF** - Qui puoi specificare i documenti pdf da visualizzare nel carosello.


## Distribuire l’esempio sul server

Per eseguire il test sul server locale, eseguire la procedura seguente:

1. [Importare la libreria client](assets/pdf-carousel-client-lib.zip) nell’istanza AEM locale [utilizzo del gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importare il componente carosello pdf](assets/pdf-carousel-component.zip) nell’istanza AEM locale [utilizzo del gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importare il modulo adattivo](assets/adaptive-form-pdf-carousel.zip) nell’istanza AEM locale [utilizzo del gestore di pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importa i PDF di esempio da visualizzare](assets/pdf-carousel-sample-documents.zip) nell’istanza AEM locale [utilizzo del collegamento di caricamento file delle risorse](http://localhost:4502/assets.html/content/dam)
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Passare alla scheda Documenti da rivedere. Dovresti visualizzare tre documenti PDF nel componente Carosello.
