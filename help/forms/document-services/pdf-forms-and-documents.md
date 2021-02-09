---
title: Comprendere i diversi tipi di PDF forms e documenti
description: PDF è in realtà una famiglia di formati di file e questo articolo descrive i tipi di PDF importanti e rilevanti per gli sviluppatori di moduli.
solution: Experience Manager Forms
product: aem
type: Documentation
role: Developer
level: Beginner,Intermediate
version: 6.3,6.4,6.5
feature: Document Services
topic: development
kt: 7071
translation-type: tm+mt
source-git-commit: 1e945afddda3d7735005029952a9d7ec46828bc6
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---


# Formati PDF

Il formato PDF (Portable Document Format) è in realtà una famiglia di formati di file e questo articolo descrive in dettaglio quelli più rilevanti per gli sviluppatori di moduli. Molti dei dettagli tecnici e degli standard dei diversi tipi di PDF sono in evoluzione e in evoluzione. Alcuni di questi formati e specifiche sono standard ISO (International Organization for Standardization) e alcuni sono proprietà intellettuale specifica di  Adobe.

Questo articolo mostra come creare vari tipi di PDF. Ti aiuterà a capire come e perché usarli. Tutti questi tipi funzionano al meglio con lo strumento client principale per la visualizzazione e l’utilizzo dei PDF,  Adobe Acrobat DC.

Questo è un esempio di file PDF/A in  Acrobat DC.
![pdfa](assets/pdfa-file-in-acrobat.png)

I file di esempio possono essere [scaricati da qui](assets/pdf-file-types.zip)

## PDF XFA

 Adobe utilizza il termine modulo PDF per fare riferimento ai moduli interattivi e dinamici creati con  AEM Forms Designer. È importante notare che esiste un altro tipo di modulo PDF, denominato Acrobat, che è diverso dai PDF forms creati in  AEM Forms Designer. I moduli e i file creati con Designer sono basati su XML Forms Architecture (XFA)  Adobe. In molti modi, il formato file PDF XFA è più simile a un file HTML che a un file PDF tradizionale. Ad esempio, il codice seguente mostra l&#39;aspetto di un semplice oggetto di testo in un file PDF XFA.

![text-field](assets/text-field.JPG)

Come si può vedere, i moduli XFA sono basati su XML. Questo formato strutturato e flessibile consente a un  AEM Forms Server di trasformare i file di Designer in diversi formati, inclusi PDF, PDF/A e HTML tradizionali. Per visualizzare la struttura XML completa dei moduli in Designer, selezionare la scheda Sorgente XML dell&#39;Editor di layout. È possibile creare moduli XFA statici e dinamici in  AEM Forms Designer.

## PDF statico

Gli PDF forms XFA statici non modificano il layout in fase di esecuzione, ma possono essere interattivi per l&#39;utente. Di seguito sono riportati alcuni vantaggi offerti dagli PDF forms XFA statici:

* Gli PDF forms XFA statici non modificano il layout in fase di esecuzione, ma possono essere interattivi per l&#39;utente.
* I moduli statici supportano  strumenti Commenti e Marcatura di Acrobat.
* I moduli statici consentono di importare ed esportare  commenti Acrobat.
* I moduli statici supportano l&#39;impostazione secondaria dei font, una tecnica che può essere eseguita su un server AEM Forms .
* È possibile eseguire il rendering dei moduli statici utilizzando il visualizzatore PDF incorporato fornito con i browser più recenti.

>[!NOTE]
> È possibile creare PDF statici utilizzando  AEM Forms Designer salvando XDP come modulo PDF statico  Adobe

## Forms dinamico

I PDF XFA dinamici possono modificare il layout in fase di esecuzione, pertanto le funzioni di marcatura e marcatura non sono supportate. Tuttavia, i PDF XFA dinamici offrono i seguenti vantaggi:

* I moduli dinamici supportano script sul lato client che modificano il layout e l&#39;impaginazione del modulo. Ad esempio, Purchase Order.xdp si espanderà e impaginerà per contenere una quantità infinita di dati se viene salvata come modulo dinamico
* I moduli dinamici supportano tutte le proprietà del modulo in fase di esecuzione, mentre i moduli statici supportano solo un sottoinsieme


>[!NOTE]
> È possibile creare file PDF dinamici utilizzando  AEM Forms Designer salvando XDP come modulo XML dinamico  Adobe

>[!NOTE]
> Non è possibile eseguire il rendering dei moduli dinamici utilizzando i visualizzatori PDF integrati dei browser più recenti.


## File PDF (PDF tradizionale)

Il formato PDF più diffuso è il file PDF tradizionale. Esistono diversi modi per creare un file PDF tradizionale, compreso l’uso di  Acrobat e di molti strumenti di terze parti.  Acrobat offre tutti i seguenti metodi per creare file PDF tradizionali. Se  Acrobat non è installato, è possibile che nel computer in uso non vengano visualizzate le seguenti opzioni.

