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
> Quando apri un file CSV di esportazione dei metadati in Excel, utilizza [Importazione Excel](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) anziché fare doppio clic sul file per evitare problemi con i file CSV con codifica UTF-8.
>
> Per aprire il file CSV di esportazione dei metadati in Excel, effettua le seguenti operazioni:
> 
> 1. Apri Microsoft Excel
> 1. Seleziona __File > Nuovo__ per creare un foglio di calcolo vuoto
> 1. Con il foglio di calcolo vuoto aperto, seleziona __File > Importa__
> 1. Seleziona __Testo__ file e fai clic su __Importa__
> 1. Seleziona il file CSV esportato dal file system e fai clic su __Ottieni dati__
> 1. Nel passaggio 1 dell&#39;Importazione guidata, selezionare __Delimitato__ e imposta __Origine file__ a __Unicode (UTF-8)__ e fai clic su __Successivo__
> 1. Al passaggio 2, impostare __Delimitatori__ a __Virgola__ e fai clic su __Successivo__
> 1. Al passaggio 3, lasciare __Formato dati colonna__ così com’è, e fai clic su __Fine__
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
* Tutti i valori validi [Tipi di proprietà JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) sono supportati

* Formato proprietà con più valori - `<metadata property name> {{<property type : MULTI }}`

### Seconda riga a N righe

* La prima colonna contiene il percorso JCR assoluto di una risorsa. Ad esempio: /content/dam/asset1.jpg
* Nel file CSV potrebbero mancare dei valori nella proprietà dei metadati di una risorsa. Le proprietà di metadati mancanti per quella particolare risorsa non vengono aggiornate.
