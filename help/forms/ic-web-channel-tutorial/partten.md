---
title: Configurazione del pannello di Outlook per la ritirata
seo-title: Configuring Retirement Outlook Panel
description: Questa è la parte 10 di un tutorial in più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, configureremo il pannello di Outlook Retirement aggiungendo testo e componenti grafico.
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 1%

---

# Configurazione del pannello di Outlook per la ritirata{#configuring-retirement-outlook-panel}

* Questa è la parte 10 di un tutorial in più passaggi per la creazione del primo documento di comunicazione interattiva. In questa parte, configureremo il pannello di Outlook Retirement aggiungendo testo e componenti grafico.

* Accedi ad AEM Forms e passa a Adobe Experience Manager > Forms > Forms e documenti.

* Apri la cartella 401KStatement .

* Apri il documento 401KStatement in modalità di modifica.

**Configurare l’area di destinazione del pannello sinistro**

* Toccare l’area di destinazione del pannello sinistro sul lato destro e fare clic sull’icona &quot;+&quot; per visualizzare la finestra di dialogo inserisci componente.

* Inserisci componente Testo .

* Tocca delicatamente il componente di testo appena aggiunto per visualizzare la barra degli strumenti del componente

* Seleziona l’icona &quot;matita&quot; per modificare il testo predefinito.

* Sostituisci il testo predefinito con &quot;**Il tuo reddito di pensione Outlook&quot;**

**Configurare l’area di destinazione del pannello destro**

* Toccare l’area di destinazione del pannello destro sul lato destro e fare clic sull’icona &quot;+&quot; per visualizzare la finestra di dialogo inserisci componente.

* Inserisci componente Testo .

* Tocca delicatamente il componente di testo appena aggiunto per visualizzare la barra degli strumenti del componente.

* Seleziona l’icona &quot;matita&quot; per modificare il testo predefinito.

* Sostituisci il testo predefinito con &quot;**Reddito mensile stimato di pensionamento&quot;**

## Aggiungi un frammento di documento di Outlook per il reddito da smobilizzo {#add-retirement-income-outlook-document-fragment}

* Fai clic sull’icona Risorse e applica il filtro per visualizzare le risorse di tipo &quot;Frammenti documento&quot;. Trascinare il frammento di documento RetirementIncomeOutlook sull&#39;area di destinazione del pannello a sinistra.

* Puoi fare riferimento a [a questa pagina](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) durante l&#39;aggiunta di frammenti di documento alle aree contenuto.

## Aggiunta della figura del reddito mensile stimato {#adding-estimated-monthly-income-chart}

* Fai clic sull’area di destinazione del pannello destro sul lato destro. Fai clic sull’icona &quot;+&quot; per inserire il componente grafico. Utilizzeremo un grafico a colonne per visualizzare il reddito mensile stimato. Tocca delicatamente il nuovo componente grafico inserito. Seleziona l&#39;icona &quot;Chiave&quot; per aprire il foglio delle proprietà di configurazione.Configura il grafico con le seguenti proprietà come mostrato nella schermata sottostante.

**AEM Forms 6.4 - Configurazione della colonna del reddito mensile stimato Grafico**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configurazione della colonna del reddito mensile stimato Grafico**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## Passaggi successivi

[Configura grafico a torta](./parteleven.md)
