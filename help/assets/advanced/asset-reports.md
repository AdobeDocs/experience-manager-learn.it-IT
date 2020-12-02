---
title: Report delle risorse in  AEM Assets
description: ' AEM Assets offre un framework di reporting a livello aziendale che consente di ridimensionare i repository di grandi dimensioni attraverso un''esperienza utente intuitiva. '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# Rapporti risorse{#using-reports-in-aem-assets}

 AEM Assets offre un framework di reporting a livello aziendale che consente di ridimensionare i repository di grandi dimensioni attraverso un&#39;esperienza utente intuitiva.

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Formule Microsoft Excel {#excel-formulas}

Nel video vengono utilizzate le formule seguenti per generare il grafico Risorse per dimensione in Microsoft Excel.

### Normalizzazione delle dimensioni delle risorse in byte {#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### Conteggio delle risorse per dimensione {#asset-count-by-size}

#### Inferiore a 200 KB {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### Da 200 KB a 500 KB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### Maggiore di 500 KB {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## Risorse aggiuntive{#additional-resources}

Scarica [tutti i file Excel delle risorse con Chart](./assets/asset-reports/all-assets.xlsx)
