---
title: Risoluzione dei problemi relativi alla firma di più documenti
description: Test e problemi nella soluzione
feature: Adaptive Forms
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 1%

---

# Test e risoluzione dei problemi


## Anteprima del modulo di rifinanziamento

Il caso d’uso viene attivato quando l’agente del servizio clienti compila e invia [modulo di rimborso](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

Il flusso di lavoro Firma multipla di Forms viene attivato all’invio del modulo e il cliente riceve una notifica e-mail con un collegamento che consente di avviare il processo di compilazione e firma del modulo.

## Compila i moduli nel pacchetto

Al cliente viene presentato di compilare e firmare il primo modulo del pacchetto. Dopo la firma del modulo, il cliente può passare al modulo successivo del pacchetto. Una volta compilati e firmati tutti i moduli, al cliente viene presentato il &quot;**AllDone**&quot; modulo.

## Risoluzione dei problemi

### La notifica via e-mail non viene generata

La notifica e-mail viene inviata dal componente Invia e-mail nel flusso di lavoro Firma più moduli . Se uno dei passaggi del flusso di lavoro non riesce, viene inviata la notifica via e-mail. Assicurati che il passaggio del processo personalizzato nel flusso di lavoro stia creando righe nel database MySQL. Se le righe vengono create, controlla le impostazioni di configurazione di Day CQ Mail Service

### Il collegamento nella notifica di posta elettronica non funziona

I collegamenti nelle notifiche e-mail vengono generati in modo dinamico. Se il server AEM non è in esecuzione su localhost:4502, fornisci il nome e la porta corretti negli argomenti del passaggio Store Forms To Sign del flusso di lavoro Sign Multiple Forms

### Impossibile firmare il modulo

Ciò può accadere se il modulo non è stato compilato correttamente recuperando i dati dall’origine dati. Controlla i log stdout del tuo server. Il fetchformdata.jsp scrive alcuni messaggi utili allo stdout.

### Impossibile passare al modulo successivo nel pacchetto

Dopo aver firmato correttamente un modulo nel pacchetto, viene attivato il flusso di lavoro Aggiorna stato firma . Il primo passaggio nel flusso di lavoro aggiorna lo stato di firma del modulo nel database. Verificare che lo stato del modulo sia aggiornato da 0 a 1.

### Mancata visualizzazione del modulo AllDone

Se non sono disponibili altri moduli per l&#39;accesso al pacchetto, allDone viene presentato all&#39;utente.Se non è presente il modulo AllDone, controllare l&#39;URL utilizzato nella riga 33 del file GetNextFormToSign.js che fa parte del file **getnextform** libreria client.
