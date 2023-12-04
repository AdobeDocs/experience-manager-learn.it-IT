---
title: Correggere gli errori di timeout durante l’unione di file di dati XML di grandi dimensioni con un modello XDP
description: Unione di file XML di grandi dimensioni con un modello in AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 58
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# Come abilitare la creazione di file PDF unendo file di dati XML di grandi dimensioni con modelli XDP

Quando si uniscono file di dati XML di grandi dimensioni con un modello XDP, è possibile che nel file di registro venga visualizzato il seguente errore

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Per correggere l&#39;errore precedente, eseguire le operazioni seguenti

## Modificare il timeout di aries

* Arresta server AEM
* Crea una cartella denominata **installare** nella cartella crx-quickstart dell&#39;installazione AEM
* Crea un file denominato **org.apache.aries.transaction.config** con i seguenti contenuti aries.transaction.timeout=&quot;1200&quot; nella cartella di installazione. Puoi modificare il valore di timeout in base alle tue esigenze. Il valore di timeout è in secondi

>[!NOTE]
> Una volta creata la configurazione org.apache.aries.transaction, è possibile modificare i valori di timeout della transazione da [configMgr](http://localhost:4502/system/console/configMgr) invece di modificare il file


## Modificare le impostazioni del provider Jacorb ORB

* [Apri OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* Cerca **Provider ORB Jacorb**
* Aggiungi la voce seguente jacorb.connection.client.pending_reply_timeout=600000 L’impostazione precedente imposta su 600 secondi il timeout della risposta in sospeso (noto anche come timeout del client CORBA).
* Salva le modifiche
