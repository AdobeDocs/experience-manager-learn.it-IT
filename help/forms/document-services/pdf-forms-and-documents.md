---
title: Comprendere i diversi tipi di PDF forms e documenti
description: PDF è in realtà una famiglia di formati di file, e questo articolo descrive i tipi di PDF importanti e rilevanti per gli sviluppatori di moduli.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner,Intermediate
version: 6.3,6.4,6.5
feature: Document Services
kt: 7071
topic: Development
translation-type: tm+mt
source-git-commit: 1b4512fdb047bec15d72a8278fd0ce5dfafa309f
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---


# PDF

Il formato PDF (Portable Document Format) è in realtà una famiglia di formati di file e questo articolo descrive quelli più rilevanti per gli sviluppatori di moduli. Molti dei dettagli tecnici e degli standard dei diversi tipi di PDF si stanno evolvendo e cambiando. Alcuni di questi formati e specifiche sono standard ISO (International Organization for Standardization) e alcuni sono proprietà intellettuale specifica di proprietà Adobe.

Questo articolo illustra come creare vari tipi di PDF. Ti aiuta a capire come e perché usarli. Tutti questi tipi funzionano al meglio nello strumento client principale per visualizzare e lavorare con i PDF: Adobe Acrobat DC.

Di seguito è riportato un esempio di file PDF/A in Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

I file di esempio possono essere [scaricati da qui](assets/pdf-file-types.zip)

## PDF dell’architettura Forms Xml

Ad Adobe, il termine modulo PDF fa riferimento alla Forms interattiva e dinamica creata con AEM Forms Designer. Forms e i file creati con Designer sono basati su XML Forms Architecture (XFA) di Adobe. In molti modi, il formato PDF XFA è più vicino a un file HTML che a un file PDF tradizionale. Ad esempio, il codice seguente mostra l’aspetto di un semplice oggetto testo in un file PDF XFA.

![Campo di testo](assets/text-field.JPG)

XFA Forms è basato su XML. Questo formato ben strutturato e flessibile consente ad AEM Forms Server di trasformare i file di Designer in formati diversi, inclusi PDF, PDF/A e HTML tradizionali. È possibile visualizzare la struttura XML completa del Forms in Designer selezionando la scheda Sorgente XML dell’Editor di layout. In AEM Forms Designer è possibile creare Forms XFA sia statico che dinamico.

## PDF statico

Il layout statico dei PDF forms XFA non cambia mai in fase di runtime, ma può essere interattivo per l’utente. Di seguito sono riportati alcuni vantaggi dei PDF forms XFA statici:

* Il layout statico dei PDF forms XFA non cambia mai in fase di runtime, ma può essere interattivo per l’utente.
* Forms statico supporta gli strumenti Commenti e Markup di Acrobat.
* La funzione Static Forms consente di importare ed esportare i commenti di Acrobat.
* Forms statico supporta l’impostazione secondaria dei font, una tecnica che può essere eseguita su un server AEM Forms.
* È possibile eseguire il rendering di Forms statico utilizzando il visualizzatore PDF integrato fornito con i browser moderni.

>[!NOTE]
>
> È possibile creare PDF statici utilizzando AEM Forms Designer salvando XDP come modulo PDF statico Adobe

## Formati PDF

Il formato PDF (Portable Document Format) è in realtà una famiglia di formati di file e questo articolo descrive quelli più rilevanti per gli sviluppatori di moduli. Molti dei dettagli tecnici e degli standard dei diversi tipi di PDF si stanno evolvendo e cambiando. Alcuni di questi formati e specifiche sono standard ISO (International Organization for Standardization) e alcuni sono proprietà intellettuale specifica di proprietà Adobe.

Questo articolo illustra come creare vari tipi di PDF. Ti aiuterà a capire come e perché usarli. Tutti questi tipi funzionano al meglio nello strumento client principale per visualizzare e lavorare con i PDF: Adobe Acrobat DC.

Questo è un esempio di file PDF/A in Acrobat DC.

![pdfa](assets/pdfa-file-in-acrobat.png)

