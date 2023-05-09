---
title: Generazione di documenti del canale di stampa utilizzando la cartella sottoposta a controllo
seo-title: Generating Print Channel Documents Using Watched Folder
description: Questa è la parte 10 dell'esercitazione su più passaggi per creare il primo documento di comunicazione interattivo per il canale di stampa. In questa parte, verranno generati i documenti del canale di stampa utilizzando il meccanismo di cartelle controllate.
seo-description: This is part 10 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will generate print channel documents using the watched folder mechanism.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# Generazione di documenti del canale di stampa utilizzando la cartella sottoposta a controllo

In questa parte, verranno generati i documenti del canale di stampa utilizzando il meccanismo di cartelle controllate.

Dopo aver creato e testato il documento del canale di stampa, è necessario un meccanismo per generare il documento in modalità batch o su richiesta. In genere, questi tipi di documenti vengono generati in modalità batch e il meccanismo più comune è quello di utilizzare cartelle controllate.

Quando configuri una cartella controllata in AEM, associ uno script ECMA o un codice java che viene eseguito quando un file viene rilasciato nella cartella controllata. In questo articolo, ci concentreremo sullo script ECMA che genererà documenti del canale di stampa e li salverà nel file system.

La configurazione della cartella controllata e lo script ECMA fanno parte delle risorse importate nella [inizio dell&#39;esercitazione](introduction.md)

Il file di input rilasciato nella cartella controllata presenta la seguente struttura. Lo script ECMA legge i numeri dell&#39;account e genera il documento del canale di stampa per ciascuno di questi account.

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

Per generare il documento del canale di stampa utilizzando il meccanismo delle cartelle controllate, seguire i passaggi seguenti:

* [Segui i passaggi indicati in questo documento](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Accedi a crx e naviga su /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Assicurarsi che il percorso di interattivoCommunicationsDocument indichi il documento corretto che si desidera stampare.(Linea 1)
* Prendi nota di saveLocation(Line 2).Puoi modificarla in base alle tue esigenze.
* Assicurati che il parametro di input del modello dati modulo sia associato all’attributo di richiesta e che il relativo valore di binding sia impostato su &quot;accountNumber&quot;. Fai riferimento alla schermata seguente.
   ![richiesta](assets/requestattributeprintchannel.gif)

* Crea il file accounts.xml con il seguente contenuto

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

* Rilascia il file xml in C:\RenderPrintChannel\input

* Controllare i file pdf nel percorso di salvataggio come specificato nello script ECMA.

## Passaggi successivi

[Apertura dell’interfaccia dell’agente durante l’invio del modulo](./opening-agent-ui-on-form-submission.md)