* Acquisendo il flusso di stampa di un’applicazione desktop: Scegliete il comando Stampa di un’applicazione di creazione e selezionate l’icona  stampante Adobe PDF. Invece di una copia stampata del documento, sarà stato creato un file PDF del documento
* Utilizzando  plug-in Acrobat PDFMaker con le applicazioni Microsoft Office: Quando installate  Acrobat, viene aggiunto un menu Adobe PDF  alle applicazioni di Microsoft Office e un&#39;icona alla barra multifunzione di Office. Potete utilizzare queste funzionalità aggiunte per creare file PDF direttamente in Microsoft Office
* Utilizzando  Acrobat Distiller per convertire file PostScript e Encapsulated Postscript (EPS) in PDF: Distiller viene in genere utilizzato per la pubblicazione di stampe e in altri flussi di lavoro che richiedono una conversione dal formato PostScript al formato PDF
* Sotto il cofano, un PDF tradizionale è molto diverso da un PDF XFA. Non ha la stessa struttura XML e, poiché viene creata acquisendo il flusso di stampa di un file, un PDF tradizionale è un file statico e di sola lettura.

Un documento certificato fornisce ai destinatari di documenti PDF e moduli ulteriori garanzie di autenticità e integrità.

## Acroformi

Gli acroformi sono  vecchia tecnologia di moduli interattivi del Adobe; risalgono alla  Acrobat versione 3.  Adobe fornisce il [ Acrobat Forms API Reference](assets/FormsAPIReference.pdf), datato maggio 2003, per fornire i dettagli tecnici per questa tecnologia. Gli acroformi sono una combinazione di
seguenti elementi:

* Un PDF tradizionale che definisce il layout statico e gli elementi grafici del modulo.
* Campi modulo interattivi con collegamento in primo piano con gli strumenti modulo del programma Adobe Acrobat . Tenere presente che questi strumenti modulo costituiscono un piccolo sottoinsieme di ciò che è disponibile in  AEM Forms Designer.

## PDF/A (PDF per l’archiviazione)

PDF/A (PDF per archivi) sfrutta i vantaggi dell’archiviazione dei documenti dei PDF tradizionali con molti dettagli specifici che migliorano l’archiviazione a lungo termine. Il formato PDF tradizionale offre molti vantaggi per l&#39;archiviazione a lungo termine dei documenti. La compattezza del formato PDF facilita il trasferimento e consente di risparmiare spazio, e la sua strutturazione consente di effettuare operazioni di indicizzazione e ricerca avanzate. I PDF tradizionali supportano ampiamente i metadati e i PDF hanno una lunga storia di supporto a diversi ambienti di computer.

Come per i PDF, PDF/A è una specifica standard ISO. È stato sviluppato da una task force che comprendeva AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) e l&#39;ufficio amministrativo dei tribunali statunitensi. Poiché l&#39;obiettivo della specifica PDF/A è quello di fornire un formato di archivio a lungo termine, molte funzioni PDF vengono omesse in modo che i file possano essere standalone. Di seguito sono riportati alcuni punti chiave della specifica che migliorano la riproducibilità a lungo termine del file PDF/A:

* Tutto il contenuto deve essere contenuto nel file e non possono esservi dipendenze da origini esterne come collegamenti ipertestuali, font o programmi software.
* Tutti i font devono essere incorporati e devono essere font con una licenza d&#39;uso illimitato per i documenti elettronici.
* JavaScript non è consentito
* Trasparenza non consentita
* Crittografia non consentita
* I contenuti audio e video non sono consentiti
* Gli spazi colore devono essere definiti in modo indipendente dal dispositivo
* Tutti i metadati devono rispettare determinati standard

## Visualizzazione di un file PDF/A

Due file di esempio sono stati creati dallo stesso file di Microsoft Word. Uno è stato creato come PDF tradizionale e l’altro come file PDF/A. Aprite questi due file in  Acrobat Professional:

simpleWordFile.pdf
simpleWordFilePDFA.pdf

Anche se i documenti hanno lo stesso aspetto, il file PDF/A si apre con una barra blu nella parte superiore, a indicare che il documento viene visualizzato in modalità PDF/A. Questa barra blu è  barra dei messaggi del documento di Acrobat, che verrà visualizzata all’apertura di alcuni tipi di file PDF.

![pdf-img](assets/pdfa-message.png)

La barra dei messaggi del documento include istruzioni ed eventualmente pulsanti per completare un&#39;attività. Viene codificata in base al colore e viene visualizzato il colore blu all’apertura di tipi speciali di PDF (come questo file PDF/A), nonché di PDF certificati e firmati in modo digitale. Quando si partecipa a una revisione PDF, la barra diventa viola per i PDF forms e giallo.

>[!NOTE]
> Se fate clic su Abilita modifica, questo documento non sarà conforme allo standard PDF/A.




