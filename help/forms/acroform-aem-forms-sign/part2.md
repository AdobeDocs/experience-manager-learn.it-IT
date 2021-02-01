---
title: Acrobat con  AEM Forms
seo-title: Unisci dati modulo adattivo con Acrobat
description: Parte 2 dell'integrazione di Acroformi con  AEM Forms. Creare uno schema da un modulo.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 2%

---


# Creazione di uno schema dall&#39;acroform

Il passaggio successivo consiste nel creare uno schema dall&#39;Acroform creato nel passaggio precedente. Nell&#39;ambito di questa esercitazione viene fornita un&#39;applicazione di esempio per creare lo schema. Per creare lo schema, attenersi alle istruzioni seguenti:

1. Accedi a [CRXDE Lite](http://localhost:4502/crx/de)
2. Aprire il file `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Modificate la cartella `saveLocation` in una cartella appropriata sul disco rigido. Verificate che la cartella in cui state salvando sia già stata creata.
4. Posizionate il browser sulla pagina [Crea XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) ospitata in AEM.
5. Trascinare l&#39;Acroform.
6. Controllate la cartella specificata al punto 3. Il file dello schema viene salvato in questa posizione.

## Caricare l&#39;Acroform

Affinché questa demo funzioni sul sistema, dovrete creare una cartella denominata `acroforms` in  AEM Assets. Caricate l&#39;Acroform in questa cartella `acroforms`.

>[!NOTE]
>
>Il codice di esempio cerca l’acroform in questa cartella. L&#39;acroform è necessario per unire i dati inviati dal modulo adattivo.
