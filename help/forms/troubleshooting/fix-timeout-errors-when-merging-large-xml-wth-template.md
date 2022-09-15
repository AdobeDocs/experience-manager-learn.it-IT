---
title: Correggere gli errori di timeout durante l'unione di file di dati xml di grandi dimensioni con il modello xdp
description: Unisci file xml di grandi dimensioni con un modello in AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# Come abilitare la creazione di file pdf unendo file di dati xml di grandi dimensioni con modelli xdp

Quando si uniscono file di dati xml di grandi dimensioni con il modello xdp, si può vedere il seguente errore nel file di log

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Per correggere l&#39;errore precedente, procedi come segue

## Modificare il timeout delle variabili

* Arresta AEM server
* Crea una cartella denominata **installare** nella cartella crx-quickstart dell&#39;installazione AEM
* Crea un file denominato **org.apache.aries.transaction.config** con il seguente contenuto aries.transaction.timeout=&quot;1200&quot; nella cartella di installazione. Puoi modificare il valore del timeout in base alle tue esigenze. Il valore di timeout è espresso in secondi

>[!NOTE]
> Una volta creata la configurazione org.apache.aries.transaction, puoi modificare i valori di timeout della transazione dalla [configMgr](http://localhost:4502/system/console/configMgr) invece di modificare il file


## Modificare le impostazioni del provider Jacorb ORB

* [Apri OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* Cerca **Provider ORB Jacorb**
* Aggiungi la seguente voce jacorb.connection.client.pending_reply_timeout=600000 L&#39;impostazione precedente imposta il timeout della risposta in sospeso (noto anche come timeout client CORBA) a 600 secondi.
* Salva le modifiche
