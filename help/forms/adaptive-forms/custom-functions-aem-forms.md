---
title: Funzioni personalizzate in AEM Forms
description: Creare e utilizzare funzioni personalizzate in Modulo adattivo
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
kt: 9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# Funzioni personalizzate

AEM Forms 6.5 ha introdotto la possibilità di definire funzioni JavaScript che possono essere utilizzate per definire regole di business complesse utilizzando l’editor di regole.
AEM Forms fornisce numerose funzioni personalizzate preconfigurate, ma sarà necessario definire le proprie funzioni personalizzate e utilizzarle in più moduli.

Per definire la prima funzione personalizzata, effettua le seguenti operazioni:
* [Accedi a crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Crea una nuova cartella sotto le app denominate experience-league (questo nome di cartella può essere un nome a tua scelta)
* Salva le modifiche.
* Sotto la cartella experience-league crea un nuovo nodo di tipo cq:ClientLibraryFolder chiamato clientlibs.
* Seleziona la nuova cartella clientlibs e aggiungi le proprietà allowProxy e categories come mostrato nella schermata iniziale e salva le modifiche.

![client-lib](assets/custom-functions.png)
* Crea una cartella denominata **js** in **clientlibs** cartella
* Crea un file denominato **function.js** in **js** cartella
* Crea un file denominato **js.txt** in **clientlibs** cartella. Salva le modifiche.
* La struttura della cartella deve essere simile alla schermata seguente.

![Editor regola](assets/folder-structure.png)

* Fai doppio clic su function.js per aprire l&#39;editor.
Copia il seguente codice in function.js e salva le modifiche.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @return {OPTIONS} drop down options 
 */
function getCountyNamesList()
{
    var countyNames= [];
    countyNames[0] = "Santa Clara";
    countyNames[1] = "Alameda";
    countyNames[2] = "Buxor";
    countyNames[3] = "Contra Costa";
    countyNames[4] = "Merced";

    return countyNames;

}
/**
* Covert UTC to Local Time
* @name convertUTC Convert UTC Time to Local Time
* @param {string} strUTCString in Stringformat
* @return {string}
*/
function convertUTC(strUTCString)
{
    var dt = new Date(strUTCString);
    console.log(dt.toLocaleString());
    return dt.toLocaleString();
}
```

Per favore [fare riferimento a jsdoc ](https://jsdoc.app/index.html)per ulteriori informazioni sull’annotazione delle funzioni javascript.
Il codice di cui sopra ha due funzioni:
**getCountyNamesList** - restituisce un array di stringa
**convertUTC** - Converte la marca temporale UTC in fuso orario locale

Apri js.txt e incolla il seguente codice e salva le modifiche.

```javascript
#base=js
functions.js
```

La riga #base=js specifica in quale directory si trovano i file JavaScript.
Le righe seguenti indicano la posizione del file JavaScript rispetto alla posizione di base.

In caso di problemi nella creazione delle funzioni personalizzate, puoi [scarica e installa il pacchetto](assets/custom-functions.zip) nella tua istanza AEM.

## Utilizzo delle funzioni personalizzate

Il video seguente illustra i passaggi necessari per utilizzare una funzione personalizzata nell’editor di regole di un modulo adattivo
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=9&learn=on)
