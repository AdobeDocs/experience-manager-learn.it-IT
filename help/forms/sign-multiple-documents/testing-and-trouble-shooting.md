---
title: Risoluzione dei problemi relativi alla firma di più documenti
description: Test e risoluzione dei problemi relativi alla soluzione
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
duration: 86
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Test e risoluzione dei problemi


## Anteprima del modulo di rifinanziamento

Il caso d’uso viene attivato quando l’agente del servizio clienti compila e invia [modulo di rifinanziamento](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

Il flusso di lavoro Firma più Forms viene attivato all’invio di questo modulo e il cliente riceve una notifica e-mail con un collegamento per avviare il processo di compilazione e firma del modulo.

## Compila i moduli nella confezione

Il cliente è invitato a compilare e firmare il primo modulo della confezione. Dopo aver firmato correttamente il modulo, il cliente può passare al modulo successivo nel pacchetto. Una volta compilati e firmati tutti i moduli, al cliente viene consegnato &quot;**AllDone**&quot;.

## Risoluzione dei problemi

### La notifica e-mail non viene generata

La notifica e-mail viene inviata dal componente Invia e-mail nel flusso di lavoro Firma più moduli. Se uno dei passaggi di questo flusso di lavoro non riesce, viene inviata la notifica e-mail. Verificare che il passaggio del processo personalizzato nel flusso di lavoro crei righe nel database MySQL. Se le righe vengono create, controlla le impostazioni di configurazione del servizio di posta Day CQ

### Il collegamento nella notifica e-mail non funziona

I collegamenti nelle notifiche e-mail vengono generati in modo dinamico. Se il server AEM non è in esecuzione su localhost:4502, specificare il nome del server e la porta corretti negli argomenti del passaggio Archivia Forms per firmare del flusso di lavoro Firma più Forms

### Impossibile firmare il modulo

Ciò può verificarsi se il modulo non è stato popolato correttamente recuperando i dati dall’origine dati. Controlla i registri di stdout del server. Il file fetchformdata.jsp scrive alcuni messaggi utili allo stdout.

### Impossibile passare al modulo successivo nel pacchetto

Dopo aver firmato correttamente un modulo nel pacchetto, viene attivato il flusso di lavoro Aggiorna stato firma. Il primo passaggio del flusso di lavoro aggiorna lo stato della firma del modulo nel database. Verifica se lo stato del modulo è aggiornato da 0 a 1.

### Mancata visualizzazione della maschera AllDone

Quando non sono presenti altri moduli per l&#39;accesso al pacchetto, il modulo AllDone viene presentato all&#39;utente.Se il modulo AllDone non viene visualizzato, controllare l&#39;URL utilizzato nella riga 33 del file GetNextFormToSign.js che fa parte del **getnextform** libreria client.
