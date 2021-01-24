---
title: Creazione di Forms per la firma
description: Creare moduli da includere nel pacchetto di firma.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# Creazione di moduli per la firma

Il passaggio successivo consiste nel creare i moduli adattivi da includere nel pacchetto. Durante la creazione di moduli per la firma, tenere presente quanto segue:

* Assicurarsi che i moduli siano basati sul modello **SignMultipleForms**. In questo modo i moduli vengono precompilati con i dati recuperati dal database.

* I moduli devono essere configurati per utilizzare  Adobe Sign e il campo del firmatario1 deve essere associato al campo E-mail del cliente
* I moduli devono essere associati anche a clientLib denominato **getnextform**
* I moduli devono utilizzare il componente Passaggio firma.
* Il modulo deve inoltre utilizzare il componente **Firma multipla** personalizzato. Questo componente permette di passare al modulo successivo per accedere al pacchetto.
* L&#39;invio del modulo deve essere configurato per attivare AEM flusso di lavoro **Aggiorna stato firma**
* Assicurarsi che il percorso del file di dati sia impostato su **Data.xml**. Ciò è molto importante in quanto il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

Dopo aver creato il modulo, includere nel modulo i **campi comuni** frammenti di modulo adattivo. Il frammento sarà contrassegnato come nascosto. Questo frammento contiene i campi seguenti.

* **firmato**  - Campo che contiene lo stato della firma
* **guid**  - Identificatore univoco per identificare il modulo nel pacchetto
* **customerEmail**  - Questo campo contiene l&#39;e-mail del cliente



>[!NOTE]
>Se si desidera trasportare i dati da un modulo all&#39;altro del pacchetto, accertarsi che i campi del modulo siano denominati in modo identico in tutti i moduli.

## Tutti i moduli completati

Una volta che tutti i moduli del pacchetto sono stati compilati e firmati, è necessario visualizzare il messaggio appropriato. Questo messaggio viene visualizzato con l&#39;aiuto del modulo Alldone. Il modulo Alldone è incluso nei moduli di esempio.

## Assets

I moduli di esempio, inclusi quelli utilizzati in questa esercitazione, possono essere [scaricati da qui](assets/forms-for-signing.zip)
