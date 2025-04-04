---
title: Configurazione del pannello Investment Mix
description: Questa è la parte 11 di un tutorial a più passaggi per creare il tuo primo documento di comunicazione interattiva.In questa parte, aggiungeremo dei grafici a torta per visualizzare il mix di investimenti attuale e di modello.
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 69
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# Configurazione del pannello Investment Mix

In questa parte verranno aggiunti grafici a torta per visualizzare il mix di investimenti corrente e modello.

* Accedi ad AEM Forms e passa a Adobe Experience Manager > Forms > Forms &amp; Documents.

* Aprire la cartella 401KStatement.

* Aprire 401KStatement in modalità di modifica.

* Aggiungeremo 2 grafici a torta per rappresentare la combinazione di investimenti corrente e modello del titolare del conto.

## Mix di risorse corrente {#current-asset-mix}

* Tocca il pannello &quot;CurrentAssetMix&quot; a destra, quindi seleziona l’icona &quot;+&quot; e inserisci il componente testo. Impostate il testo predefinito su &quot;Current Asset Mix&quot;.

* Tocca il pannello &quot;CurrentAssetMix&quot;, seleziona l’icona &quot;+&quot; e inserisci il componente grafico. Tocca il componente grafico appena inserito e fai clic sull’icona &quot;chiave inglese&quot; per aprire la finestra delle proprietà di configurazione per il grafico.

* Imposta le proprietà come mostrato nell’immagine seguente. Assicurarsi che il tipo di grafico sia Grafico a torta.

* Tieni presente l’oggetto modello dati associato agli assi X e Y. È necessario selezionare l’elemento principale del modello dati del modulo, quindi eseguire il drill-down per selezionare l’elemento appropriato.

* ![currentassetmix](assets/currentassetmixchart.png)

## Mix risorse modello {#model-asset-mix}

* Tocca il pannello &quot;RecommendedAssetMix&quot; a destra, quindi seleziona l’icona &quot;+&quot; e inserisci il componente testo. Impostate il testo predefinito su &quot;Model Asset Mix&quot;.

* Tocca il pannello &quot;RecommendedAssetMix&quot;, seleziona l’icona &quot;+&quot; e inserisci il componente del grafico. Tocca il componente grafico appena inserito e fai clic sull’icona &quot;chiave inglese&quot; per aprire la finestra delle proprietà di configurazione per il grafico.

* Imposta le proprietà come mostrato nell’immagine seguente. Assicurarsi che il tipo di grafico sia Grafico a torta.

* Tieni presente l’oggetto modello dati associato agli assi X e Y. È necessario selezionare l’elemento principale del modello dati del modulo, quindi eseguire il drill-down per selezionare l’elemento appropriato.

* ![assettype](assets/modelassettypechart.png)

## Passaggi successivi

[Preparare la consegna del documento del canale web](./parttwelve.md)
