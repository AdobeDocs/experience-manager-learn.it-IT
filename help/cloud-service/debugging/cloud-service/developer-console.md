---
title: Console per sviluppatori
description: AEM as a Cloud Service fornisce una Console per sviluppatori per ogni ambiente che espone vari dettagli del servizio AEM in esecuzione utili per il debug.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1348'
ht-degree: 0%

---

# Debug di AEM come Cloud Service con Developer Console

AEM as a Cloud Service fornisce una Console per sviluppatori per ogni ambiente che espone vari dettagli del servizio AEM in esecuzione utili per il debug.

Ogni AEM come ambiente di Cloud Service dispone di una propria Console per sviluppatori.

## Accesso a Developer Console

Per accedere e utilizzare la Console per sviluppatori, è necessario assegnare le seguenti autorizzazioni all’Adobe ID dello sviluppatore tramite [Admin Console di Adobe](https://adminconsole.adobe.com).

1. Assicurati che l’organizzazione di Adobe che ha effettuato Cloud Manager e AEM come prodotti di Cloud Service sia attiva nel commutatore dell’organizzazione di Adobe.
1. Lo sviluppatore deve essere membro del profilo di prodotto __Sviluppatore - Cloud Service__ di Cloud Manager.
   + Se l’iscrizione non esiste, lo sviluppatore non sarà in grado di accedere a Developer Console.
1. Lo sviluppatore deve essere membro del profilo di prodotto __AEM Utenti__ o __AEM Amministratori__ su AEM Author e/o Publish.
   + Se l&#39;appartenenza non esiste, le immagini [status](#status) scadranno con un errore 401 Non autorizzato.

### Risoluzione dei problemi di accesso a Developer Console

#### 401 Errore non autorizzato in caso di dumping

![Console per sviluppatori - 401 non autorizzato](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se viene segnalato un errore 401 Non autorizzato, significa che l’utente non esiste ancora con le autorizzazioni necessarie in AEM come Cloud Service o che l’utilizzo dei token di accesso non è valido o è scaduto.

Per risolvere il problema 401 Non autorizzato:

1. Assicurati che il tuo utente sia membro del profilo di prodotto Adobe IMS appropriato (amministratori AEM o utenti AEM) per il AEM associato della Console per sviluppatori come istanza di prodotto del Cloud Service.
   + Ricorda che Developer Console accede a 2 istanze di prodotto Adobe IMS; AEM istanze di prodotto Autore e pubblicazione di Cloud Service, assicurati che vengano utilizzati i profili di prodotto corretti a seconda del livello di servizio che richiede l’accesso tramite Console per sviluppatori.
1. Accedi al AEM come Cloud Service (Author o Publish) e assicurati che l’utente e i gruppi siano stati sincronizzati correttamente in AEM.
   + Developer Console richiede la creazione del record utente nel livello di servizio AEM corrispondente per l’autenticazione a tale livello di servizio.
1. Elimina i cookie del browser e lo stato dell’applicazione (archiviazione locale) e accedi nuovamente a Developer Console, garantendo che il token di accesso utilizzato da Developer Console sia corretto e non sia scaduto.

## Pod

I servizi Author e Publish di AEM sono composti rispettivamente di più istanze per gestire la variabilità del traffico e gli aggiornamenti continui senza downtime. Queste istanze sono denominate Pods. La selezione del contenitore in Developer Console definisce l’ambito dei dati che verranno esposti tramite gli altri controlli.

![Console per sviluppatori - Pod](./assets/developer-console/pod.png)

+ Un pod è un’istanza discreta che fa parte di un servizio AEM (Author or or Publish)
+ I baccelli sono transitori, il che significa AEM come un Cloud Service li crea e li distrugge secondo necessità
+ Solo i pod che fanno parte del AEM associato come ambiente di Cloud Service, sono elencati il commutatore Pod della Console per sviluppatori dell’ambiente.
+ Nella parte inferiore dello switcher Pod, le opzioni di comodità consentono di selezionare i pod in base al tipo di servizio:
   + Tutti gli autori
   + Tutti gli editori
   + Tutte le istanze

## Stato

Lo stato fornisce opzioni per l’output di uno stato di runtime AEM specifico nel testo o nell’output JSON. Developer Console fornisce informazioni simili alla console Web OSGi locale dell’SDK AEM, con la differenza marcata che Developer Console è in sola lettura.

![Console per sviluppatori - Stato](./assets/developer-console/status.png)

### Bundle

I bundle elencano tutti i bundle OSGi in AEM. Questa funzionalità è simile ai [AEM bundle OSGi locali dell&#39;SDK quickstart](http://localhost:4502/system/console/bundles) in `/system/console/bundles`.

I bundle aiutano a eseguire il debug:

+ Elencare tutti i bundle OSGi distribuiti su AEM as a Service
+ Elencare lo stato di ciascun bundle OSGi; anche se sono attivi o meno
+ Fornire dettagli sulle dipendenze non risolte che causano l’attivazione dei bundle OSGi

### Componenti

Componenti elenca tutti i componenti OSGi in AEM. Questa funzionalità è simile ai componenti OSGi locali dell&#39;SDK [AEM ](http://localhost:4502/system/console/components) in `/system/console/components` di .

I componenti sono utili per il debug:

+ Elencare tutti i componenti OSGi distribuiti per AEM come Cloud Service
+ Fornire lo stato di ciascun componente OSGi; anche se sono attivi o insoddisfatti
+ Fornire dettagli in riferimenti di servizio non soddisfatti può causare l&#39;attivazione dei componenti OSGi
+ Elenco delle proprietà OSGi e dei loro valori associati al componente OSGi

### Configurazioni

Le configurazioni elencano tutte le configurazioni del componente OSGi (proprietà e valori OSGi). Questa funzionalità è simile a [AEM OSGi Configuration Manager locale dell&#39;SDK ](http://localhost:4502/system/console/configMgr)in `/system/console/configMgr`.

Le configurazioni sono utili per il debug:

+ Elencare le proprietà OSGi e i loro valori dal componente OSGi
+ Individuazione e identificazione delle proprietà non configurate

### Indici Oak

Gli indici Oak forniscono un’immagine dei nodi definiti sotto `/oak:index`. Tieni presente che questo non mostra gli indici uniti, che si verifica quando viene modificato un indice AEM.

Aiuto per gli indici Oak nel debug tramite:

+ Elencando tutte le definizioni di Oak Index che forniscono approfondimenti su come vengono eseguite le query di ricerca in AEM. Tieni presente che gli indici modificati in AEM non vengono riportati qui. Questa visualizzazione è utile solo per gli indici forniti esclusivamente da AEM o unicamente dal codice personalizzato.

### Servizi OSGi

Componenti elenca tutti i servizi OSGi. Questa funzionalità è simile a [AEM servizi OSGi locali dell&#39;SDK ](http://localhost:4502/system/console/services) in `/system/console/services`.

Guida dei servizi OSGi per il debug:

+ Elencare tutti i servizi OSGi in AEM, insieme al suo bundle OSGi che fornisce, e tutti i bundle OSGi che lo utilizzano

### Processi Sling

In Processi Sling sono elencate tutte le code di Processi Sling. Questa funzionalità è simile ai processi locali dell&#39;SDK [AEM ](http://localhost:4502/system/console/slingevent) in `/system/console/slingevent` dell&#39;SDK.

La guida dei processi Sling per il debug è disponibile nei seguenti modi:

+ Elenco delle code di lavoro Sling e delle relative configurazioni
+ Fornire informazioni sul numero di processi Sling attivi, in coda ed elaborati, utile per il debug dei problemi con Flusso di lavoro, Flusso di lavoro transitorio e altri lavori eseguiti da Processi Sling in AEM.

## Pacchetti Java

Java Packages consente di verificare se un pacchetto Java e una versione sono disponibili per l&#39;uso in AEM come Cloud Service. Questa funzionalità è la stessa di [AEM dâ SDK locale a quickstart&#39;s Dependency Finder](http://localhost:4502/system/console/depfinder) all&#39;indirizzo `/system/console/depfinder`.

![Console per sviluppatori - Pacchetti Java](./assets/developer-console/java-packages.png)

I pacchetti Java vengono utilizzati per risolvere i problemi di avvio I bundle non vengono avviati a causa di importazioni non risolte o classi non risolte negli script (HTL, JSP, ecc.). Se i pacchetti Java segnalano che nessun bundle esporta un pacchetto Java (o la versione non corrisponde a quella importata da un bundle OSGi):

+ Assicurati che la versione della dipendenza AEM API maven del progetto corrisponda alla versione di rilascio AEM dell’ambiente (e, se possibile, aggiorna tutto all’ultima versione).
+ Se nel progetto Maven vengono utilizzate dipendenze Maven aggiuntive
   + Determina se è invece possibile utilizzare un’API alternativa fornita dalla dipendenza API SDK AEM.
   + Se la dipendenza extra è necessaria, assicurati che sia fornita come bundle OSGi (anziché come Jar normale) e sia incorporata nel pacchetto di codice del progetto, (`ui.apps`), in modo simile al modo in cui il bundle OSGi principale è incorporato nel pacchetto `ui.apps`.

## Servlet

Servlets viene utilizzato per fornire informazioni su come AEM risolve un URL a un servlet o a uno script Java (HTL, JSP) che gestisce in ultima analisi la richiesta. Questa funzionalità è la stessa di [AEM Sling Servlet Resolver dell&#39;SDK locale di quickstart](http://localhost:4502/system/console/servletresolver) in `/system/console/servletresolver`.

![Console per sviluppatori - Servlet](./assets/developer-console/servlets.png)

I servlet consentono di determinare nel debug:

+ Modalità di decomposizione di un URL nelle relative parti indirizzabili (risorsa, selettore, estensione).
+ A quale servlet o script viene risolto un URL, per identificare gli URL formati male o i servlet/script non registrati.

## Query

Le query forniscono informazioni dettagliate su cosa e come vengono eseguite le query di ricerca su AEM. Questa funzionalità è la stessa della console [AEM quickstart locale dell’SDK > Prestazioni query ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) .

Le query funzionano solo quando è selezionato un pod specifico, in quanto apre la console Web Query Performance del pod, che richiede allo sviluppatore di avere accesso per accedere al servizio AEM.

![Console per sviluppatori - Query - Spiegare query](./assets/developer-console/queries__explain-query.png)

Le query consentono di eseguire il debug in base a:

+ Spiegazione di come le query vengono interpretate, analizzate ed eseguite da Oak. Questo è molto importante quando si tiene traccia del motivo per cui una query è lenta e si capisce come può essere accelerata.
+ Elencando le query più popolari in esecuzione in AEM, con la possibilità di Spiegarle.
+ Elencando le query più lente in esecuzione in AEM, con la possibilità di Spiegarle.
