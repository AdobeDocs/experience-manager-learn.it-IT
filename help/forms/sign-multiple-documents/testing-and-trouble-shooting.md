---
title: Risoluzione dei problemi relativi alla firma di più documenti
description: Test e problemi nella soluzione
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 1%

---


# Test e risoluzione dei problemi


## Anteprima del modulo di rifinanziamento

Il caso d&#39;uso viene attivato quando l&#39;agente del servizio clienti compila e invia il [modulo di rifinanziamento](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

Il flusso di lavoro Sign Multiple Forms viene attivato durante l&#39;invio del modulo e il cliente riceve una notifica e-mail con un collegamento per avviare il processo di compilazione e firma del modulo.

## Compilare moduli nel pacchetto

Il cliente viene presentato per compilare e firmare il primo modulo nel pacchetto. Dopo la firma del modulo, il cliente può passare al modulo successivo del pacchetto. Una volta compilati tutti i moduli e firmati, al cliente viene presentato il modulo &quot;**AllDone**&quot;.

## Risoluzione dei problemi

### La notifica e-mail non viene generata

La notifica e-mail viene inviata dal componente Invia e-mail nel flusso di lavoro Firma più moduli. Se uno dei passaggi del flusso di lavoro non riesce, la notifica e-mail viene inviata. Verificare che il passaggio del processo personalizzato nel flusso di lavoro sia la creazione di righe nel database MySQL. Se le righe vengono create, controllate le impostazioni di configurazione Day CQ Mail Service

### Il collegamento nella notifica e-mail non funziona

I collegamenti nelle notifiche e-mail vengono generati in modo dinamico. Se il server AEM non è in esecuzione su localhost:4502, fornire il nome server e la porta corretti negli argomenti del passaggio Store Forms To Sign del flusso di lavoro Sign Multiple Forms

### Impossibile firmare il modulo

Ciò può verificarsi se il modulo non è stato compilato correttamente recuperando i dati dall&#39;origine dati. Controllate i registri stdout del server. fetchformdata.jsp scrive alcuni messaggi utili allo stdout.

### Impossibile passare al modulo successivo nel pacchetto

Dopo l&#39;apposizione della firma a un modulo nel pacchetto, viene attivato il flusso di lavoro Aggiorna stato firma. Il primo passaggio del flusso di lavoro aggiorna lo stato di firma del modulo nel database. Verificare se lo stato del modulo è aggiornato da 0 a 1.

### Mancata visualizzazione del modulo AllDone

Se non sono presenti altri moduli da firmare nel pacchetto, allDone viene presentato all&#39;utente. Se il modulo AllDone non è visualizzato, controllare l&#39;URL utilizzato nella riga 33 del file GetNextFormToSign.js che fa parte della libreria client **getnextform**.