I file di esempio possono essere [scaricati da qui](assets/pdf-file-types.zip)

### PDF XFA

Ad Adobe, il termine modulo PDF fa riferimento ai moduli interattivi e dinamici creati con AEM Forms Designer. È importante notare che esiste un altro tipo di modulo PDF, denominato Acroform, diverso dai PDF forms creati in AEM Forms Designer. I moduli e i file creati con Designer sono basati su XML Forms Architecture (XFA) di Adobe. In molti modi, il formato PDF XFA è più vicino a un file HTML che a un file PDF tradizionale. Ad esempio, il codice seguente mostra l’aspetto di un semplice oggetto testo in un file PDF XFA.

![campo di testo](assets/text-field.JPG)

Come è possibile vedere, i moduli XFA sono basati su XML. Questo formato ben strutturato e flessibile consente ad AEM Forms Server di trasformare i file di Designer in formati diversi, inclusi PDF, PDF/A e HTML tradizionali. È possibile visualizzare la struttura XML completa dei moduli in Designer selezionando la scheda Sorgente XML dell’Editor di layout. In AEM Forms Designer è possibile creare moduli XFA statici e dinamici.

### PDF statico

I PDF forms XFA statici non modificano il layout in fase di runtime, ma possono essere interattivi per l’utente. Di seguito sono riportati alcuni vantaggi dei PDF forms XFA statici:

* I PDF forms XFA statici non modificano il layout in fase di runtime, ma possono essere interattivi per l’utente.
* I moduli statici supportano gli strumenti Commenti e Markup di Acrobat.
* I moduli statici consentono di importare ed esportare i commenti di Acrobat.
* I moduli statici supportano l’impostazione secondaria dei font, una tecnica che può essere eseguita su un server AEM Forms.
* È possibile eseguire il rendering dei moduli statici utilizzando il visualizzatore pdf incorporato fornito con i browser moderni.

>[!NOTE]
> È possibile creare pdf statici utilizzando AEM Forms Designer salvando XDP come modulo PDF statico Adobe

### Forms dinamico

I PDF XFA dinamici possono modificare il layout in fase di esecuzione, pertanto le funzioni di commento e markup non sono supportate. Tuttavia, i PDF XFA dinamici offrono i seguenti vantaggi:

* I moduli dinamici supportano script sul lato client che modificano il layout e l’impaginazione del modulo. Ad esempio, il file Purchase Order.xdp si espanderà e paginerà per contenere una quantità infinita di dati se viene salvato come modulo dinamico
* I moduli dinamici supportano tutte le proprietà del modulo in fase di esecuzione, mentre i moduli statici supportano solo un sottoinsieme

>[!NOTE]
>
> È possibile creare pdf dinamici utilizzando AEM Forms Designer salvando XDP come Adobe modulo XML dinamico

>[!NOTE]
>
> Non è possibile eseguire il rendering dei moduli dinamici utilizzando i visualizzatori pdf incorporati dei browser moderni.

### File PDF (PDF tradizionale)

Un documento certificato fornisce ai destinatari di documenti PDF e Forms garanzie aggiuntive sulla sua autenticità e integrità.

Il formato PDF più diffuso è il file PDF tradizionale. Esistono diversi modi per creare un file PDF tradizionale, tra cui l’utilizzo di Acrobat e di molti strumenti di terze parti. Acrobat offre tutti i seguenti modi per creare file PDF tradizionali. Se Acrobat non è installato, è possibile che nel computer non siano presenti queste opzioni.

* Acquisendo il flusso di stampa di un&#39;applicazione desktop: Scegliere il comando Stampa di un&#39;applicazione di authoring e selezionare l&#39;icona della stampante Adobe PDF. Invece di una copia stampata del documento, avrai creato un file PDF del documento
* Utilizzando il plug-in Acrobat PDFMaker con le applicazioni Microsoft Office: Quando si installa Acrobat, viene aggiunto un menu Adobe PDF alle applicazioni Microsoft Office e un&#39;icona alla barra multifunzione di Office. È possibile utilizzare queste funzioni aggiunte per creare file PDF direttamente in Microsoft Office
* Utilizzando Acrobat Distiller per convertire i file Postscript e Incapsulated Postscript (EPS) in PDF: Distiller viene generalmente utilizzato nella pubblicazione di stampa e in altri flussi di lavoro che richiedono una conversione dal formato Postscript al formato PDF
* Sotto il cofano, un PDF tradizionale è molto diverso da un PDF XFA. Non ha la stessa struttura XML e, poiché viene creata acquisendo il flusso di stampa di un file, un PDF tradizionale è un file statico e di sola lettura.

