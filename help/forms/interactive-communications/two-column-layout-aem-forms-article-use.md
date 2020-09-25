---
title: Creazione di due layout di colonna per i documenti del canale di stampa
seo-title: Creazione di due layout di colonna per i documenti del canale di stampa
description: Creare 2 layout di colonna per il documento del canale di stampa
seo-description: Creare 2 layout di colonna per il documento del canale di stampa
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 314f798f7a80f9c554e5bea052f8a64ae397d0de
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# Due colonne di layout nel documento del canale di stampa

Questo breve articolo evidenzia i passaggi necessari per creare il layout a 2 colonne nel canale di stampa. Il caso d’uso è quello di generare documenti di due pagine con pagina 1 con layout a due colonne e pagina 2 con il layout a 1 colonna standard.

Di seguito sono riportati i passaggi di alto livello per la creazione di layout a due colonne con  AEM Forms Designer.

* Creare 2 aree contenuto nella pagina master 1
* Denominate le 2 aree contenuto &quot;colonna a sinistra&quot; e &quot;colonna a destra&quot;
* Creare una seconda pagina master con un&#39;area contenuto (questa è l&#39;impostazione predefinita)
* Selezionare la scheda Impaginazione (Sottomodulo senza titolo) (pagina 1) e (Sottomodulo senza titolo) (pagina 2) e impostare le proprietà come mostrato nelle schermate sottostanti.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Una volta impostate le proprietà di impaginazione, è possibile aggiungere sottomoduli o aree di destinazione in (Sottomodulo senza titolo) (Pagina 1).

È quindi possibile aggiungere frammenti di documento a tali sottomoduli o aree di destinazione. Quando la colonna a sinistra è piena, il contenuto scorre verso la colonna a destra.

Per eseguire il test sul server locale, scaricate le risorse correlate a questo articolo. Scorri verso il basso fino alla parte inferiore della pagina

* [Download e installazione di Sample Print Channel Document tramite il gestore pacchetti](assets/print-channel-with-two-column-layout.zip)
* [Anteprima del documento del canale di stampa](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
