---
title: Configurazione del pannello di Outlook per la pensione
seo-title: Configurazione del pannello di Outlook per la pensione
description: Questa è la parte 10 di un'esercitazione con più passaggi per creare il primo documento di comunicazione interattiva. In questa parte, configureremo il Pannello di Outlook di pensione aggiungendo testo e componenti grafico.
seo-description: Questa è la parte 10 di un'esercitazione con più passaggi per creare il primo documento di comunicazione interattiva. In questa parte, configureremo il Pannello di Outlook di pensione aggiungendo testo e componenti grafico.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
translation-type: tm+mt
source-git-commit: f6a9c0522219614b87c483504c9c64875f2e4286
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---


# Configurazione del pannello di Outlook per la pensione{#configuring-retirement-outlook-panel}

* Questa è la parte 10 di un&#39;esercitazione con più passaggi per creare il primo documento di comunicazione interattiva. In questa parte, configureremo il Pannello di Outlook di pensione aggiungendo testo e componenti grafico.

* Accedete a  AEM Forms e andate a Adobe Experience Manager > Forms > Forms &amp; Documents.

* Aprite la cartella 401KStatement.

* Aprite il documento 401KStatement in modalità di modifica.

**Configurare l&#39;area di destinazione del pannello sinistro**

* Toccate l’area di destinazione del pannello sinistro sul lato destro e fate clic sull’icona &quot;+&quot; per visualizzare la finestra di dialogo Inserisci componente.

* Inserisci componente Testo.

* Toccate delicatamente il componente di testo appena aggiunto per visualizzare la barra degli strumenti del componente

* Selezionate l&#39;icona &quot;matita&quot; per modificare il testo predefinito.

* Sostituire il testo predefinito con &quot;**Prospettive del reddito pensionabile&quot;**

**Configurare l&#39;area di destinazione del pannello destro**

* Toccate l’area di destinazione del pannello destro sul lato destro e fate clic sull’icona &quot;+&quot; per visualizzare la finestra di dialogo Inserisci componente.

* Inserisci componente Testo.

* Toccate delicatamente il componente di testo appena aggiunto per visualizzare la barra degli strumenti del componente.

* Selezionate l&#39;icona &quot;matita&quot; per modificare il testo predefinito.

* Sostituisce il testo predefinito con &quot;**Stima reddito mensile&quot;**

## Aggiungi frammento di documento di Outlook reddito pensione {#add-retirement-income-outlook-document-fragment}

* Fai clic sull’icona Risorse e applica il filtro per visualizzare le risorse di tipo &quot;Frammenti documento&quot;. Trascinare il frammento di documento RetirementIncomeOutlook nell&#39;area di destinazione del pannello sinistro.

* È possibile fare riferimento a [questa pagina](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/9.html) quando si aggiunge un frammento di documento alle aree contenuto.

## Aggiunta della figura del reddito mensile stimato {#adding-estimated-monthly-income-chart}

* Fate clic sull’area di destinazione del pannello destro sul lato destro. Fare clic sull&#39;icona &quot;+&quot; per inserire il componente del grafico. Utilizzeremo un grafico a colonne per visualizzare il reddito mensile stimato. Toccate delicatamente il componente grafico appena inserito. Seleziona l&#39;icona &quot;chiave inglese&quot; per aprire il foglio delle proprietà di configurazione.Configura il grafico con le seguenti proprietà come mostrato nella schermata seguente.

**AEM Forms 6.4 - Configurazione della colonna Stima del reddito mensile**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configurazione della colonna Stima del reddito mensile**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