Un documento certificato fornisce ai destinatari di documenti PDF e moduli ulteriori garanzie di autenticità e integrità.

### Acroformati

Gli acromoduli sono la tecnologia dei moduli interattivi di Adobe; risalgono alla versione 3 di Acrobat. L’Adobe fornisce il [Riferimento API di Acrobat Forms](assets/FormsAPIReference.pdf), datato maggio 2003, per fornire i dettagli tecnici di questa tecnologia. Gli acroformi sono una combinazione dei
punti seguenti:

* Un PDF tradizionale che definisce il layout statico e gli elementi grafici del modulo.
* Campi del modulo interattivo con blocco superiore con gli strumenti del modulo del programma Adobe Acrobat. Questi strumenti per i moduli sono un piccolo sottoinsieme di quelli disponibili in AEM Forms Designer.

### PDF/A (PDF per archiviazione)

PDF/A (PDF for Archives) sfrutta i vantaggi dell’archiviazione dei documenti dei PDF tradizionali con molti dettagli specifici che migliorano l’archiviazione a lungo termine. Il formato PDF tradizionale offre molti vantaggi per l&#39;archiviazione a lungo termine dei documenti. La compattezza del formato PDF facilita il trasferimento e consente di risparmiare spazio, e la sua natura ben strutturata consente potenti funzionalità di indicizzazione e ricerca. Il formato PDF tradizionale supporta ampiamente i metadati e PDF supporta da tempo diversi ambienti di computer.

Come il PDF, PDF/A è una specifica standard ISO. È stato sviluppato da una task force che comprendeva AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) e l&#39;ufficio amministrativo dei tribunali statunitensi. Poiché l’obiettivo della specifica PDF/A è quello di fornire un formato di archivio a lungo termine, molte funzioni PDF vengono omesse in modo che i file possano essere contenuti autonomamente. Di seguito sono riportati alcuni punti chiave della specifica che migliorano la riproducibilità a lungo termine del file PDF/A:

* Tutto il contenuto deve essere contenuto nel file e non ci possono essere dipendenze da fonti esterne come collegamenti ipertestuali, font o programmi software.
* Tutti i font devono essere incorporati e devono essere font con una licenza d&#39;uso illimitata per i documenti elettronici.
* JavaScript non è consentito
* Trasparenza non consentita
* Crittografia non consentita
* I contenuti audio e video non sono consentiti
* Gli spazi colore devono essere definiti in modo indipendente dal dispositivo
* Tutti i metadati devono rispettare determinati standard

### Visualizzazione di un file PDF/A

Due file di esempio sono stati creati dallo stesso file di Microsoft Word. Uno è stato creato come PDF tradizionale e l’altro come file PDF/A. Apri questi due file in Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Anche se i documenti hanno lo stesso aspetto, il file PDF/A viene aperto con una barra blu nella parte superiore, che indica che il documento è visualizzato in modalità PDF/A. Questa barra blu è la barra dei messaggi del documento di Acrobat, visualizzata all’apertura di alcuni tipi di file PDF.

![Pdf-img](assets/pdfa-message.png)

La barra dei messaggi del documento include istruzioni ed eventualmente pulsanti per completare un&#39;attività. È codificata in colori e vedrai il colore blu quando apri tipi speciali di PDF (come questo file PDF/A) e PDF certificati e firmati digitalmente. La barra diventa viola per i PDF forms e giallo quando si partecipa a una revisione PDF.

>[!NOTE]
>
> Se fai clic su Abilita modifica, rimuovi questo documento dalla conformità PDF/A.




