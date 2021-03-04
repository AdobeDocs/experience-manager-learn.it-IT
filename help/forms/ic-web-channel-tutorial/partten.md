---
title: Configurazione del pannello di Outlook per la ritirata
seo-title: Configurazione del pannello di Outlook per la ritirata
description: Questa è la parte 10 di un tutorial in più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, configureremo il pannello di Outlook Retirement aggiungendo testo e componenti grafico.
seo-description: Questa è la parte 10 di un tutorial in più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, configureremo il pannello di Outlook Retirement aggiungendo testo e componenti grafico.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Comunicazione interattiva
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 1%

---


# Configurazione del pannello di Outlook per la ritirata{#configuring-retirement-outlook-panel}

* Questa è la parte 10 di un tutorial in più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, configureremo il pannello di Outlook Retirement aggiungendo testo e componenti grafico.

* Accedi a AEM Forms e passa a Adobe Experience Manager > Moduli > Moduli e documenti.

* Apri la cartella 401KStatement .

* Apri il documento 401KStatement in modalità di modifica.

**Configurare l’area di destinazione del pannello sinistro**

* Toccare l’area di destinazione del pannello sinistro sul lato destro e fare clic sull’icona &quot;+&quot; per visualizzare la finestra di dialogo inserisci componente.

* Inserisci componente Testo .

* Tocca delicatamente il componente di testo appena aggiunto per visualizzare la barra degli strumenti del componente

* Seleziona l’icona &quot;matita&quot; per modificare il testo predefinito.

* Sostituisci il testo predefinito con &quot;**Prospettive sul reddito da pensione&quot;**

**Configurare l’area di destinazione del pannello destro**

* Toccare l’area di destinazione del pannello destro sul lato destro e fare clic sull’icona &quot;+&quot; per visualizzare la finestra di dialogo inserisci componente.

* Inserisci componente Testo .

* Tocca delicatamente il componente di testo appena aggiunto per visualizzare la barra degli strumenti del componente.

* Seleziona l’icona &quot;matita&quot; per modificare il testo predefinito.

* Sostituisci il testo predefinito con &quot;**Reddito mensile stimato&quot;**

## Aggiungi frammento di documento di Outlook per la pensione {#add-retirement-income-outlook-document-fragment}

* Fai clic sull’icona Risorse e applica il filtro per visualizzare le risorse di tipo &quot;Frammenti documento&quot;. Trascinare il frammento di documento RetirementIncomeOutlook sull&#39;area di destinazione del pannello a sinistra.

* È possibile fare riferimento a [a questa pagina](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/9.html) all&#39;aggiunta di frammenti di documento alle aree contenuto.

## Aggiunta della tabella del reddito mensile stimato {#adding-estimated-monthly-income-chart}

* Fai clic sull’area di destinazione del pannello destro sul lato destro. Fai clic sull’icona &quot;+&quot; per inserire il componente grafico. Utilizzeremo un grafico a colonne per visualizzare il reddito mensile stimato. Tocca delicatamente il nuovo componente grafico inserito. Seleziona l&#39;icona &quot;Chiave&quot; per aprire il foglio delle proprietà di configurazione.Configura il grafico con le seguenti proprietà come mostrato nella schermata sottostante.

**AEM Forms 6.4 - Configurazione della colonna del reddito mensile stimato**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configurazione della colonna del reddito mensile stimato Grafico**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




