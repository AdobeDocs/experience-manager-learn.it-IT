---
title: Creare Forms per la firma
description: Creare moduli da includere nel pacchetto di firma.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Creazione di moduli per la firma

Il passaggio successivo consiste nel creare i moduli adattivi da includere nel pacchetto. Quando si creano moduli per la firma, tenere presente quanto segue:

* Assicurati che i moduli siano basati sul **SignMultipleForms** modello. In questo modo i moduli vengono precompilati con i dati recuperati dal database.

* I moduli devono essere configurati per utilizzare Adobe Sign e il campo firmatario1 deve essere associato al campo E-mail del cliente
* È inoltre necessario associare i moduli a clientLib **getnextform**
* I moduli devono utilizzare il componente Passaggio firma .
* Il modulo deve inoltre utilizzare **Firma multipla** componente. Questo componente consente di passare al modulo successivo per accedere al pacchetto.
* L’invio del modulo deve essere configurato per attivare AEM flusso di lavoro **Aggiorna stato firma**
* Assicurati che il Percorso file dati sia impostato su **Data.xml**. Questo è molto importante in quanto il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

Dopo aver creato il modulo, includere **campi comuni** frammento di modulo adattivo nel modulo. Il frammento è contrassegnato come nascosto. Questo frammento contiene i campi seguenti.

* **firmato** - Campo che contiene lo stato della firma
* **guid** - Identificatore univoco per identificare il modulo nel pacchetto
* **customerEmail** - Questo campo contiene l’e-mail del cliente



>[!NOTE]
>Se si desidera trasferire i dati da un modulo a un altro del pacchetto, assicurarsi che i campi del modulo siano denominati in modo identico in tutti i moduli.

## Tutti i moduli completati

Una volta compilati e firmati tutti i moduli del pacchetto, è necessario visualizzare il messaggio appropriato. Questo messaggio viene visualizzato con l’aiuto del modulo Alldone. Il modulo Alldone è incluso nei moduli di esempio.

## Assets

I moduli di esempio, tra cui quelli utilizzati in questa esercitazione, possono essere [scaricato da qui](assets/forms-for-signing.zip)
