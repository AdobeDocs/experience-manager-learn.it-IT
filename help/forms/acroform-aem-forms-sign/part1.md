---
title: Acroforms con AEM Forms
description: Parte 1 dell’integrazione di Acroforms con AEM Forms. Creazione di un modulo adattivo tramite Acroform e unione dei dati per ottenere un PDF.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 144
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# Creazione di Acroform

Le acroforme sono moduli creati con Acrobat. È possibile creare un nuovo modulo da zero utilizzando Acrobat oppure prendere un modulo esistente creato in Microsoft Word e convertirlo in Acroform utilizzando Acrobat. Per convertire in Acroform un modulo creato in Microsoft Word, è necessario eseguire la procedura seguente.

* Apri documento Word tramite Acrobat
* Utilizza lo strumento Prepara modulo di Acrobat per identificare i campi del modulo nel modulo.
* Salva il PDF. Verificare che il nome del file non contenga spazi.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>Se desideri inviare l’acroforma compilabile per la firma utilizzando Acrobat Sign, assegna un nome ai campi di conseguenza. Ad esempio, è possibile denominare un campo **`Sig_es_:signer1:signature`**. Questa è la sintassi che Acrobat Sign comprende.

>[!NOTE]
>
>Se si invia un documento basato su XFA, è necessario appiattire il documento e i tag di firma Acrobat Sign devono essere presenti come testo statico nel documento.

[Documento tag di testo Acrobat Sign](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Verificare che il nome del file dell&#39;acroform non contenga spazi. Il codice di esempio corrente non gestisce gli spazi.
>
>I nomi dei campi modulo possono contenere solo i seguenti elementi:
>
>* spazio singolo
>* carattere di sottolineatura singolo
>* caratteri alfanumerici
