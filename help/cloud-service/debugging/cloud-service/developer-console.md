---
title: Console per sviluppatori
description: AEM as a Cloud Service fornisce una Console per sviluppatori per ogni ambiente che espone vari dettagli del servizio AEM in esecuzione utili per il debug.
feature: Strumenti per gli sviluppatori
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Sviluppo
role: Developer (Sviluppatore)
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1349'
ht-degree: 0%

---


# Debug di AEM as a Cloud Service con la Console per sviluppatori

AEM as a Cloud Service fornisce una Console per sviluppatori per ogni ambiente che espone vari dettagli del servizio AEM in esecuzione utili per il debug.

Ogni ambiente AEM as a Cloud Service dispone di una propria Console per sviluppatori.

## Accesso a Developer Console

Per accedere e utilizzare la Console per sviluppatori, è necessario assegnare le seguenti autorizzazioni all’Adobe ID dello sviluppatore tramite [Admin Console di Adobe](https://adminconsole.adobe.com).

1. Verifica che l’organizzazione Adobe che ha interessato i prodotti Cloud Manager e AEM as a Cloud Service sia attiva nel commutatore Adobe Org.
1. Lo sviluppatore deve essere membro del profilo di prodotto __Developer - Cloud Service__ del prodotto Cloud Manager.
   + Se l’iscrizione non esiste, lo sviluppatore non sarà in grado di accedere a Developer Console.
1. Lo sviluppatore deve essere membro del profilo di prodotto del servizio AEM Author and Publish __Amministratori AEM__.
   + Se l&#39;appartenenza non esiste, le immagini [status](#status) scadranno con un errore 401 Non autorizzato.

### Risoluzione dei problemi di accesso a Developer Console

#### 401 Errore non autorizzato in caso di dumping

![Console per sviluppatori - 401 non autorizzato](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se scarichi uno stato viene segnalato un errore 401 non autorizzato, significa che l’utente non esiste ancora con le autorizzazioni necessarie in AEM as a Cloud Service o che l’utilizzo dei token di accesso non è valido o è scaduto.

Per risolvere il problema 401 Non autorizzato:

1. Assicurati che il tuo utente sia membro del profilo di prodotto Adobe IMS appropriato (Amministratori AEM o Utenti AEM) per l’istanza di prodotto AEM as a Cloud Service associata alla Console per sviluppatori.
   + Ricorda che Developer Console accede a 2 istanze di prodotto Adobe IMS; le istanze di prodotto Author e Publish di AEM as a Cloud Service, quindi assicurati che vengano utilizzati i profili di prodotto corretti a seconda del livello di servizio che richiede l’accesso tramite Developer Console.
1. Accedi ad AEM as a Cloud Service (Author o Publish) e assicurati che l’utente e i gruppi siano stati sincronizzati correttamente in AEM.
   + Developer Console richiede la creazione del record utente nel corrispondente livello di servizio AEM per l’autenticazione a tale livello di servizio.
1. Elimina i cookie del browser e lo stato dell’applicazione (archiviazione locale) e accedi nuovamente a Developer Console, garantendo che il token di accesso utilizzato da Developer Console sia corretto e non sia scaduto.

## Pod

I servizi Author e Publish di AEM as a Cloud Service sono costituiti rispettivamente da più istanze per gestire la variabilità del traffico e gli aggiornamenti continui senza downtime. Queste istanze sono denominate Pods. La selezione del contenitore in Developer Console definisce l’ambito dei dati che verranno esposti tramite gli altri controlli.

![Console per sviluppatori - Pod](./assets/developer-console/pod.png)

+ Un pod è un’istanza discreta che fa parte di un servizio AEM (Author or or Publish)
+ I contenitori sono transitori, il che significa che AEM as a Cloud Service li crea e li distrugge in base alle esigenze
+ Sono elencati solo i pod che fanno parte dell’ambiente AEM as a Cloud Service associato allo switcher Pod della Console per sviluppatori dell’ambiente.
+ Nella parte inferiore dello switcher Pod, le opzioni di comodità consentono di selezionare i pod in base al tipo di servizio:
   + Tutti gli autori
   + Tutti gli editori
   + Tutte le istanze

## Stato

Lo stato fornisce opzioni per l’output di uno stato di runtime AEM specifico nel testo o nell’output JSON. La Console per sviluppatori fornisce informazioni simili alla console Web OSGi locale dell’SDK AEM, con la differenza marcata che la Console per sviluppatori è di sola lettura.

![Console per sviluppatori - Stato](./assets/developer-console/status.png)

### Bundle

I bundle elencano tutti i bundle OSGi in AEM. Questa funzionalità è simile ai [bundle OSGi di AEM SDK locali per quickstart](http://localhost:4502/system/console/bundles) in `/system/console/bundles`.

I bundle aiutano a eseguire il debug:

+ Elencare tutti i bundle OSGi implementati in AEM as a Service
+ Elencare lo stato di ciascun bundle OSGi; anche se sono attivi o meno
+ Fornire dettagli sulle dipendenze non risolte che causano l’attivazione dei bundle OSGi

### Componenti

Componenti elenca tutti i componenti OSGi in AEM. Questa funzionalità è simile ai [componenti OSGi locali di AEM SDK ](http://localhost:4502/system/console/components) all&#39;indirizzo `/system/console/components` di .

I componenti sono utili per il debug:

+ Elenco di tutti i componenti OSGi implementati in AEM as a Cloud Service
+ Fornire lo stato di ciascun componente OSGi; anche se sono attivi o insoddisfatti
+ Fornire dettagli in riferimenti di servizio non soddisfatti può causare l&#39;attivazione dei componenti OSGi
+ Elenco delle proprietà OSGi e dei loro valori associati al componente OSGi

### Configurazioni

Le configurazioni elencano tutte le configurazioni del componente OSGi (proprietà e valori OSGi). Questa funzionalità è simile a [OSGi Configuration Manager locale di AEM SDK](http://localhost:4502/system/console/configMgr) all&#39;indirizzo `/system/console/configMgr`.

Le configurazioni sono utili per il debug:

+ Elencare le proprietà OSGi e i loro valori dal componente OSGi
+ Individuazione e identificazione delle proprietà non configurate

### Indici Oak

Gli indici Oak forniscono un’immagine dei nodi definiti sotto `/oak:index`. Tieni presente che questo non mostra gli indici uniti, che si verifica quando un indice AEM viene modificato.

Aiuto per gli indici Oak nel debug tramite:

+ Elencando tutte le definizioni di Oak Index che forniscono informazioni approfondite sul modo in cui le query di ricerca vengono eseguite in AEM. Tieni presente che gli indici AEM modificati non vengono riportati qui. Questa visualizzazione è utile solo per gli indici forniti esclusivamente da AEM o unicamente dal codice personalizzato.

### Servizi OSGi

Componenti elenca tutti i servizi OSGi. Questa funzionalità è simile ai servizi OSGi di AEM SDK [in `/system/console/services` all&#39;indirizzo  di ](http://localhost:4502/system/console/services).

Guida dei servizi OSGi per il debug:

+ Elencare tutti i servizi OSGi in AEM, insieme al suo bundle OSGi che fornisce, e tutti i bundle OSGi che lo utilizzano

### Processi Sling

In Processi Sling sono elencate tutte le code di Processi Sling. Questa funzionalità è simile ai processi dell&#39;avvio rapido locale dell&#39;SDK AEM](http://localhost:4502/system/console/slingevent) in `/system/console/slingevent`.[

La guida dei processi Sling per il debug è disponibile nei seguenti modi:

+ Elenco delle code di lavoro Sling e delle relative configurazioni
+ Fornisce informazioni sul numero di lavori Sling attivi, in coda ed elaborati, utile per il debug dei problemi con Flusso di lavoro, Flusso di lavoro transitorio e altri lavori eseguiti da Processi Sling in AEM.

## Pacchetti Java

I pacchetti Java consentono di verificare se un pacchetto Java e una versione sono disponibili per l’uso in AEM as a Cloud Service. Questa funzionalità è la stessa di [AEM SDK&#39;s local quickstart&#39;s Dependency Finder](http://localhost:4502/system/console/depfinder) all&#39;indirizzo `/system/console/depfinder`.

![Console per sviluppatori - Pacchetti Java](./assets/developer-console/java-packages.png)

I pacchetti Java vengono utilizzati per risolvere i problemi di avvio I bundle non vengono avviati a causa di importazioni non risolte o classi non risolte negli script (HTL, JSP, ecc.). Se i pacchetti Java segnalano che nessun bundle esporta un pacchetto Java (o la versione non corrisponde a quella importata da un bundle OSGi):

+ Assicurati che la versione della dipendenza AEM API maven del progetto corrisponda alla versione di AEM Release dell’ambiente (e, se possibile, aggiorna tutto all’ultima versione).
+ Se nel progetto Maven vengono utilizzate dipendenze Maven aggiuntive
   + Determina se è invece possibile utilizzare un’API alternativa fornita dalla dipendenza API SDK AEM.
   + Se la dipendenza extra è necessaria, assicurati che sia fornita come bundle OSGi (anziché come Jar normale) e sia incorporata nel pacchetto di codice del progetto, (`ui.apps`), in modo simile al modo in cui il bundle OSGi principale è incorporato nel pacchetto `ui.apps`.

## Servlet

I servlet forniscono informazioni dettagliate su come AEM risolve un URL di un servlet Java o di uno script (HTL, JSP) che gestisce in ultima analisi la richiesta. Questa funzionalità è la stessa di [Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) dell&#39;SDK AEM su `/system/console/servletresolver`.

![Console per sviluppatori - Servlet](./assets/developer-console/servlets.png)

I servlet consentono di determinare nel debug:

+ Modalità di decomposizione di un URL nelle relative parti indirizzabili (risorsa, selettore, estensione).
+ A quale servlet o script viene risolto un URL, per identificare gli URL formati male o i servlet/script non registrati.

## Query

Le query forniscono informazioni approfondite su cosa e come vengono eseguite le query di ricerca su AEM. Questa funzionalità è la stessa della console locale di quickstart Tools > Query Performance ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) dell’SDK di AEM.[

Le query funzionano solo quando è selezionato un pod specifico, in quanto apre la console web Query Performance del pod, che richiede allo sviluppatore di avere accesso per accedere al servizio AEM.

![Console per sviluppatori - Query - Spiegare query](./assets/developer-console/queries__explain-query.png)

Le query consentono di eseguire il debug in base a:

+ Spiegazione di come le query vengono interpretate, analizzate ed eseguite da Oak. Questo è molto importante quando si tiene traccia del motivo per cui una query è lenta e si capisce come può essere accelerata.
+ Elencando le query più comuni in esecuzione in AEM, con la possibilità di Spiegarle.
+ Elencando le query più lente in esecuzione in AEM, con la possibilità di spiegarle.
