---
title: Console per sviluppatori
description: AEM come Cloud Service fornisce una console per sviluppatori per ogni ambiente che espone vari dettagli del servizio AEM in esecuzione utili per il debug.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
translation-type: tm+mt
source-git-commit: 1af3661e5c18206d58d339d51d5189834e843023
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---


# Debug di AEM come Cloud Service con Developer Console

AEM come Cloud Service fornisce una console per sviluppatori per ogni ambiente che espone vari dettagli del servizio AEM in esecuzione utili per il debug.

Ogni AEM come ambiente di Cloud Service dispone di una propria console per sviluppatori.

## Accesso alla console per sviluppatori

Per accedere e utilizzare la Developer Console, è necessario assegnare le seguenti autorizzazioni all&#39; Adobe ID dello sviluppatore tramite [ Adobe   Admin Console](https://adminconsole.adobe.com).

1. Assicurati che l’organizzazione  Adobe che ha influenzato Cloud Manager e AEM come prodotti Cloud Service sia attiva nello switcher di organizzazione  Adobe.
1. Lo sviluppatore deve essere membro del profilo di prodotto __Sviluppatore - Cloud Service__ di Cloud Manager.
   + Se l&#39;iscrizione non esiste, lo sviluppatore non sarà in grado di accedere a Developer Console.
1. Lo sviluppatore deve essere membro del profilo di prodotto __AEM Administrators__ di AEM Author and Publish Service.
   + Se l&#39;iscrizione non esiste, i gattini [status](#status) scadranno con un errore 401 non autorizzato.

### Risoluzione dei problemi di accesso alla console per sviluppatori

#### 401 Errore non autorizzato durante il dumping

