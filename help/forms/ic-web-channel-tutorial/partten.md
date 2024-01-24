---
title: Configurazione del pannello di Outlook smobilizzo
description: Questa è la parte 10 di un tutorial in più passaggi per creare il tuo primo documento di comunicazione interattiva. In questa parte verrà configurato il pannello di Outlook per il ritiro aggiungendo testo e componenti del grafico.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
duration: 82
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# Configurazione del pannello di Outlook smobilizzo{#configuring-retirement-outlook-panel}

* Questa è la parte 10 di un tutorial in più passaggi per creare il tuo primo documento di comunicazione interattiva. In questa parte verrà configurato il pannello di Outlook per il ritiro aggiungendo testo e componenti del grafico.

* Accedi ad AEM Forms e passa a Adobe Experience Manager > Forms > Forms &amp; Documents.

* Aprire la cartella 401KStatement.

* Aprire il documento 401KStatement in modalità di modifica.

**Configura area di destinazione LeftPanel**

* Tocca l’area di destinazione LeftPanel a destra, quindi fai clic sull’icona &quot;+&quot; per visualizzare la finestra di dialogo Inserisci componente.

* Inserisci componente testo.

* Tocca delicatamente il componente testo appena aggiunto per visualizzare la barra degli strumenti del componente

* Seleziona l’icona &quot;matita&quot; per modificare il testo predefinito.

* Sostituisci il testo predefinito con &quot;**Il tuo reddito di pensione Outlook&quot;**

**Configura area di destinazione RightPanel**

* Tocca l’area di destinazione RightPanel sul lato destro e fai clic sull’icona &quot;+&quot; per visualizzare la finestra di dialogo Inserisci componente.

* Inserisci componente testo.

* Tocca delicatamente il componente testo appena aggiunto per visualizzare la barra degli strumenti del componente.

* Seleziona l’icona &quot;matita&quot; per modificare il testo predefinito.

* Sostituisci il testo predefinito con &quot;**Reddito da pensione mensile stimato&quot;**

## Aggiungi frammento documento di Outlook reddito da pensione {#add-retirement-income-outlook-document-fragment}

* Fai clic sull’icona Risorse e applica il filtro per visualizzare le risorse di tipo &quot;Frammenti di documento&quot;. Trascinare il frammento di documento RetirementIncomeOutlook nell&#39;area di destinazione del pannello sinistro.

* Puoi consultare [a questa pagina](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) quando si aggiunge un frammento di documento ad aree di contenuto.

## Grafico Ad Entrate Mensili Stimate {#adding-estimated-monthly-income-chart}

* Fare clic sull&#39;area di destinazione RightPanel sul lato destro. Fai clic sull’icona &quot;+&quot; per inserire il componente grafico. Utilizzeremo un istogramma per visualizzare il reddito mensile stimato. Tocca delicatamente il componente grafico appena inserito. Seleziona l’icona &quot;Wrench&quot; (Chiave) per aprire la finestra delle proprietà di configurazione. Configura il grafico con le seguenti proprietà come mostrato nella schermata seguente.

**AEM Forms 6.4 - Configurazione del grafico a colonne Reddito mensile stimato**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configurazione del grafico a colonne Reddito mensile stimato**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## Passaggi successivi

[Configura grafico a torta](./parteleven.md)
