---
title: Grafici multi serie in  AEM Forms
seo-title: Grafici multi serie in  AEM Forms
description: Creare un modello dati modulo appropriato per creare grafici a più serie nei documenti per la stampa e i canali Web.
seo-description: Creare un modello dati modulo appropriato per creare grafici a più serie nei documenti per la stampa e i canali Web.
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Grafici multiserie

 AEM Forms 6.5 ha introdotto la possibilità di creare e configurare grafici a serie multiple. I grafici a serie multiple sono generalmente utilizzati in associazione con il tipo di grafico Linea,Barra,Colonna. Il seguente grafico è un buon esempio di grafico a più serie. Il grafico mostra la crescita di $10.000 USD in 3 diversi fondi comuni di investimento in un periodo di tempo. Per poter creare e utilizzare grafici di questo tipo in  AEM Forms, sarà necessario creare il modello dati modulo appropriato.

![multiserie](assets/seriescharts.jfif)

Per creare grafici a più serie in  AEM Forms, è necessario creare un modello dati modulo appropriato con le entità e le associazioni necessarie tra le entità. La schermata seguente evidenzia le entità e le associazioni tra le 3 entità. Al livello principale, abbiamo un&#39;entità chiamata &quot;Organizzazione&quot;, che ha un&#39;associazione uno-a-molti con l&#39;entità del Fondo. L&#39;entità del Fondo, a sua volta, ha un&#39;associazione uno-molti con l&#39;entità Performance.

![formdatamodel](assets/formdatamodel.jfif)


## Crea modello dati modulo per grafici con più serie

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Configurare i grafici serie di linee

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Per eseguire il test sul sistema, attenetevi alla seguente procedura

* [Scaricate e importate il file MutualFundFactSheet.zip tramite AEM Package Manager.](assets/mutualfundfactsheet.zip)
* [Scarica SeriesChartSampleData.json sul disco rigido.](assets/serieschartsampledata.json) Si tratta dei dati di esempio che verranno utilizzati per compilare il grafico.
* [Passa ad Forms e Documenti.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* Selezionate delicatamente il modello di comunicazione interattiva &quot;MutualFundGrowthFactSheet&quot;.
* Fate clic su Anteprima | Carica dati di esempio.
* Individuate il file di dati di esempio fornito nell&#39;articolo.
* Visualizzare in anteprima il canale di stampa della comunicazione interattiva &quot;MutualFundGrowthFactSheet&quot; con i dati di esempio scaricati nel passaggio precedente.
