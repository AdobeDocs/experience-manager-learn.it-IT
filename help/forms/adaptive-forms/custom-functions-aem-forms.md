---
title: Funzioni personalizzate in AEM Forms
description: Creare e utilizzare funzioni personalizzate in un modulo adattivo
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
duration: 286
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---

# Funzioni personalizzate

In AEM Forms 6.5 è stata introdotta la possibilità di definire funzioni di JavaScript che possono essere utilizzate per definire regole di business complesse utilizzando l’editor di regole.
AEM Forms fornisce una serie di funzioni personalizzate pronte all’uso, ma sarà necessario definire funzioni personalizzate e utilizzarle in più moduli.

Per definire la prima funzione personalizzata, procedere come segue:
* [Accedi a crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Crea una nuova cartella in app denominata Experience-league (il nome della cartella può essere scelto dall’utente).
* Salva le modifiche.
* Nella cartella experience-league crea un nuovo nodo di tipo cq:ClientLibraryFolder denominato clientlibs.
* Seleziona la cartella clientlibs appena creata e aggiungi le proprietà allowProxy e Categories come mostrato nella schermata e salva le modifiche.

![libreria client](assets/custom-functions.png)
* Crea una cartella denominata **js** nella cartella **clientlibs**
* Crea un file denominato **functions.js** nella cartella **js**
* Crea un file denominato **js.txt** nella cartella **clientlibs**. Salva le modifiche.
* La struttura delle cartelle deve essere simile alla schermata seguente.

![Editor di regole](assets/folder-structure.png)

* Fai doppio clic su functions.js per aprire l’editor.
Copia il seguente codice in functions.js e salva le modifiche.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @returns {string[]} An array of county names
 */
function getCountyNamesList()
{
    return ["Santa Clara", "Alameda", "Buxor", "Contra Costa", "Merced"];

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

[Per ulteriori dettagli sull&#39;annotazione delle funzioni JavaScript, fare riferimento a jsdoc](https://jsdoc.app/index.html).
Il codice di cui sopra ha due funzioni:
**getCountyNamesList** - restituisce una matrice di stringa
**convertUTC** - Converte il timestamp UTC in fuso orario locale

Apri il file js.txt e incolla il seguente codice, quindi salva le modifiche.

```javascript
#base=js
functions.js
```

La riga #base=js specifica in quale directory si trovano i file JavaScript.
Le righe seguenti indicano la posizione del file JavaScript rispetto alla posizione di base.

In caso di problemi durante la creazione delle funzioni personalizzate, puoi [scaricare e installare questo pacchetto](assets/custom-functions.zip) nella tua istanza di AEM.

## Utilizzo delle funzioni personalizzate

Il video seguente illustra i passaggi necessari per utilizzare la funzione personalizzata nell’editor di regole di un modulo adattivo
>[!VIDEO](https://video.tv.adobe.com/v/3445849?quality=12&learn=on&captions=ita)
