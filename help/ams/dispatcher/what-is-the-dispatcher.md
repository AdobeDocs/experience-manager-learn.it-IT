---
title: Cos’è "Dispatcher"
description: Sappiate cos'è in realtà un Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# Cos’è &quot;Dispatcher&quot;

[Sommario](./overview.md)

A partire dalla descrizione di base di ciò che comporta un Dispatcher per l’AEM.

## Server web Apache

Inizia con un’installazione di base del server web Apache su un server Linux.

Spiegazione di base delle funzioni di un server Apache:

- Seguono regole semplici per distribuire i file tramite i protocolli HTTP(s) dalla directory dei documenti statici (`DocumentRoot`)
- I file archiviati in un percorso predefinito (`/var/www/html`) vengono associati alle richieste e sottoposti a rendering nel browser del client richiedente




## File modulo specifico per AEM (`mod_dispatcher.so`)

Quindi aggiungi al server web Apache un plug-in denominato modulo Dispatcher.

Spiegazione di base delle funzioni del modulo Dispatcher dell’AEM di Adobe:

- Aumenta il gestore di file predefinito
- Filtra le richieste non valide / Protegge la pancia morbida/gli endpoint dell’AEM
- Bilanciamenti del carico se è presente più di un renderer
- Consente la creazione di una directory di cache attiva / Supporta lo scaricamento di file stagnanti
- È la porta d&#39;ingresso di tutte le installazioni AMS e fornisce siti web e risorse al browser del cliente
- Memorizza nella cache le richieste di reindirizzamento a una velocità molto più elevata di quella che un server AEM potrebbe eseguire da solo
- Molto altro...

## Flusso di lavoro per traffico web

Sapendo quali pezzi vengono installati insieme per creare un server Dispatcher di base, possiamo comprendere il flusso di lavoro del traffico web di base per una configurazione dei servizi Adobe Manager.
Questo dovrebbe aiutarti a capire quale ruolo svolge nella catena di sistemi che distribuiscono contenuti ai visitatori dei tuoi contenuti AEM.

<b>Contenuto già memorizzato nella cache</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Distribuzione di contenuto fresco da AEM</b>

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

<b>Pubblicazione contenuti/modifiche</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Avanti -> Layout file di base](./basic-file-layout.md)
