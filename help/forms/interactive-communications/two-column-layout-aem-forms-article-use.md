---
title: Creazione di due layout di colonna per i documenti del canale di stampa
seo-title: Creating two column layouts for print channel documents
description: Crea 2 layout di colonna per il documento del canale di stampa
seo-description: Create 2 column layouts for print channel document
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Due layout di colonna nel documento del canale di stampa

Questo breve articolo evidenzierà i passaggi necessari per creare il layout a 2 colonne nel canale di stampa. Il caso d’uso è quello di generare 2 documenti di pagina con layout di pagina 1 a 2 colonne e a pagina 2 con layout di colonna standard 1.

Di seguito sono riportati i passaggi di alto livello relativi alla creazione di layout di due colonne utilizzando AEM Forms Designer.

* Creare 2 aree contenuto nella pagina master 1
* Denominare le 2 aree contenuto &quot;colonna a sinistra&quot; e &quot;colonna a destra&quot;
* Crea una seconda pagina master con un’area contenuto (questa è l’impostazione predefinita)
* Selezionare la scheda impaginazione (Sottomodulo senza titolo) (pagina 1) e (Sottomodulo senza titolo) (pagina 2) e impostare le proprietà come mostrato nelle schermate seguenti.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Una volta impostate le proprietà di impaginazione, è possibile aggiungere sottomoduli o aree di destinazione in (Sottomodulo senza titolo) (Pagina 1).

È quindi possibile aggiungere frammenti di documento a tali sottomoduli o aree di destinazione. Quando la colonna a sinistra è piena, il contenuto si sposta sulla colonna a destra.

Per eseguire il test sul server locale, scarica le risorse correlate a questo articolo. Scorri verso il basso fino alla parte inferiore della pagina

* [Scarica e installa il documento del canale di stampa di esempio utilizzando il gestore dei pacchetti](assets/print-channel-with-two-column-layout.zip)
* [Anteprima del documento del canale di stampa](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
