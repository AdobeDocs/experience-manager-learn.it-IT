---
title: Generazione di documenti del canale di stampa tramite cartella controllata
description: Questa è la parte 10 del tutorial a più passaggi per creare il primo documento di comunicazione interattiva per il canale di stampa. In questa parte verranno generati documenti del canale di stampa utilizzando il meccanismo di cartelle controllate.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
duration: 70
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Generazione di documenti del canale di stampa tramite cartella controllata

In questa parte verranno generati documenti del canale di stampa utilizzando il meccanismo di cartelle controllate.

Dopo aver creato e testato il documento del canale di stampa, è necessario un meccanismo per generare tale documento in modalità batch o su richiesta. In genere, questi tipi di documenti vengono generati in modalità batch e il meccanismo più comune consiste nell’utilizzare la cartella controllata.

Quando configuri una cartella controllata in AEM, associ uno script ECMA o un codice Java che viene eseguito quando un file viene rilasciato nella cartella controllata. In questo articolo, ci concentreremo sullo script ECMA che genererà documenti del canale di stampa e li salverà nel file system.

La configurazione della cartella controllata e lo script ECMA fanno parte delle risorse importate in [inizio di questa esercitazione](introduction.md)

Il file di input rilasciato nella cartella controllata ha la seguente struttura. Lo script ECMA legge i numeri di account e genera il documento del canale di stampa per ciascuno di questi account.

Per ulteriori dettagli sullo script ECMA per la generazione di documenti, [fai riferimento a questo articolo](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

Per generare un documento del canale di stampa utilizzando il meccanismo delle cartelle controllate, procedere come segue:

* [Segui i passaggi indicati in questo documento](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Accedi a crx e passa a /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Verificare che il percorso di interactiveCommunicationsDocument punti al documento corretto che si desidera stampare.( Riga 1)
* Prendere nota di saveLocation(Line 2).È possibile modificarlo in base alle proprie esigenze.
* Assicurati che il parametro di input per il modello dati modulo sia associato all’attributo richiesta e che il relativo valore di associazione sia impostato su &quot;accountnumber&quot;. Fai riferimento alla schermata seguente.
  ![richiesta](assets/requestattributeprintchannel.gif)

* Crea il file accountnumbers.xml con il seguente contenuto

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

* Rilascia il file XML in C:\RenderPrintChannel\input

* Controllare i file PDF nel percorso di salvataggio come specificato nello script ECMA.

## Passaggi successivi

[Apertura dell’interfaccia utente dell’agente all’invio del modulo](./opening-agent-ui-on-form-submission.md)