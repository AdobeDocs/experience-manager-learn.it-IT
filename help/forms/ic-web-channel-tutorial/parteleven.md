---
title: Configurazione del pannello Investimenti misti
seo-title: Configurazione del pannello Investimenti misti
description: Questa è la parte 11 di esercitazione multistep per la creazione del primo documento di comunicazione interattiva.In questa parte, verranno aggiunti grafici a torta per visualizzare il mix di investimento corrente e modello.
seo-description: Questa è la parte 11 di esercitazione multistep per la creazione del primo documento di comunicazione interattiva.In questa parte, verranno aggiunti grafici a torta per visualizzare il mix di investimento corrente e modello.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---


# Configurazione del pannello Investimenti misti

In questa parte, aggiungeremo grafici a torta per visualizzare il mix di investimento attuale e modello.

* Accedete a  AEM Forms e andate a Adobe Experience Manager > Forms > Forms &amp; Documents.

* Aprite la cartella 401KStatement.

* Aprite l&#39;istruzione 401KS in modalità di modifica.

* Aggiungeremo 2 grafici a torta per rappresentare l&#39;attuale e modello mix di investimenti del titolare del conto.

## Miscela risorsa corrente {#current-asset-mix}

* Toccate il pannello &quot;CurrentAssetMix&quot; a destra, selezionate l’icona &quot;+&quot; e inserite il componente di testo. Cambia il testo predefinito in &quot;Miscela risorsa corrente&quot;.

* Toccate il pannello &quot;CurrentAssetMix&quot;, selezionate l’icona &quot;+&quot; e inserite il componente grafico. Toccate il componente del grafico appena inserito e fate clic sull&#39;icona &quot;chiave inglese&quot; per aprire il foglio delle proprietà di configurazione per il grafico.

* Impostate le proprietà come illustrato nell&#39;immagine seguente. Accertatevi che il tipo di grafico sia a torta.

* Tenere presente l&#39;oggetto modello dati associato agli assi X e Y. È necessario selezionare l&#39;elemento principale del modello dati del modulo, quindi eseguire il drill-down per selezionare l&#39;elemento appropriato.

* ![currentassetmix](assets/currentassetmixchart.png)

## Miscela risorse modello {#model-asset-mix}

* Toccate il pannello &quot;ConsigliatoAssetMix&quot; a destra, selezionate l’icona &quot;+&quot; e inserite il componente di testo. Modificate il testo predefinito in &quot;Model Asset Mix&quot;.

* Toccate il pannello &quot;ConsigliatoAssetMix&quot;, selezionate l’icona &quot;+&quot; e inserite il componente grafico. Toccate il componente del grafico appena inserito e fate clic sull&#39;icona &quot;chiave inglese&quot; per aprire il foglio delle proprietà di configurazione per il grafico.

* Impostate le proprietà come illustrato nell&#39;immagine seguente. Accertatevi che il tipo di grafico sia a torta.

* Tenere presente l&#39;oggetto modello dati associato agli assi X e Y. È necessario selezionare l&#39;elemento principale del modello dati del modulo, quindi eseguire il drill-down per selezionare l&#39;elemento appropriato.

* ![assettype](assets/modelassettypechart.png)

