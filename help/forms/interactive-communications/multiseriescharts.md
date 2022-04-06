---
title: Grafici multiserie in AEM Forms
seo-title: Multi Series Charts in AEM Forms
description: Creare un modello dati modulo appropriato per creare grafici a più serie in documenti per la stampa e per i canali Web.
seo-description: Create appropriate Form Data Model to create multi series charts in print and web channel documents.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---

# Grafici multiserie

AEM Forms 6.5 ha introdotto la possibilità di creare e configurare grafici a serie multiple. I grafici a serie multiple sono generalmente utilizzati in associazione con il tipo di grafico a linee, barre, colonne. Il grafico seguente è un buon esempio di grafico a più serie. Il grafico mostra la crescita di $10.000 USD in 3 diversi fondi comuni in un periodo di tempo. Per poter creare e utilizzare grafici di questo tipo in AEM Forms, è necessario creare il modello dati modulo appropriato.

![multiserie](assets/seriescharts.jfif)

Per creare grafici multiserie in AEM Forms, è necessario creare un modello dati modulo appropriato con le entità e le associazioni necessarie tra le entità. La schermata seguente evidenzia le entità e le associazioni tra le 3 entità. Al livello più alto, abbiamo un&#39;entità chiamata &quot;Organizzazione&quot;, che ha un&#39;associazione uno-a-molti con l&#39;entità Fondo. L’entità fondo, a sua volta, ha un’associazione uno-a-molti con l’entità Performance.

![formdatamodel](assets/formdatamodel.jfif)


## Creare un modello di dati per grafici a più serie

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Configurare i grafici a serie di linee

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Per eseguire il test sul sistema, segui i seguenti passaggi

* [Scarica e importa il file MutualFundFactSheet.zip utilizzando AEM Package Manager.](assets/mutualfundfactsheet.zip)
* [Scarica SeriesChartSampleData.json sul disco rigido.](assets/serieschartsampledata.json) Si tratta dei dati di esempio che verranno utilizzati per compilare il grafico.
* [Passa a Forms e Documenti.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Selezionare delicatamente il modello di comunicazione interattiva &quot;MutualFundGrowthFactSheet&quot;.
* Fai clic su Anteprima | Canale di stampa | Carica i dati di esempio.
* Sfoglia il file di dati di esempio fornito come parte di questo articolo.
* Visualizza in anteprima il canale di stampa della comunicazione interattiva &quot;MutualFundGrowthFactSheet&quot; con i dati di esempio scaricati nel passaggio precedente.
