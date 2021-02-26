---
title: Utilizzo dell’importazione e dell’esportazione di metadati in  AEM Assets
description: Scopri come importare ed esportare le funzioni di metadati di Adobe Experience Manager Assets. Le funzionalità di importazione ed esportazione consentono agli autori di contenuti di aggiornare in blocco i metadati per le risorse esistenti.
version: 6.3, 6.4, 6.5, cloud-service
topics: Content Management
feature: Metadati
role: Amministratore
level: Intermedio
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: 0d012d61b7740e461e641dddd6c5255a022305ea
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 4%

---


# Utilizzo dell&#39;importazione e dell&#39;esportazione di metadati in  AEM Assets {#metadata-import-and-export}

Scopri come importare ed esportare le funzioni di metadati di Adobe Experience Manager Assets. Le funzionalità di importazione ed esportazione consentono agli autori di contenuti di aggiornare in blocco i metadati per le risorse esistenti.

## Esportazione metadati {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## Importazione metadati {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> Quando preparate un file CSV da importare, è più semplice generare un CSV con l’elenco delle risorse utilizzando la funzione Esportazione metadati. Potete quindi modificare il file CSV generato e importarlo utilizzando la funzione Importa.

## Formato file CSV metadati {#metadata-file-format}

### Prima riga

* La prima riga del file CSV definisce lo schema di metadati.
* Per impostazione predefinita, la prima colonna è `assetPath`, che contiene il percorso JCR assoluto per una risorsa.

* Le colonne successive nella prima riga indicano le altre proprietà di metadati di una risorsa.
   * Esempio : `dc:title, dc:description, jcr:title`

* Formato proprietà valore singolo

   * `<metadata property name> {{<property type}}`
   * Se il tipo di proprietà non è specificato, per impostazione predefinita è Stringa.
   * Esempio: `dc:title {{String}}`

* Il nome della proprietà fa distinzione tra maiuscole e minuscole
   * Corretto : `dc:title {{String}}`
   * Scorretto: `Dc:Title {{String}}`

* Il tipo di proprietà non fa distinzione tra maiuscole e minuscole
* Sono supportati tutti i tipi di proprietà [JCR ](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) validi

* Formato proprietà multivalore - `<metadata property name> {{<property type : MULTI }}`

### Seconda riga a N righe

* La prima colonna contiene il percorso JCR assoluto di una risorsa. Ad esempio: /content/dam/asset1.jpg
* La proprietà dei metadati per una risorsa potrebbe presentare valori mancanti nel file CSV. Le proprietà di metadati mancanti per quella particolare risorsa non vengono aggiornate.
