---
title: Creazione di layout a due colonne per i documenti del canale di stampa
description: Crea layout a 2 colonne per documento canale di stampa
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
duration: 64
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# Layout a due colonne nel documento del canale di stampa

Questo breve articolo illustra i passaggi necessari per creare il layout a 2 colonne nel canale di stampa. Il caso d’uso prevede la generazione di documenti a 2 pagine con una pagina 1 con layout a 2 colonne e una pagina 2 con layout a 1 colonne standard.

Di seguito sono riportati i passaggi di alto livello per la creazione di layout a 2 colonne con AEM Forms Designer.

* Crea 2 aree contenuto nella pagina 1 pagina master
* Denomina le 2 aree di contenuto &quot;leftcolumn&quot; e &quot;right tcolumn&quot;
* Crea una seconda pagina master con un&#39;area di contenuto (impostazione predefinita)
* Selezionare la scheda di impaginazione (Sottomaschera senza titolo) (pagina 1) e (Sottomaschera senza titolo) (pagina 2) e impostare le proprietà come mostrato nelle schermate seguenti.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Una volta impostate le proprietà di impaginazione, è possibile aggiungere sottomaschere o aree di destinazione in (Sottomaschera senza titolo) (pagina 1).

È quindi possibile aggiungere frammenti di documento a questi sottomaschere o aree di destinazione. Quando la colonna sinistra è piena, il contenuto scorre fino alla colonna destra.

Per eseguire il test sul server locale, scarica le risorse correlate a questo articolo. Scorri verso il basso fino alla parte inferiore della pagina

* [Scarica e installa Sample Print Channel Document utilizzando Gestione pacchetti](assets/print-channel-with-two-column-layout.zip)
* [Anteprima del documento Canale di stampa](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
