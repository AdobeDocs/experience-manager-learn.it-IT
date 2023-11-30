---
title: Utilizzo dell’importazione e dell’esportazione dei metadati in AEM Assets
description: Scopri come utilizzare le funzioni di importazione ed esportazione dei metadati di Adobe Experience Manager Assets. Le funzionalità di importazione ed esportazione consentono agli autori di contenuto di aggiornare in blocco i metadati per le risorse esistenti.
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 3%

---

# Utilizzo dell’importazione e dell’esportazione dei metadati in AEM Assets {#metadata-import-and-export}

Scopri come utilizzare le funzioni di importazione ed esportazione dei metadati di Adobe Experience Manager Assets. Le funzionalità di importazione ed esportazione consentono agli autori di contenuto di aggiornare in blocco i metadati per le risorse esistenti.

## Esportazione metadati {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

## Importazione metadati {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> Quando prepari un file CSV da importare, è più semplice generare un file CSV con l’elenco delle risorse utilizzando la funzione Esportazione metadati. Puoi quindi modificare il file CSV generato e importarlo utilizzando la funzione Importa.

## Formato file CSV metadati {#metadata-file-format}

### Prima riga

* La prima riga del file CSV definisce lo schema metadati.
* Il valore predefinito della prima colonna è `assetPath`, che contiene il percorso JCR assoluto di una risorsa.

* Le colonne successive nella prima riga puntano ad altre proprietà di metadati di una risorsa.
   * Ad esempio : `dc:title, dc:description, jcr:title`

* Formato proprietà valore singolo

   * `<metadata property name> {{<property type}}`
   * Se il tipo di proprietà non è specificato, viene utilizzato il valore predefinito String.
   * Esempio: `dc:title {{String}}`

* Il nome della proprietà distingue tra maiuscole e minuscole
   * Corretto: `dc:title {{String}}`
   * Errato: `Dc:Title {{String}}`

* Il tipo di proprietà non distingue tra maiuscole e minuscole
* Tutti i valori validi [Tipi di proprietà JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) sono supportati

* Formato proprietà con più valori - `<metadata property name> {{<property type : MULTI }}`

### Seconda riga a N righe

* La prima colonna contiene il percorso JCR assoluto di una risorsa. Ad esempio: /content/dam/asset1.jpg
* Nel file CSV potrebbero mancare dei valori nella proprietà dei metadati di una risorsa. Le proprietà di metadati mancanti per quella particolare risorsa non vengono aggiornate.