![Console per sviluppatori - 401 non autorizzato](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se viene segnalato un errore 401 Non autorizzato, significa che l&#39;utente non esiste ancora con le autorizzazioni necessarie in AEM come Cloud Service o che l&#39;utilizzo dei token di accesso non è valido o è scaduto.

Per risolvere il problema 401 Non autorizzato:

1. Assicurati che l&#39;utente sia un membro del profilo di prodotto IMS  Adobe appropriato (AEM amministratori o AEM utenti) per il AEM associato della Developer Console come istanza di Cloud Service.
   + Ricorda che Developer Console accede a 2 istanze di prodotto IMS  Adobe; aem come Cloud Service di istanze di prodotti Autore e Pubblica, accertati che vengano utilizzati i profili di prodotto corretti a seconda del livello di servizio a cui è necessario accedere tramite Developer Console.
1. Accedete al AEM come Cloud Service (Autore o Pubblica) e accertatevi che utenti e gruppi siano sincronizzati correttamente in AEM.
   + La console per sviluppatori richiede la creazione del record utente nel livello di servizio AEM corrispondente per l&#39;autenticazione a tale livello di servizio.
1. Cancella i cookie dei browser e lo stato dell’applicazione (archiviazione locale) ed effettua nuovamente l’accesso a Developer Console, verificando che il token di accesso utilizzato dalla Developer Console sia corretto e non sia scaduto.

## Pod

AEM come Cloud Service, i servizi Autore e Pubblica sono composti rispettivamente di più istanze per gestire la variabilità del traffico e gli aggiornamenti di scorrimento senza tempi di inattività. Tali istanze sono denominate Contenitori. La selezione del contenitore in Developer Console definisce l’ambito dei dati che verranno esposti tramite gli altri controlli.

![Console per sviluppatori - Contenitore](./assets/developer-console/pod.png)

+ Un contenitore è un&#39;istanza distinta che fa parte di un servizio AEM (Autore o Pubblica)
+ I contenitori sono transitori, cioè AEM come un Cloud Service li crea e li distrugge in base alle necessità
+ Solo i contenitori che fanno parte del AEM associato come ambiente di Cloud Service, sono elencati nello switcher di contenitori della console per sviluppatori dell&#39;ambiente.
+ Nella parte inferiore dello switcher del contenitore, le opzioni di convenienza consentono di selezionare i contenitori in base al tipo di servizio:
   + Tutti gli autori
   + Tutti gli editori
   + Tutte le istanze

## Stato

Lo stato fornisce opzioni per l&#39;output AEM stato di runtime specifico nel testo o nell&#39;output JSON. Developer Console fornisce informazioni simili alla console Web OSGi dell’SDK AEM, con la differenza marcata che Developer Console è in sola lettura.

![Console per sviluppatori - Stato](./assets/developer-console/status.png)

### Bundle

I pacchetti elencano tutti i bundle OSGi in AEM. Questa funzionalità è simile a [AEM Bundle OSGi dell&#39;SDK locale in &lt;a0/>`/system/console/bundles` all&#39;indirizzo &lt;a2/>.](http://localhost:4502/system/console/bundles)

I bundle consentono di eseguire il debug tramite:

+ Elenco di tutti i bundle OSGi distribuiti per AEM come servizio
+ elencare lo stato di ciascun bundle OSGi; anche se sono attivi o meno
+ Fornire dettagli sulle dipendenze non risolte che causano l&#39;attivazione dei bundle OSGi

### Componenti

I componenti elencano tutti i componenti OSGi in AEM. Questa funzionalità è simile a [AEM componenti OSGi locali dell&#39;SDK &lt;a0/>in `/system/console/components` all&#39;indirizzo &lt;a2/>.](http://localhost:4502/system/console/components)

I componenti sono utili per eseguire il debug:

+ Elenco di tutti i componenti OSGi distribuiti come Cloud Service
+ Fornire lo stato di ciascun componente OSGi; anche se sono attivi o insoddisfatti
+ Fornire dettagli in riferimenti di servizio non soddisfatti potrebbe causare l&#39;attivazione dei componenti OSGi
+ Elenco delle proprietà OSGi e dei relativi valori associati al componente OSGi

### Configurazioni

Configurazioni elenca tutte le configurazioni del componente OSGi (proprietà e valori OSGi). Questa funzionalità è simile a [AEM QuickStart locale OSGi Configuration Manager dell&#39;SDK ](http://localhost:4502/system/console/configMgr) all&#39;indirizzo `/system/console/configMgr`.

Le configurazioni sono utili per il debug:

+ Elenco delle proprietà OSGi e dei relativi valori per componente OSGi
+ Individuazione e identificazione delle proprietà non configurate

### Indici Oak

Gli indici Oak forniscono un dump dei nodi definiti sotto `/oak:index`. Tenere presente che questo non mostra indici uniti, che si verifica quando viene modificato un indice AEM.

Gli indici Oak consentono di eseguire il debug:

+ Elenco di tutte le definizioni dell&#39;indice Oak che forniscono informazioni dettagliate su come le query di ricerca vengono eseguite in AEM. Tenere presente che le modifiche apportate agli indici AEM non vengono riportate qui. Questa visualizzazione è utile solo per gli indici forniti esclusivamente da AEM o forniti esclusivamente dal codice personalizzato.

### OSGi Services

I componenti elencano tutti i servizi OSGi. Questa funzionalità è simile a [AEM servizi OSGi locali dell&#39;SDK &lt;a0/>`/system/console/services` all&#39;indirizzo &lt;a2/>.](http://localhost:4502/system/console/services)

I servizi OSGi consentono di eseguire il debug tramite:

+ Elencare tutti i servizi OSGi in AEM, insieme al relativo bundle OSGi fornito, e tutti i bundle OSGi che lo utilizzano

### Processi Sling

In Processi Sling sono elencate tutte le code dei processi Sling. Questa funzionalità è simile a quella di [AEM Processi di avvio rapido dell&#39;SDK ](http://localhost:4502/system/console/slingevent) all&#39;indirizzo `/system/console/slingevent`.

La guida di Sling Jobs consente di eseguire il debug tramite:

+ Elenco delle code dei processi Sling e relative configurazioni
+ Fornisce informazioni sul numero di processi Sling attivi, in coda ed elaborati, utili per il debug dei problemi relativi a Flusso di lavoro, Flusso di lavoro transitorio e altri lavori eseguiti da Sling Jobs in AEM.

## Pacchetti Java

Java Packages consente di verificare se un pacchetto Java e una versione sono disponibili per l&#39;uso in AEM come Cloud Service. Questa funzionalità è identica a quella di [AEM QuickStart nel Finder dipendenza dell&#39;SDK ](http://localhost:4502/system/console/depfinder) in `/system/console/depfinder`.

![Console per sviluppatori - Pacchetti Java](./assets/developer-console/java-packages.png)

I pacchetti Java vengono utilizzati per evitare l&#39;avvio dei pacchetti di ripresa a causa di importazioni non risolte o di classi non risolte negli script (HTL, JSP, ecc.). Se i rapporti sui pacchetti Java non riportano alcun bundle, esportate un pacchetto Java (o la versione non corrisponde a quella importata da un bundle OSGi):

+ Assicurati che la versione della dipendenza AEM API maven del progetto corrisponda alla versione AEM Release dell&#39;ambiente (e, se possibile, aggiorna tutto alla versione più recente).
+ Se ulteriori dipendenze Paradiso vengono utilizzate nel progetto Paradiso
   + Determinate se è invece possibile utilizzare un&#39;API alternativa fornita dalla dipendenza dell&#39;API SDK AEM.
   + Se la dipendenza supplementare è necessaria, accertatevi che sia fornita come bundle OSGi (anziché come Jar normale) e sia incorporata nel pacchetto di codice del progetto (`ui.apps`), in modo simile al modo in cui il pacchetto OSGi di base è incorporato nel pacchetto `ui.apps`.

## Servlet

Servlets viene utilizzato per fornire informazioni su come AEM risolve un URL a un servlet o script Java (HTL, JSP) che in ultima istanza gestisce la richiesta. Questa funzionalità è la stessa di Sling Servlet Resolver [AEM SDK locale di &lt;a0/>`/system/console/servletresolver` all&#39;indirizzo &lt;a2/>.](http://localhost:4502/system/console/servletresolver)

![Console per sviluppatori - Servlet](./assets/developer-console/servlets.png)

Servlet aiuta a determinare il debug:

+ Modalità di scomposizione di un URL nelle relative parti indirizzabili (risorsa, selettore, estensione).
+ A quale servlet o script corrisponde un URL, per identificare gli URL con formato errato o i servlet/script non registrati.

## Query

Le query forniscono informazioni approfondite su cosa e come vengono eseguite le query di ricerca in AEM. Questa funzionalità è la stessa della console [AEM QuickStart locale dell&#39;SDK > Prestazioni query ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html).

Le query funzionano solo quando è selezionato un contenitore specifico, poiché apre la console Web Prestazioni query del contenitore, per cui lo sviluppatore deve avere accesso al servizio AEM.

![Console per sviluppatori - Query - Spiegazione](./assets/developer-console/queries__explain-query.png)

Le query sono utili per eseguire il debug:

+ Spiegazione di come le query vengono interpretate, analizzate ed eseguite da Oak. Questo è molto importante quando si monitora il motivo per cui una query è lenta e si capisce come possa essere velocizzata.
+ Elencando le query più comuni in esecuzione in AEM, con la possibilità di spiegarle.
+ Elencando le query più lente in esecuzione in AEM, con la possibilità di spiegarle.
