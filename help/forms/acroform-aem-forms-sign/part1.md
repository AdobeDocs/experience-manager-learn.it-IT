---
title: Acrobat con  AEM Forms
seo-title: Unisci dati modulo adattivo con Acrobat
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---


# Creazione di un modulo

I moduli Acrobat sono moduli creati utilizzando  Acrobat. È possibile creare un nuovo modulo da zero utilizzando  Acrobat oppure utilizzare un modulo esistente creato in Microsoft Word e convertirlo in Acrobat utilizzando  Acrobat. Per convertire in Acrobat un modulo creato in Microsoft Word, è necessario seguire la procedura seguente.

* Documento a termine aperto con  Acrobat
* Utilizzare  strumento Prepara modulo Acrobat per identificare i campi modulo nel modulo.
* Salvate il pdf. Accertatevi che il nome del file non contenga spazi.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Se si desidera inviare il modulo compilabile per la firma tramite  Adobe Sign, assegnare un nome ai campi di conseguenza. Ad esempio, è possibile assegnare un nome a un campo **Sig_es_:signer1:signature**. Questa è la sintassi  Adobe Sign comprende.

>[!NOTE]
>
>Se si invia un documento basato su XFA, è necessario appiattire il documento e i tag firma Adobe Sign  devono essere presenti come testo statico nel documento.

[documento tag di testo Adobe Sign](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Verificate che il nome del file acroform non contenga spazi. Il codice di esempio corrente non gestisce gli spazi.
>
>I nomi dei campi modulo possono contenere solo i seguenti elementi:
>
>* spazio singolo
>* sottolineatura singola
>* caratteri alfanumerici

