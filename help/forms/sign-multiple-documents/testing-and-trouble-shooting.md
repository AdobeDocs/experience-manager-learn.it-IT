---
title: Risoluzione dei problemi relativi alla firma di più documenti
description: Test e problemi nella soluzione
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# Test e risoluzione dei problemi


## Anteprima del modulo di rifinanziamento

Il caso d&#39;uso viene attivato quando l&#39;agente del servizio clienti compila e invia [modulo di rifinanziamento](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

Il flusso di lavoro Firma più moduli viene attivato all’invio del modulo e il cliente riceve una notifica via e-mail con un collegamento che consente di avviare il processo di compilazione e firma del modulo.

## Compila i moduli nel pacchetto

Al cliente viene presentato di compilare e firmare il primo modulo del pacchetto. Dopo la firma del modulo, il cliente può passare al modulo successivo del pacchetto. Una volta compilati e firmati tutti i moduli, al cliente viene presentato il modulo &quot;**AllDone**&quot;.

## Risoluzione dei problemi

### La notifica via e-mail non viene generata

La notifica e-mail viene inviata dal componente Invia e-mail nel flusso di lavoro Firma più moduli . Se uno dei passaggi del flusso di lavoro non riesce, la notifica e-mail verrà inviata. Assicurati che il passaggio del processo personalizzato nel flusso di lavoro stia creando righe nel database MySQL. Se le righe vengono create, controlla le impostazioni di configurazione di Day CQ Mail Service

### Il collegamento nella notifica di posta elettronica non funziona

I collegamenti nelle notifiche e-mail vengono generati in modo dinamico. Se il server AEM non è in esecuzione su localhost:4502, fornisci il nome server e la porta corretti negli argomenti del passaggio Store Forms To Sign del flusso di lavoro Sign Multiple Forms

### Impossibile firmare il modulo

Ciò può accadere se il modulo non è stato compilato correttamente recuperando i dati dall’origine dati. Controlla i log stdout del tuo server. Il fetchformdata.jsp scrive alcuni messaggi utili allo stdout.

### Impossibile passare al modulo successivo nel pacchetto

Dopo aver firmato correttamente un modulo nel pacchetto, viene attivato il flusso di lavoro Aggiorna stato firma . Il primo passaggio nel flusso di lavoro aggiorna lo stato di firma del modulo nel database. Verificare che lo stato del modulo sia aggiornato da 0 a 1.

### Mancata visualizzazione del modulo AllDone

Se non sono presenti altri moduli da firmare nel pacchetto, all&#39;utente viene presentato il modulo AllDone. Se non è presente il modulo AllDone, controlla l&#39;URL utilizzato alla riga 33 del file GetNextFormToSign.js che fa parte della libreria client **getnextform**.











