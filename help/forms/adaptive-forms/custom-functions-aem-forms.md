---
title: Funzioni personalizzate in AEM Forms
description: Creare e utilizzare funzioni personalizzate in un modulo adattivo
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# Funzioni personalizzate

In AEM Forms 6.5 è stata introdotta la possibilità di definire funzioni JavaScript da utilizzare per definire regole di business complesse tramite l’editor di regole.
AEM Forms fornisce una serie di funzioni personalizzate pronte all’uso, ma sarà necessario definire funzioni personalizzate e utilizzarle in più moduli.

Per definire la prima funzione personalizzata, procedere come segue:
* [Accedi a crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Crea una nuova cartella in app denominata Experience-league (il nome della cartella può essere scelto dall’utente).
* Salva le modifiche.
* Nella cartella experience-league crea un nuovo nodo di tipo cq:ClientLibraryFolder denominato clientlibs.
* Seleziona la cartella clientlibs appena creata e aggiungi le proprietà allowProxy e Categories come mostrato nella schermata e salva le modifiche.

![client-lib](assets/custom-functions.png)
* Crea una cartella denominata **js** sotto **clientlibs** cartella
* Crea un file denominato **function.js** sotto **js** cartella
* Crea un file denominato **js.txt** sotto **clientlibs** cartella. Salva le modifiche.
* La struttura delle cartelle deve essere simile alla schermata seguente.

![Editor regola](assets/folder-structure.png)

* Fai doppio clic su functions.js per aprire l’editor.
Copia il seguente codice in functions.js e salva le modifiche.

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

Per favore [fai riferimento a jsdoc](https://jsdoc.app/index.html)per ulteriori dettagli sull’annotazione delle funzioni javascript.
Il codice di cui sopra ha due funzioni:
**getCountyNamesList** - restituisce una matrice di stringa
**convertUTC** - Converte la marca temporale UTC in fuso orario locale

Apri il file js.txt e incolla il seguente codice, quindi salva le modifiche.

```javascript
#base=js
functions.js
```

La riga #base=js specifica in quale directory si trovano i file JavaScript.
Le righe seguenti indicano la posizione del file JavaScript rispetto alla posizione di base.

In caso di problemi durante la creazione delle funzioni personalizzate, non esitare a [scarica e installa questo pacchetto](assets/custom-functions.zip) nel suo caso di AEM.

## Utilizzo delle funzioni personalizzate

Il video seguente illustra i passaggi necessari per utilizzare la funzione personalizzata nell’editor di regole di un modulo adattivo
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=12&learn=on)
