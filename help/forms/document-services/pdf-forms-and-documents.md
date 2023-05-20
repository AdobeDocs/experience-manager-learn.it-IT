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
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

Portable Document Format (PDF) è in realtà un insieme di formati di file e in questo articolo vengono illustrati i più rilevanti per gli sviluppatori di moduli. Molti dei dettagli tecnici e degli standard dei diversi tipi di PDF sono in evoluzione e in evoluzione. Alcuni di questi formati e specifiche sono standard ISO (International Organization for Standardization), mentre altri sono specifici diritti di proprietà intellettuale di proprietà dell&#39;Adobe.

Questo articolo illustra come creare vari tipi di PDF. Ti aiuta a capire come e perché usarle tutte. Tutti questi tipi funzionano al meglio nello strumento client principale per la visualizzazione e l’utilizzo con i PDF: Adobe Acrobat DC.

Di seguito è riportato un esempio di file PDF/A in Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

I file di esempio possono essere [scaricato da qui](assets/pdf-file-types.zip)

## PDF dell&#39;architettura XML Forms (PDF XFA)

In Adobe, il termine modulo XFA PDF viene utilizzato per fare riferimento al Forms interattivo e dinamico creato con AEM Forms Designer. Il Forms e i file creati con Designer si basano sull’architettura XFA (XML Forms Architecture) di Adobe. In molti modi, il formato di file PDF XFA è più vicino a un file HTML che a un file PDF tradizionale. Il codice seguente, ad esempio, mostra l&#39;aspetto di un oggetto di testo semplice in un file PDF XFA.

![Campo di testo](assets/text-field.JPG)

I Forms XFA sono basati su XML. Questo formato ben strutturato e flessibile consente a un server AEM Forms di trasformare i file Designer in formati diversi, tra cui PDF tradizionale, PDF/A e HTML. È possibile visualizzare la struttura XML completa del Forms in Designer selezionando la scheda Origine XML dell&#39;Editor di layout. In AEM Forms Designer è possibile creare Forms XFA sia statici che dinamici.

## Static PDF

Il layout dei PDF forms XFA statici non cambia mai in fase di runtime, ma può essere interattivo per l’utente. Di seguito sono riportati alcuni vantaggi dei PDF forms XFA statici:

* Il layout dei PDF forms XFA statici non cambia mai in fase di runtime, ma può essere interattivo per l’utente.
* Forms statico supporta gli strumenti di commento e markup di Acrobat.
* Forms statico consente di importare ed esportare commenti Acrobat.
* Forms statico supporta l’impostazione secondaria del font, una tecnica che può essere eseguita su un server AEM Forms.
* È possibile eseguire il rendering di Forms statici utilizzando il visualizzatore di PDF integrato fornito con i browser moderni.

>[!NOTE]
>
> È possibile creare PDF statici utilizzando AEM Forms Designer salvando XDP come Adobe Static PDF Form



### Dynamic Forms

I PDF XFA dinamici possono modificare il layout in fase di runtime, pertanto le funzioni di commento e markup non sono supportate. Tuttavia, i PDF XFA dinamici offrono i seguenti vantaggi:

* I moduli dinamici supportano script lato client che modificano il layout e l’impaginazione del modulo. Ad esempio, il file Purchase Order.xdp si espanderà e impaginerà per contenere una quantità infinita di dati se vengono salvati come modulo dinamico
* I moduli dinamici supportano tutte le proprietà del modulo in fase di esecuzione, mentre i moduli statici supportano solo un sottoinsieme

>[!NOTE]
>
> È possibile creare file PDF dinamici utilizzando AEM Forms Designer salvando il file XDP come Adobe di modulo XML dinamico

>[!NOTE]
>
> Non è possibile eseguire il rendering dei moduli dinamici utilizzando i visualizzatori PDF incorporati dei browser moderni.

### File PDF (Traditional PDF)

Un documento certificato fornisce ai destinatari di documenti PDF e Forms ulteriori garanzie di autenticità e integrità.

Il formato PDF più diffuso e diffuso è il file PDF tradizionale. Esistono diversi modi per creare un file PDF tradizionale, incluso l’utilizzo di Acrobat e di molti strumenti di terze parti. Acrobat offre tutti i seguenti modi per creare file PDF tradizionali. Se Acrobat non è installato, è possibile che queste opzioni non vengano visualizzate nel computer.

