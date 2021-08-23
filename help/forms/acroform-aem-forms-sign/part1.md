---
title: Acroformi con AEM Forms
seo-title: Unire i dati del modulo adattivo con Acroform
description: Parte 1 dell’integrazione di Acroforms con AEM Forms. Creazione di un modulo adattivo tramite Acroform e unione dei dati per ottenere un PDF.
feature: moduli adattivi
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---


# Creazione di un modulo Acroform

I moduli sono creati con Acrobat. È possibile creare un nuovo modulo da zero utilizzando Acrobat oppure utilizzare un modulo esistente creato in Microsoft Word e convertirlo in Acroform utilizzando Acrobat. Per convertire un modulo creato in Microsoft Word in Acroform è necessario attenersi alla procedura descritta di seguito.

* Documento a parole aperte con Acrobat
* Utilizzare lo strumento di preparazione del modulo di Acrobat per identificare i campi del modulo.
* Salvare il pdf. Assicurati che il nome del file non contenga spazi.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Se desideri inviare l’acromodulo compilabile per la firma utilizzando Adobe Sign, dai un nome ai campi di conseguenza. Ad esempio, è possibile denominare un campo **Sig_es_:signer1:signature**. Sintassi comprensibile da Adobe Sign.

>[!NOTE]
>
>Se si invia un documento basato su XFA, è necessario appiattire il documento e i tag firma Adobe Sign devono essere presenti come testo statico nel documento.

[Documento sui tag di testo di Adobe Sign](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Assicurati che il nome del file acroform non contenga spazi. Il codice di esempio corrente non gestisce gli spazi.
>
>I nomi dei campi modulo possono contenere solo quanto segue:
>
>* spazio singolo
>* sottolineatura singola
>* caratteri alfanumerici

