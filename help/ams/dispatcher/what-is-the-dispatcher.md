---
title: Cos’è "Il Dispatcher"
description: Scopri cos’è veramente un Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# Cos’è &quot;Il Dispatcher&quot;

[Sommario](./overview.md)

A partire dalla descrizione di base di ciò che comporta un Dispatcher AEM.

## Server web Apache

Inizia con un&#39;installazione di base di Apache Web Server su un server Linux.

Spiegazione di base sulle attività di un server Apache:

- Segue semplici regole per elaborare i file sui protocolli HTTP(s) dalla relativa directory dei documenti statici (`DocumentRoot`)
- File memorizzati in una posizione predefinita (`/var/www/html`) viene confrontata con le richieste ed è eseguito il rendering nel browser del client richiedente




## AEM file di modulo specifico (`mod_dispatcher.so`)

Quindi aggiungi un plug-in ad Apache Web Server chiamato modulo Dispatcher

Spiegazione di base del funzionamento del modulo Adobe AEM Dispatcher:

- Aumenta il gestore di file predefinito
- Filtra le richieste errate / Protegge AEM soft belly/endpoint
- Bilancia il carico se sono presenti più render
- Permette di gestire una directory cache attiva / Supporta lo scaricamento di file stagnanti
- È la porta principale per tutte le installazioni AMS e fornisce siti web e risorse al browser del cliente
- Memorizza in cache le richieste per rielaborare a una velocità molto più veloce di quanto un server AEM possa eseguire autonomamente
- Molto altro...

## Flusso di lavoro del traffico web

Per comprendere quali elementi vengono installati insieme per creare un server Dispatcher di base, è necessario conoscere il flusso di lavoro del traffico web di base per una configurazione di Adobe Manager Services.
Questo dovrebbe aiutarti a comprendere il ruolo che svolge nella catena di sistemi che distribuiscono contenuti ai visitatori del tuo contenuto AEM.

<b>Distribuzione di contenuti già memorizzati nella cache</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Servizio di contenuti freschi da AEM</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>Pubblicazione/modifiche del contenuto</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Avanti -> Layout dei file di base](./basic-file-layout.md)