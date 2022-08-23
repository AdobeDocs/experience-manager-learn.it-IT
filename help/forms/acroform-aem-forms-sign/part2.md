---
title: Acroformi con AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Parte 2 dell’integrazione di Acroforms con AEM Forms. Creare uno schema da un Acroform.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 2%

---


# Creare uno schema dall’acroform

Il passaggio successivo consiste nel creare uno schema da Acroform creato nel passaggio precedente. Nell&#39;ambito di questa esercitazione viene fornita un&#39;applicazione di esempio per creare lo schema. Per creare lo schema, segui le seguenti istruzioni:

1. Accedi a [CRXDE Lite](http://localhost:4502/crx/de)
2. Apri nel file . `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Modificare la `saveLocation` in una cartella appropriata sul disco rigido. Assicurati che la cartella in cui stai salvando sia già stata creata.
4. Posiziona il browser su [Crea XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) pagina ospitata su AEM.
5. Trascinare l&#39;Acroform.
6. Controlla la cartella specificata nel passaggio 3. Il file di schema viene salvato in questa posizione.

## Caricare l’Acroform

Affinché questa demo funzioni sul tuo sistema, dovrai creare una cartella denominata `acroforms` in AEM Assets. Carica l&#39;Acroform in questo `acroforms` cartella.

>[!NOTE]
>
>Il codice di esempio cerca l’acroform in questa cartella. La macro è necessaria per unire i dati inviati dal modulo adattivo.
