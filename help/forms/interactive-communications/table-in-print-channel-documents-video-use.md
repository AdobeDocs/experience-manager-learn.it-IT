---
title: Utilizzo del componente Tabella  documento canale di stampa AEM Forms
seo-title: Utilizzo del componente Tabella  documento canale di stampa AEM Forms
description: Il seguente video illustra i passaggi necessari per utilizzare il componente Tabella nelle comunicazioni interattive per i documenti dei canali di stampa.
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Utilizzo del componente Tabella  documento canale di stampa AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

Il seguente video illustra i passaggi necessari per utilizzare il componente Tabella nelle comunicazioni interattive per i documenti dei canali di stampa.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Le tabelle vengono utilizzate per visualizzare i dati in modo tabulare. Le righe della tabella devono essere estese o ridotte in base ai dati restituiti dall&#39;origine dati. Per utilizzare una tabella nel documento del canale di stampa, è necessario creare un file di layout (file xdp) utilizzando  AEM Forms Designer. In questo file di layout, aggiungiamo la tabella con il numero di colonne richiesto. Assicurarsi che il tipo di oggetto campo colonna sia TextField o Numeric Field a seconda dei requisiti. Per ciascuna colonna, i campi devono essere certi che il binding dei dati sia impostato su Nome utente.

>[!NOTE]
Per rendere la tabella dinamica, accertarsi di aver contrassegnato la riga come ripetuta.

**Prova sul tuo server**

* [Scaricare e decomprimere il file delle risorse sul disco rigido](assets/usingtablesinprintchannel.zip)

* Importare i due file ZIP in AEM utilizzando il gestore pacchetti

* Tra le risorse associate a questo articolo sono incluse le seguenti:

   * Frammento layout

   * Dati modulo modale

   * Documento di comunicazione interattiva
   * sampleretirementaccountdata.json

* Aprite il documento di comunicazione interattiva in modalità di [modifica](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Aggiungere il frammento di layout TabellaDemo alla sezione dei contributi.
* Eseguire un binding delle celle della tabella con gli elementi del modello dati modulo appropriati, come illustrato nel video

* Anteprima documento di comunicazione interattiva con il file di dati json di esempio fornito all&#39;utente

