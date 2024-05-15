---
title: Acroforms con AEM Forms
description: Parte 2 dell’integrazione di Acroforms con AEM Forms. Crea uno schema da un Acroform.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integrazione" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 34
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---


# Creare uno schema dall’acroform

Il passaggio successivo consiste nel creare uno schema dall&#39;Acroform creato nel passaggio precedente. Come parte di questa esercitazione, viene fornita un’applicazione di esempio per creare lo schema. Per creare lo schema, segui le seguenti istruzioni:

1. Accedi a [CRXDE Liti](http://localhost:4502/crx/de)
2. Apri nel file `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Modificare il `saveLocation` in una cartella appropriata sul disco rigido. Assicurati che la cartella in cui stai salvando sia già stata creata.
4. Puntare il browser a [Crea XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) pagina ospitata sull’AEM.
5. Trascina e rilascia Acroform.
6. Controlla la cartella specificata nel passaggio 3. Il file dello schema viene salvato in questa posizione.

## Carica Acroform

Affinché questa demo possa funzionare sul sistema, è necessario creare una cartella denominata `acroforms` in AEM Assets. Carica Acroform in questo `acroforms` cartella.

>[!NOTE]
>
>Il codice di esempio cerca l&#39;acroforma in questa cartella. L’acroforma è necessaria per unire i dati inviati del modulo adattivo.
