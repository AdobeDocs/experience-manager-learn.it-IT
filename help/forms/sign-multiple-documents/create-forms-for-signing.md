---
title: Crea Forms per la firma
description: Crea i moduli da includere nel pacchetto di firma.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 98
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Creare moduli per la firma

Il passaggio successivo consiste nel creare i moduli adattivi da includere nel pacchetto. Ricorda di rispettare i seguenti punti durante la creazione di moduli per la firma:

* Assicurati che i moduli siano basati su **SignMultipleForms** modello. In questo modo i moduli vengono precompilati con i dati recuperati dal database.

* I moduli devono essere configurati per utilizzare Acrobat Sign e il campo signer1 deve essere associato al campo E-mail cliente
* I moduli devono essere associati anche a clientLib denominata **getnextform**
* I moduli devono utilizzare il componente Passaggio di firma.
* Il modulo deve inoltre utilizzare il **Firma più moduli** componente. Questo componente consente di passare al modulo successivo per accedere al pacchetto.
* L’invio del modulo deve essere configurato per attivare il flusso di lavoro AEM **Aggiorna stato firma**
* Assicurarsi che il percorso del file di dati sia impostato su **Dati.xml**. Questo è molto importante, poiché il codice di esempio cerca un file denominato Data.xml nel payload del processo di invio del modulo.

Dopo aver creato il modulo, includi **commonfield** frammento di modulo adattivo nel modulo. Il frammento è contrassegnato come nascosto. Questo frammento contiene i campi seguenti.

* **firmato** - Campo contenente lo stato della firma
* **GUID** - Identificatore univoco per identificare il modulo nel pacchetto
* **customerEmail** - Questo campo contiene l’e-mail del cliente



>[!NOTE]
>Se si desidera trasferire i dati da un modulo a un altro nel pacchetto, assicurarsi che i campi del modulo siano denominati in modo identico in tutti i moduli.

## Modulo completato

Una volta compilati e firmati tutti i moduli del pacchetto, è necessario visualizzare il messaggio appropriato. Questo messaggio viene visualizzato con l&#39;aiuto di tutti i moduli. Il modulo Alldone è incluso nei moduli di esempio.

## Risorse

I moduli di esempio che includono quelli utilizzati in questa esercitazione possono essere [scaricato da qui](assets/forms-for-signing.zip)

## Passaggi successivi

[Test della soluzione sul sistema locale](./testing-and-trouble-shooting.md)
