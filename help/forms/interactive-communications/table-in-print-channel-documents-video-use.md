---
title: Utilizzo del componente Tabella nel documento del canale di stampa AEM Forms
seo-title: Using Table Component in AEM Forms Print Channel Document
description: Il video seguente illustra i passaggi necessari per utilizzare il componente tabella nelle comunicazioni interattive per la stampa dei documenti del canale.
feature: Interactive Communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# Utilizzo del componente Tabella nel documento del canale di stampa AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

Il video seguente illustra i passaggi necessari per utilizzare il componente tabella nelle comunicazioni interattive per la stampa dei documenti del canale.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Le tabelle vengono utilizzate per visualizzare i dati in modo tabulare. Le righe della tabella devono crescere o ridursi a seconda dei dati restituiti dall’origine dati. Per utilizzare una tabella nel documento del canale di stampa, è necessario creare un file di layout (file xdp) utilizzando AEM Forms Designer. In questo file di layout viene aggiunta la tabella con il numero di colonne richiesto. Assicurarsi che il tipo di oggetto campo colonna sia TextField o Numeric Field a seconda delle esigenze. Per ciascuna colonna, assicurarsi che il binding dei dati sia impostato su Usa nome.

>[!NOTE]
>
>Per rendere la tabella dinamica, assicurarsi di aver contrassegnato la riga come ripetuta.

**Prova sul tuo server**

* [Scarica e decomprimi il file di risorse sul disco rigido](assets/usingtablesinprintchannel.zip)

* Importare i due file zip in AEM utilizzando il gestore dei pacchetti

* Le risorse associate a questo articolo sono le seguenti:

   * Frammento layout

   * Modale dati modulo

   * Documento di comunicazione interattiva
   * sampleretirementaccountdata.json

* Apri il documento di comunicazione interattiva in [modalità di modifica](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Aggiungere il frammento di layout TabellaDemo alla sezione contributi.
* Eseguire un binding delle celle della tabella con gli elementi del modello dati modulo appropriati, come illustrato nel video

* Anteprima del documento di comunicazione interattiva con il file di dati json di esempio fornito all’utente
