---
title: Utilizzo dell’importazione e dell’esportazione di metadati in  AEM Assets
description: ' funzionalità di importazione ed esportazione dei metadati AEM Assets consentono agli autori di contenuti di spostare facilmente i metadati delle risorse in AEM e sfruttare la potenza di Microsoft Excel per manipolare i metadati in scala, facilitando l''aggiornamento in massa dei metadati per le risorse esistenti in AEM.'
topics: metadata
audience: all
doc-type: feature video
activity: use
kt: 647
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 2c0818d0223a3db55e6407068f4802b9e7f7dd83
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 3%

---


# Utilizzo dell&#39;importazione e dell&#39;esportazione di metadati in  AEM Assets{#using-metadata-import-and-export-in-aem-assets}

 funzionalità di importazione ed esportazione dei metadati AEM Assets consentono agli autori di contenuti di spostare facilmente i metadati delle risorse in AEM e sfruttare la potenza di Microsoft Excel per manipolare i metadati in scala, facilitando l&#39;aggiornamento in massa dei metadati per le risorse esistenti in AEM.

## Esportazione metadati {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## Importazione metadati {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

Scarica [cartella sportiva WeRetail](assets/we-retail-sports.zip)

Scarica [pacchetto di metadati delle risorse](assets/we-retail-sports-asset-metadata.zip)

## Formato file metadati {#metadata-file-format}

### Formato file CSV

#### Prima riga

* La prima riga del file CSV definisce lo schema di metadati.
* Per impostazione predefinita, la prima colonna è `assetPath`, che contiene il percorso JCR assoluto per una risorsa.

* Le colonne successive nella prima riga indicano le altre proprietà di metadati di una risorsa.

   * Esempio : `dc:title, dc:description, jcr:title`

* Formato proprietà valore singolo

   * `<metadata property name> {{<property type}}`
   * Se il tipo di proprietà non è specificato, per impostazione predefinita è String.
   * Esempio: `dc:title {{String}}`

* Il nome della proprietà fa distinzione tra maiuscole e minuscole
   * Corretto : `dc:title {{String}}`
   * Scorretto: `Dc:Title {{String}}`

* Il tipo di proprietà non fa distinzione tra maiuscole e minuscole
* Sono supportati tutti i tipi di proprietà [JCR ](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) validi

* Formato proprietà multivalore - `<metadata property name> {{<property type : MULTI }}`

#### Seconda riga a N righe

* La prima colonna contiene il percorso JCR assoluto di una risorsa. Ad esempio: /content/dam/asset1.jpg
* La proprietà dei metadati per una risorsa potrebbe presentare valori mancanti nel file CSV. La proprietà di metadati mancante per quella particolare risorsa non verrà aggiornata.