* Acquisendo il flusso di stampa di un’applicazione desktop: scegli il comando Stampa di un’applicazione di authoring e seleziona l’icona della stampante Adobe PDF. Anziché una copia stampata del documento, sarà stato creato un file PDF del documento
* Utilizzando il plug-in Acrobat PDFMaker con le applicazioni Microsoft Office: quando si installa Acrobat, viene aggiunto un menu Adobe PDF alle applicazioni Microsoft Office e un&#39;icona alla barra multifunzione Office. È possibile utilizzare le funzionalità aggiunte per creare file PDF direttamente in Microsoft Office
* Utilizzando Acrobat Distiller per convertire i file Postscript e Incapsulated Postscript (EPS) in PDF: Distiller viene generalmente utilizzato nella pubblicazione stampata e in altri flussi di lavoro che richiedono una conversione dal formato Postscript al formato PDF
* Sotto il cofano, un PDF tradizionale è molto diverso da un PDF XFA. Non ha la stessa struttura XML e poiché viene creato catturando il flusso di stampa di un file, un PDF tradizionale è un file statico e di sola lettura.

Un documento certificato fornisce ai PDF documenti e forma i destinatari con ulteriori garanzie sulla sua autenticità e integrità.

### Acroform

Acroforms è la tecnologia di moduli interattivi precedente di Adobe; risale alla versione 3 di Acrobat. L&#39;Adobe fornisce [Riferimento API di Acrobat Forms](assets/FormsAPIReference.pdf), del maggio 2003, per fornire i dettagli tecnici di questa tecnologia. Le acroforme sono una combinazione dei seguenti elementi:

* PDF tradizionale che definisce il layout statico e gli elementi grafici del modulo.
* Campi modulo interattivi aggiunti tramite gli strumenti modulo del programma Adobe Acrobat. Questi strumenti di modulo sono un piccolo sottoinsieme di ciò che è disponibile in AEM Forms Designer.

### PDF/A (PDF per l&#39;archiviazione)

PDF/A (PDF for Archives) si basa sui vantaggi dell&#39;archiviazione dei documenti dei PDF tradizionali con molti dettagli specifici che migliorano l&#39;archiviazione a lungo termine. Il formato di file PDF tradizionale offre numerosi vantaggi per l&#39;archiviazione dei documenti a lungo termine. La compattezza di PDF facilita il trasferimento e consente di risparmiare spazio; inoltre, la sua struttura ottimale offre potenti funzionalità di indicizzazione e ricerca. La versione tradizionale di PDF supporta ampiamente i metadati, mentre la versione di PDF supporta ambienti informatici diversi da molto tempo.

Come PDF, PDF/A è una specifica standard ISO. È stato sviluppato da una task force che comprendeva AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) e l&#39;Ufficio Amministrativo degli Stati Uniti. Poiché l’obiettivo delle specifiche PDF/A è quello di fornire un formato di archivio a lungo termine, molte funzioni PDF vengono omesse in modo che i file possano essere autonomi. Di seguito sono riportati alcuni punti chiave della specifica che migliorano la riproducibilità a lungo termine del file PDF/A:

* Tutto il contenuto deve essere contenuto nel file e non ci possono essere dipendenze da fonti esterne come collegamenti ipertestuali, font o programmi software.
* Tutti i tipi di carattere devono essere incorporati e devono essere tipi di carattere con una licenza di utilizzo illimitata per documenti elettronici.
* JavaScript non è consentito
* Trasparenza non consentita
* Crittografia non consentita
* I contenuti audio e video non sono consentiti
* Gli spazi colore devono essere definiti in modo indipendente dal dispositivo
* Tutti i metadati devono rispettare determinati standard

### Visualizzazione di un file PDF/A

Due file nei file di esempio sono stati creati dallo stesso file di Microsoft Word. Una è stata creata come PDF tradizionale e l’altra come file PDF/A. Apri questi due file in Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Anche se i documenti hanno lo stesso aspetto, il file PDF/A viene aperto con una barra blu nella parte superiore, che indica che il documento è visualizzato in modalità PDF/A. Questa barra blu è la barra dei messaggi del documento di Acrobat, che viene visualizzata quando si aprono determinati tipi di file PDF.

![Pdf-img](assets/pdfa-message.png)

La barra dei messaggi del documento include istruzioni ed eventualmente pulsanti per il completamento di un&#39;operazione. È codificato da un colore e quando apri tipi speciali di PDF (come questo file PDF/A) e PDF certificati e con firma digitale vedrai il colore blu. La barra diventa viola per i PDF forms e gialla quando partecipi a una revisione PDF.

>[!NOTE]
>
> Se fai clic su Abilita modifica, questo documento non sarà più conforme a PDF/A.
