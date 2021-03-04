---
title: Creare moduli per la firma
description: Creare moduli da includere nel pacchetto di firma.
feature: moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---


# Creazione di moduli per la firma

Il passaggio successivo consiste nel creare i moduli adattivi da includere nel pacchetto. Quando si creano moduli per la firma, tenere presente quanto segue:

* Assicurati che i moduli siano basati sul modello **SignMultipleForms** . In questo modo i moduli vengono precompilati con i dati recuperati dal database.

* I moduli devono essere configurati per utilizzare Adobe Sign e il campo firmatario1 deve essere associato al campo E-mail del cliente
* I moduli devono essere associati anche a clientLib denominato **getnextform**
* I moduli devono utilizzare il componente Passaggio firma .
* Il modulo deve inoltre utilizzare il componente **Firma più moduli** personalizzato. Questo componente consente di passare al modulo successivo per accedere al pacchetto.
* L’invio del modulo deve essere configurato per attivare il flusso di lavoro AEM **Aggiorna stato firma**
* Assicurati che il Percorso file dati sia impostato su **Data.xml**. Questo è molto importante in quanto il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

Dopo aver creato il modulo, includere nel modulo i **campi comuni** frammenti di modulo adattivo. Il frammento viene contrassegnato come nascosto. Questo frammento contiene i campi seguenti.

* **firmato**  - Campo che contiene lo stato della firma
* **guid**  - Identificatore univoco per identificare il modulo nel pacchetto
* **customerEmail**  - Questo campo contiene l’e-mail del cliente



>[!NOTE]
>Se si desidera trasferire i dati da un modulo a un altro del pacchetto, assicurarsi che i campi del modulo siano denominati in modo identico in tutti i moduli.

## Tutti i moduli completati

Una volta compilati e firmati tutti i moduli del pacchetto, è necessario visualizzare il messaggio appropriato. Questo messaggio viene visualizzato con l’aiuto del modulo Alldone. Il modulo Alldone è incluso nei moduli di esempio.

## Assets

I moduli di esempio con quelli utilizzati in questa esercitazione possono essere [scaricati da qui](assets/forms-for-signing.zip)
