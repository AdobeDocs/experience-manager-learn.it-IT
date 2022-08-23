---
title: Comprendere i diversi tipi di PDF forms e documenti
description: PDF è in realtà una famiglia di formati di file e questo articolo descrive i tipi di PDF importanti e rilevanti per gli sviluppatori di moduli.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

Portable Document Format (PDF) è in realtà una famiglia di formati di file e questo articolo descrive quelli più rilevanti per gli sviluppatori di moduli. Molti dei dettagli tecnici e degli standard di diversi tipi di PDF si stanno evolvendo e cambiando. Alcuni di questi formati e specifiche sono standard ISO (International Organization for Standardization) e alcuni sono proprietà intellettuale specifica di proprietà Adobe.

Questo articolo mostra come creare vari tipi di PDF. Ti aiuta a capire come e perché usarli. Tutti questi tipi funzionano al meglio nello strumento client principale per la visualizzazione e l’utilizzo degli PDF: Adobe Acrobat DC.

Di seguito è riportato un esempio di file PDF/A in Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

I file di esempio possono essere [scaricato da qui](assets/pdf-file-types.zip)

## XML Forms Architecture PDF(XFA PDF)

In Adobe viene utilizzato il termine modulo XFA PDF per fare riferimento al Forms interattivo e dinamico creato con AEM Forms Designer. Forms e i file creati con Designer sono basati su XML Forms Architecture (XFA) di Adobe. In molti modi, il formato di file XFA PDF è più simile a un file HTML che a un file PDF tradizionale. Ad esempio, il codice seguente mostra l’aspetto di un semplice oggetto testo in un file XFA PDF.

![Campo di testo](assets/text-field.JPG)

XFA Forms è basato su XML. Questo formato ben strutturato e flessibile consente ad AEM Forms Server di trasformare i file di Designer in formati diversi, tra cui PDF, PDF/A e HTML tradizionali. È possibile visualizzare la struttura XML completa del Forms in Designer selezionando la scheda Sorgente XML dell’Editor di layout. In AEM Forms Designer è possibile creare Forms XFA sia statico che dinamico.

## PDF statico

Il layout statico dei PDF forms XFA non cambia mai in fase di runtime, ma può essere interattivo per l’utente. Di seguito sono riportati alcuni vantaggi dei PDF forms XFA statici:

* Il layout statico dei PDF forms XFA non cambia mai in fase di runtime, ma può essere interattivo per l’utente.
* Forms statico supporta gli strumenti Commenti e Markup di Acrobat.
* La funzione Static Forms consente di importare ed esportare i commenti di Acrobat.
* Forms statico supporta l’impostazione secondaria dei font, una tecnica che può essere eseguita su un server AEM Forms.
* Il rendering statico di Forms può essere eseguito utilizzando il visualizzatore PDF incorporato fornito con i browser moderni.

>[!NOTE]
>
> È possibile creare PDF statici utilizzando AEM Forms Designer salvando XDP come modulo statico di Adobe PDF



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

Un documento certificato fornisce ai destinatari del documento PDF e Forms ulteriori garanzie di autenticità e integrità.

Il formato PDF più diffuso e diffuso è il file PDF tradizionale. Esistono diversi modi per creare un file PDF tradizionale, tra cui l’utilizzo di Acrobat e di molti strumenti di terze parti. Acrobat offre tutti i seguenti modi per creare file PDF tradizionali. Se Acrobat non è installato, è possibile che nel computer non siano presenti queste opzioni.

* Acquisendo il flusso di stampa di un&#39;applicazione desktop: Scegliere il comando Stampa di un&#39;applicazione di authoring e selezionare l&#39;icona della stampante Adobe PDF. Invece di una copia stampata del documento, avrai creato un file PDF del documento
* Utilizzando il plug-in Acrobat PDFMaker con le applicazioni Microsoft Office: Quando si installa Acrobat, viene aggiunto un menu Adobe PDF alle applicazioni Microsoft Office e un&#39;icona alla barra multifunzione di Office. È possibile utilizzare queste funzioni aggiunte per creare file PDF direttamente in Microsoft Office
* Utilizzando Acrobat Distiller per convertire i file Postscript e Incapsulated Postscript (EPS) in PDF: Distiller viene generalmente utilizzato nella pubblicazione di stampa e in altri flussi di lavoro che richiedono una conversione dal formato Postscript al formato PDF
* Sotto il cofano, un PDF tradizionale è molto diverso da un PDF XFA. Non ha la stessa struttura XML e, poiché viene creata catturando il flusso di stampa di un file, un PDF tradizionale è un file statico e di sola lettura.

Un documento certificato fornisce ai destinatari dei documenti e dei moduli di PDF garanzie aggiuntive sulla loro autenticità e integrità.

### Acroformati

Gli acromoduli sono la tecnologia dei moduli interattivi più vecchia di Adobe; risalgono alla versione 3 di Acrobat. L&#39;Adobe fornisce [Riferimento API di Acrobat Forms](assets/FormsAPIReference.pdf), del maggio 2003, per fornire i dettagli tecnici di questa tecnologia. Gli acromoduli sono una combinazione dei seguenti elementi:

* Un PDF tradizionale che definisce il layout statico e gli elementi grafici del modulo.
* Campi del modulo interattivo con blocco superiore con gli strumenti del modulo del programma Adobe Acrobat. Questi strumenti modulo sono un piccolo sottoinsieme di ciò che è disponibile in AEM Forms Designer.

### PDF/A (PDF per archivio)

PDF/A (PDF for Archives) sfrutta i vantaggi dell&#39;archiviazione dei documenti dei PDF tradizionali con molti dettagli specifici che migliorano l&#39;archiviazione a lungo termine. Il formato di file PDF tradizionale offre molti vantaggi per l&#39;archiviazione a lungo termine dei documenti. La compattezza di PDF facilita il trasferimento e consente di risparmiare spazio, e la sua natura ben strutturata consente potenti funzionalità di indicizzazione e ricerca. Il tradizionale PDF supporta ampiamente i metadati e PDF ha una lunga storia di supporto di diversi ambienti di computer.

Come PDF, PDF/A è una specifica standard ISO. È stato sviluppato da una task force che comprendeva AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) e l&#39;ufficio amministrativo dei tribunali statunitensi. Poiché l’obiettivo della specifica PDF/A è quello di fornire un formato di archivio a lungo termine, molte funzioni di PDF vengono omesse in modo che i file possano essere contenuti autonomamente. Di seguito sono riportati alcuni punti chiave della specifica che migliorano la riproducibilità a lungo termine del file PDF/A:

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

Anche se i documenti hanno lo stesso aspetto, il file PDF/A viene aperto con una barra blu nella parte superiore, a indicare che il documento è in visualizzazione in modalità PDF/A. Questa barra blu è la barra dei messaggi del documento di Acrobat, visualizzata all’apertura di alcuni tipi di file PDF.

![Pdf-img](assets/pdfa-message.png)

La barra dei messaggi del documento include istruzioni ed eventualmente pulsanti per completare un&#39;attività. È codificata in colori e vedrai il colore blu quando aprite tipi speciali di PDF (come questo file PDF/A) e PDF certificati e firmati digitalmente. La barra diventa viola per i PDF forms e giallo quando si partecipa a una revisione di PDF.

>[!NOTE]
>
> Se fai clic su Abilita modifica, rimuovi questo documento dalla conformità PDF/A.
