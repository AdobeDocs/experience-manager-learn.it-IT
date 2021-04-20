---
title: Configurazione del pannello Mix investimenti
seo-title: Configurazione del pannello Mix investimenti
description: Questa è la parte 11 dell'esercitazione multistep per la creazione del primo documento di comunicazione interattivo.In questa parte, aggiungeremo grafici a torta per visualizzare il mix di investimenti attuale e modello.
seo-description: Questa è la parte 11 dell'esercitazione multistep per la creazione del primo documento di comunicazione interattivo.In questa parte, aggiungeremo grafici a torta per visualizzare il mix di investimenti attuale e modello.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 1%

---


# Configurazione del pannello Mix investimenti

In questa parte, aggiungeremo grafici a torta per visualizzare il mix di investimenti attuale e di modelli.

* Accedi a AEM Forms e passa a Adobe Experience Manager > Moduli > Moduli e documenti.

* Apri la cartella 401KStatement .

* Apri l&#39;istruzione 401KSin modalità di modifica.

* Aggiungeremo 2 grafici a torta per rappresentare il mix di investimenti attuale e modello del titolare del conto.

## Miscela risorse corrente {#current-asset-mix}

* Tocca il pannello &quot;CurrentAssetMix&quot; a destra, seleziona l’icona &quot;+&quot; e inserisci il componente testo. Cambia il testo predefinito in &quot;Miscela risorsa corrente&quot;.

* Tocca il pannello &quot;CurrentAssetMix&quot;, seleziona l’icona &quot;+&quot; e inserisci il componente grafico. Tocca il componente grafico appena inserito e fai clic sull’icona &quot;chiave inglese&quot; per aprire il foglio delle proprietà di configurazione del grafico.

* Imposta le proprietà come mostrato nell&#39;immagine seguente. Assicurati che il tipo di grafico sia Grafico a torta.

* Si prega di notare l&#39;oggetto modello dati associato agli assi X e Y. È necessario selezionare l’elemento principale del modello dati del modulo, quindi approfondire la ricerca per selezionare l’elemento appropriato.

* ![currentassetmix](assets/currentassetmixchart.png)

## Modello Asset Mix {#model-asset-mix}

* Tocca il pannello &quot;ConsigliatoAssetMix&quot; a destra, seleziona l’icona &quot;+&quot; e inserisci il componente testo. Modificate il testo predefinito in &quot;Model Asset Mix&quot;.

* Tocca il pannello &quot;ConsigliatoAssetMix&quot; e seleziona l’icona &quot;+&quot; e inserisci il componente grafico. Tocca il componente grafico appena inserito e fai clic sull’icona &quot;chiave inglese&quot; per aprire il foglio delle proprietà di configurazione del grafico.

* Imposta le proprietà come mostrato nell&#39;immagine seguente. Assicurati che il tipo di grafico sia Grafico a torta.

* Si prega di notare l&#39;oggetto modello dati associato agli assi X e Y. È necessario selezionare l’elemento principale del modello dati del modulo, quindi approfondire la ricerca per selezionare l’elemento appropriato.

* ![assettype](assets/modelassettypechart.png)

