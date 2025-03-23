---
title: Visualizzare più documenti PDF
description: Consente di scorrere più documenti PDF in un modulo adattivo.
version: Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 66
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 1%

---

# Visualizzare più documenti PDF in un carosello

Un caso d’uso comune consiste nel mostrare più documenti PDF alla funzione di compilazione del modulo per rivederli prima di inviarli.

Per eseguire questo caso d&#39;uso è stata utilizzata l&#39;[API di incorporamento Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Qui è possibile trovare una demo live di questo esempio.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

Per completare l’integrazione sono stati eseguiti i seguenti passaggi

## Creare un componente personalizzato per visualizzare più documenti PDF

È stato creato un componente personalizzato (pdf-carosello) per scorrere i documenti pdf

## Libreria client

È stata creata una libreria client per visualizzare i PDF utilizzando l’API di incorporamento di Adobe PDF. I PDF da visualizzare sono specificati nei componenti pdf-carosello.

## Creare un modulo adattivo

Crea un modulo adattivo basato su alcune schede (in questo esempio sono presenti 3 schede)
Aggiungi alcuni componenti per moduli adattivi nelle prime due schede
Aggiungi il componente Carosello pdf nella terza scheda
Configura il componente pdf-carosello come illustrato nella schermata seguente
![carosello-pdf](assets/pdf-carousel-af-component.png)

**Incorpora chiave API PDF**: è la chiave che puoi usare per incorporare il pdf. Questa chiave funziona solo con localhost. Puoi creare [la tua chiave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associarla ad un altro dominio.

**Specificare i documenti di PDF** - Qui è possibile specificare i documenti PDF che si desidera visualizzare nel carosello.


## Distribuire l’esempio sul server

Per eseguire il test sul server locale, eseguire la procedura seguente:

1. [Importa la libreria client](assets/pdf-carousel-client-lib.zip) nell&#39;istanza AEM locale [tramite Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importa il componente carosello pdf](assets/pdf-carousel-component.zip) nell&#39;istanza AEM locale [tramite Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importa il modulo adattivo](assets/adaptive-form-pdf-carousel.zip) nell&#39;istanza AEM locale [tramite Gestione pacchetti](http://localhost:4502/crx/packmgr/index.jsp)
1. [Importa i pdf di esempio da visualizzare](assets/pdf-carousel-sample-documents.zip) nell&#39;istanza AEM locale [utilizzando il collegamento per il caricamento dei file delle risorse](http://localhost:4502/assets.html/content/dam)
1. [Anteprima modulo adattivo](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Passare alla scheda Documenti da rivedere. Dovresti visualizzare tre documenti PDF nel componente Carosello.
