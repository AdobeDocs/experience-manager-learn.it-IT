---
title: Utilizzo del componente Tabella nel documento del canale di stampa di AEM Forms
description: Il video seguente illustra i passaggi necessari per utilizzare il componente tabella nelle comunicazioni interattive per i documenti del canale di stampa.
feature: Interactive Communication
doc-type: technical video
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
duration: 285
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Utilizzo del componente Tabella nel documento del canale di stampa di AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

Il video seguente illustra i passaggi necessari per utilizzare il componente tabella nelle comunicazioni interattive per i documenti del canale di stampa.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

Le tabelle vengono utilizzate per visualizzare i dati in modo tabulare. Le righe della tabella devono aumentare o diminuire a seconda dei dati restituiti dall&#39;origine dati. Per utilizzare una tabella in un documento del canale di stampa, è necessario creare un file di layout (file xdp) con AEM Forms Designer. In questo file di layout, aggiungiamo la tabella con il numero di colonne richiesto. Verificare che il tipo di oggetto campo colonna sia TextField o Numeric Field a seconda dei requisiti. Per ogni colonna, i campi assicurano che l&#39;associazione dati sia impostata su Usa nome.

>[!NOTE]
>
>Per rendere la tabella dinamica, assicurarsi di aver contrassegnato la riga come ripetuta.

**Prova sul tuo server**

* [Scarica e decomprimi il file delle risorse sul disco rigido](assets/usingtablesinprintchannel.zip)

* Importare i due file zip in AEM utilizzando Gestione pacchetti

* Tra le risorse associate a questo articolo sono incluse le seguenti:

   * Frammento layout

   * Modale dati modulo

   * Documento di comunicazione interattiva
   * sampleretirementaccountdata.json

* Aprire il documento di comunicazione interattiva in [modalità di modifica](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Aggiungi il frammento di layout TableDemo alla sezione Contributi.
* Associate le celle della tabella agli elementi appropriati del modello dati modulo, come mostrato nel video

* Anteprima del documento di comunicazione interattiva con il file di dati json di esempio fornito
