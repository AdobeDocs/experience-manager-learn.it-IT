---
title: Grafici Multi Series in AEM Forms
description: Crea un modello di dati modulo appropriato per creare grafici a più serie nei documenti stampati e nei documenti dei canali web.
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
duration: 430
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Grafici Multi Series

In AEM Forms 6.5 è stata introdotta la possibilità di creare e configurare grafici a più serie. I grafici a serie multiple vengono in genere utilizzati in associazione con il tipo di grafico a linee, a barre o a colonne. Il grafico seguente è un buon esempio di grafico a serie multiple. Il grafico mostra la crescita di 10.000 dollari in 3 diversi fondi comuni di investimento in un periodo di tempo. Per poter creare e utilizzare grafici di questo tipo in AEM Forms, è necessario creare il modello dati modulo appropriato.

![Grafico a più serie](assets/series_charts.png)

Per creare grafici a serie multiple in AEM Forms, è necessario creare un modello dati modulo appropriato con le entità e le associazioni necessarie. La schermata seguente evidenzia le entità e le associazioni tra le 3 entità. Al livello superiore, abbiamo un&#39;entità denominata &quot;Organizzazione&quot;, che ha un&#39;associazione uno-a-molti con l&#39;entità del Fondo. L&#39;entità fondo, a sua volta, ha un&#39;associazione uno-a-molti con l&#39;entità performance.

![Modello dati modulo](assets/form_data_model.png)

## Crea modello dati modulo per grafici a più serie

>[!VIDEO](https://video.tv.adobe.com/v/329213?quality=12&learn=on&captions=ita)

### Configurare i grafici a linee

>[!VIDEO](https://video.tv.adobe.com/v/329214?quality=12&learn=on&captions=ita)

Per eseguire il test sul sistema, attieniti alla seguente procedura

* [Scarica e importa MutualFundFactSheet.zip utilizzando Gestione pacchetti di AEM.](assets/mutualfundfactsheet.zip)
* [Scarica SeriesChartSampleData.json sul disco rigido.](assets/serieschartsampledata.json) Dati di esempio utilizzati per popolare il grafico.
* [Passare a Forms e documenti.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Selezionare delicatamente il modello di comunicazione interattiva &quot;MutualGrowthFactSheet&quot;.
* Fai clic sull’anteprima | Stampa canale | Carica dati di esempio.
* Individua il file di dati di esempio fornito come parte di questo articolo.
* Visualizzare in anteprima il canale di stampa della comunicazione interattiva &quot;MutualGrowthFactSheet&quot; con i dati di esempio scaricati nel passaggio precedente.
