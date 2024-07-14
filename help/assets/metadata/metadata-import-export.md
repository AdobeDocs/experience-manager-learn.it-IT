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
duration: 419
source-git-commit: 726715890d997ba3bb85f4833e220ac2222b3a42
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---

# Utilizzo dell’importazione e dell’esportazione dei metadati in AEM Assets {#metadata-import-and-export}

Scopri come utilizzare le funzioni di importazione ed esportazione dei metadati di Adobe Experience Manager Assets. Le funzionalità di importazione ed esportazione consentono agli autori di contenuto di aggiornare in blocco i metadati per le risorse esistenti.

## Esportazione metadati {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

>[!TIP]
>
> Quando apri il file CSV di esportazione dei metadati in Excel, utilizza l&#39;[importazione Excel](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) invece di fare doppio clic sul file per evitare problemi con i file CSV con codifica UTF-8.
>
> Per aprire il file CSV di esportazione dei metadati in Excel, effettua le seguenti operazioni:
> 
> 1. Apri Microsoft Excel
> 1. Seleziona __File > Nuovo__ per creare un foglio di calcolo vuoto
> 1. Con il foglio di calcolo vuoto aperto, selezionare __File > Importa__
> 1. Seleziona il file __Testo__ e fai clic su __Importa__
> 1. Seleziona il file CSV esportato dal file system e fai clic su __Ottieni dati__
> 1. Nel passaggio 1 dell&#39;importazione guidata, selezionare __Delimitato__ e impostare __Origine file__ su __Unicode (UTF-8)__, quindi fare clic su __Avanti__
> 1. Al passaggio 2, imposta __Delimitatori__ su __Virgola__ e fai clic su __Avanti__
> 1. Al passaggio 3, lasciare invariato il formato dati __Colonna__ e fare clic su __Fine__
> 1. Seleziona __Importa__ per aggiungere i dati al foglio di calcolo

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
   * Esempio: `dc:title, dc:description, jcr:title`

* Formato proprietà valore singolo

   * `<metadata property name> {{<property type}}`
   * Se il tipo di proprietà non è specificato, viene utilizzato il valore predefinito String.
   * Esempio: `dc:title {{String}}`

* Il nome della proprietà distingue tra maiuscole e minuscole
   * Corretto: `dc:title {{String}}`
   * Errato: `Dc:Title {{String}}`

* Il tipo di proprietà non distingue tra maiuscole e minuscole
* Sono supportati tutti i tipi di proprietà [JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) validi

* Formato proprietà con più valori - `<metadata property name> {{<property type : MULTI }}`

### Seconda riga a N righe

* La prima colonna contiene il percorso JCR assoluto di una risorsa. Ad esempio: /content/dam/asset1.jpg
* Nel file CSV potrebbero mancare dei valori nella proprietà dei metadati di una risorsa. Le proprietà di metadati mancanti per quella particolare risorsa non vengono aggiornate.
