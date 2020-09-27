---
title: Generazione di documenti del canale di stampa utilizzando la cartella esaminata
seo-title: Generazione di documenti del canale di stampa utilizzando la cartella esaminata
description: Questa è la parte 10 dell'esercitazione multistep per la creazione del primo documento di comunicazione interattiva per il canale di stampa. In questa parte, verranno generati documenti per i canali di stampa utilizzando il meccanismo delle cartelle controllato.
seo-description: Questa è la parte 10 dell'esercitazione multistep per la creazione del primo documento di comunicazione interattiva per il canale di stampa. In questa parte, verranno generati documenti per i canali di stampa utilizzando il meccanismo delle cartelle controllato.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---


# Generazione di documenti del canale di stampa utilizzando la cartella esaminata

In questa parte, verranno generati documenti per i canali di stampa utilizzando il meccanismo delle cartelle controllato.

Dopo aver creato e verificato il documento del canale di stampa, è necessario un meccanismo per generare il documento in modalità batch o su richiesta. In genere, questi tipi di documenti vengono generati in modalità batch e il meccanismo più comune consiste nell’utilizzare le cartelle esaminate.

Quando si configura una cartella esaminata in AEM, si associa uno script ECMA o un codice Java che viene eseguito quando un file viene rilasciato nella cartella esaminata. In questo articolo, ci concentreremo sullo script ECMA che genererà documenti per il canale di stampa e li salverà nel file system.

La configurazione delle cartelle e lo script ECMA controllati fanno parte delle risorse importate all&#39; [inizio di questa esercitazione](introduction.md)

Il file di input rilasciato nella cartella esaminata ha la struttura seguente. Lo script ECMA legge i numeri di conto e genera il documento del canale di stampa per ciascuno di questi account.

Per ulteriori dettagli sullo script ECMA per la generazione di documenti, [consultare questo articolo](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

Per generare il documento del canale di stampa utilizzando il meccanismo delle cartelle controllato, procedere come segue:

* [Seguire i passaggi indicati in questo documento](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Effettua il login per crx e passa a /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Assicuratevi che il percorso di interactiveCommunicationsDocument indichi il documento corretto da stampare.(Linea 1)
* Prendi nota di saveLocation(Linea 2). Puoi modificarla in base alle tue esigenze.
* Assicurarsi che il parametro di input del modello dati modulo sia associato all&#39;attributo Request e che il valore di binding sia impostato su &quot;accountnumber&quot;. Fare riferimento alla schermata sottostante.
   ![request](assets/requestattributeprintchannel.gif)

* Crea file accountnumber.xml con il contenuto seguente

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* Rilasciate il file xml in C:\RenderPrintChannel\input

* Controllate i file pdf nel percorso di salvataggio come specificato nello script ECMA.




