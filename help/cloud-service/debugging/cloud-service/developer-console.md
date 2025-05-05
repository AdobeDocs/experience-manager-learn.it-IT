---
title: Console per sviluppatori
description: AEM as a Cloud Service fornisce un Developer Console per ogni ambiente che espone vari dettagli del servizio AEM in esecuzione utili per il debug.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 295
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 0%

---

# Debug di AEM as a Cloud Service con Developer Console

AEM as a Cloud Service fornisce un Developer Console per ogni ambiente che espone vari dettagli del servizio AEM in esecuzione utili per il debug.

Ogni ambiente AEM as a Cloud Service dispone di un proprio Developer Console.

## Passa a Developer Console

Developer Console è accessibile per ogni ambiente AEM as a Cloud Service tramite Cloud Manager.

![Passa a Developer Console](./assets/developer-console/navigate.png)

1. Passa a __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. Apri il __programma__ che contiene l&#39;ambiente AEM as a Cloud Service per aprire Developer Console.
3. Individuare l&#39;__ambiente__ e selezionare `...`.
4. Seleziona __Developer Console__ dall&#39;elenco a discesa.


## Accesso a Developer Console

Per accedere a Developer Console e utilizzarlo, è necessario concedere le seguenti autorizzazioni all&#39;Adobe ID dello sviluppatore tramite [Admin Console](https://adminconsole.adobe.com) di Adobe.

1. Assicurati che nel selettore dell’organizzazione Adobe sia visibile l’organizzazione Adobe relativa agli ambienti che desideri ispezionare in Developer Console.
1. Per accedere a Developer Console, lo sviluppatore deve essere membro di uno dei seguenti ruoli:
   + [Prodotto Cloud Manager __Sviluppatore - Profilo prodotto Cloud Service__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html?lang=it#assign-developer): in questo caso, lo sviluppatore visualizzerà l&#39;elenco completo degli ambienti disponibili nell&#39;URL Developer Console selezionato; se è stato selezionato un ambiente di sviluppo o un RDE in Cloud Manager, è possibile che vengano visualizzati altri ambienti di sviluppo o RDE nello stesso programma.
   + [__Amministratori AEM__ profilo di prodotto in __AEM Author__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html?lang=it#aem-product-profiles): in questo caso, l&#39;elenco degli ambienti descritti nel punto precedente sarà limitato ai profili di prodotto correlati a cui è assegnato questo ruolo.
1. Lo sviluppatore deve essere membro del profilo di prodotto [__Utenti AEM__ o __Amministratori AEM__ in AEM Author e/o Publish](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html?lang=it#aem-product-profiles).
   + Se l&#39;appartenenza non esiste, le immagini di stato [stato](#status) si interromperanno con un errore 401 Non autorizzato.

### Risoluzione dei problemi di accesso a Developer Console

#### Quando accedo non vedo elencato l’ambiente che sto cercando

Verifica quanto segue:

+ Hai selezionato l’URL Developer Console corretto facendo clic sui tre punti per l’ambiente selezionato tramite Cloud Manager e seleziona Developer Console.
+ Hai [il profilo di prodotto __Sviluppatore - Cloud Service__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html?lang=it#assign-developer) di Cloud Manager per visualizzare l&#39;elenco completo degli ambienti oppure fai parte del profilo di prodotto [__Amministratori AEM__ in __AEM Author__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html?lang=it#aem-product-profiles) per l&#39;ambiente che non trovi.

#### 401 Errore non autorizzato durante il dumping dello stato

![Developer Console - 401 Non autorizzato](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se viene segnalato un errore 401 Unauthorized, significa che l’utente non dispone ancora delle autorizzazioni necessarie in AEM as a Cloud Service oppure che l’utilizzo dei token di accesso non è valido o è scaduto.

Per risolvere il problema 401 Unauthorized:

1. Assicurati che l’utente sia membro del profilo di prodotto Adobe IMS appropriato (amministratori di AEM o utenti di AEM) per l’istanza di prodotto AEM as a Cloud Service associata a Developer Console.
   + Ricorda che Developer Console accede a 2 istanze di prodotto Adobe IMS; le istanze di prodotto AEM as a Cloud Service Author e Publish, quindi assicurati che vengano utilizzati i profili di prodotto corretti a seconda del livello di servizio a cui è necessario accedere tramite Developer Console.
1. Accedi ad AEM as a Cloud Service (Author o Publish) e assicurati che gli utenti e i gruppi siano sincronizzati correttamente in AEM.
   + Developer Console richiede che il record utente venga creato nel livello di servizio AEM corrispondente per l’autenticazione in tale livello di servizio.
1. Cancella i cookie del browser e lo stato dell’applicazione (archiviazione locale) ed effettua di nuovo l’accesso a Developer Console, verificando che il token di accesso utilizzato da Developer Console sia corretto e non scaduto.

## Pod

I servizi Author e Publish di AEM as a Cloud Service sono composti rispettivamente da più istanze per gestire la variabilità del traffico e gli aggiornamenti continui senza tempi di inattività. Queste istanze sono denominate Pod. La selezione del pod in Developer Console definisce l’ambito dei dati che verranno esposti tramite gli altri controlli.

![Developer Console - Pod](./assets/developer-console/pod.png)

+ Un pod è un’istanza discreta che fa parte di un servizio AEM (authoring o pubblicazione)
+ I baccelli sono transitori, il che significa che AEM as a Cloud Service li crea e li distrugge in base alle esigenze
+ Solo i pod che fanno parte dell’ambiente AEM as a Cloud Service associato, sono elencati nello switcher Pod di tale ambiente Developer Console.
+ Nella parte inferiore dello switcher Pod, le opzioni di praticità consentono di selezionare i Pod in base al tipo di servizio:
   + Tutti gli autori
   + Tutti gli editori
   + Tutte le istanze

## Stato

Stato fornisce opzioni per l’output di uno stato di runtime specifico di AEM nell’output di testo o JSON. Developer Console fornisce informazioni simili a quelle della console web OSGi quickstart locale di AEM SDK, con la netta differenza che Developer Console è di sola lettura.

![Developer Console - Stato](./assets/developer-console/status.png)

### Bundle

Nei bundle sono elencati tutti i bundle OSGi in AEM. Questa funzionalità è simile ai [bundle OSGi per avvio rapido locale di AEM SDK](http://localhost:4502/system/console/bundles) in `/system/console/bundles`.

I bundle consentono di eseguire il debug in base a:

+ Elenco di tutti i bundle OSGi distribuiti in AEM as a Service
+ Elencando lo stato di ciascun bundle OSGi, anche se è attivo o meno
+ Fornire dettagli sulle dipendenze non risolte che causano l’attivazione dei bundle OSGi

### Componenti

Componenti elenca tutti i componenti OSGi in AEM. Questa funzionalità è simile a [Componenti OSGi per avvio rapido locale di AEM SDK](http://localhost:4502/system/console/components) in `/system/console/components`.

I componenti facilitano il debug mediante:

+ Elenco di tutti i componenti OSGi distribuiti in AEM as a Cloud Service
+ Fornire lo stato di ciascun componente OSGi, anche se è attivo o non soddisfatto
+ Se si forniscono dettagli sui riferimenti di servizio non soddisfatti, i componenti OSGi potrebbero diventare attivi
+ Elenco delle proprietà OSGi e dei relativi valori associati al componente OSGi.
   + Verranno visualizzati i valori effettivi inseriti tramite [variabili di configurazione dell&#39;ambiente OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=it#environment-specific-configuration-values).

### Configurazioni

Configurazioni elenca tutte le configurazioni del componente OSGi (proprietà e valori OSGi). Questa funzionalità è simile a [Gestione configurazione OSGi quickstart locale di AEM SDK](http://localhost:4502/system/console/configMgr) alle `/system/console/configMgr`.

Le configurazioni aiutano a eseguire il debug:

+ Elenco delle proprietà OSGi e dei relativi valori per componente OSGi
   + I valori effettivi inseriti tramite [variabili di configurazione dell&#39;ambiente OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=it#environment-specific-configuration-values) NON verranno visualizzati. Vedere [Componenti](#components) sopra, per i valori inseriti.
+ Individuazione e identificazione delle proprietà non configurate correttamente

### Indici Oak

Gli indici Oak forniscono un dump dei nodi definiti sotto `/oak:index`. Tieni presente che questo non mostra gli indici uniti, che si verifica quando viene modificato un indice AEM.

Gli indici di Oak aiutano nel debug:

+ Elenco di tutte le definizioni degli indici di Oak che forniscono informazioni approfondite sull’esecuzione delle query di ricerca in AEM. Tieni presente che le modifiche apportate agli indici AEM non vengono riportate qui. Questa visualizzazione è utile solo per gli indici forniti esclusivamente da AEM o solo dal codice personalizzato.

### Servizi OSGi

Componenti elenca tutti i servizi OSGi. Questa funzionalità è simile a [Servizi OSGi di avvio rapido locale di AEM SDK](http://localhost:4502/system/console/services) in `/system/console/services`.

I servizi OSGi aiutano nel debug:

+ Elencando tutti i servizi OSGi in AEM, insieme al bundle OSGi che lo fornisce e a tutti i bundle OSGi che lo utilizzano

### Processi Sling

Processi Sling elenca tutte le code dei processi Sling. Questa funzionalità è simile a [Processi rapidi locali di AEM SDK](http://localhost:4502/system/console/slingevent) in `/system/console/slingevent`.

I processi Sling facilitano il debug:

+ Elenco delle code di processi Sling e relative configurazioni
+ Fornire informazioni sul numero di processi Sling attivi, in coda ed elaborati, utile per il debug dei problemi relativi ai flussi di lavoro, ai flussi di lavoro transitori e ad altri lavori eseguiti dai processi Sling in AEM.

## Pacchetti Java

I pacchetti Java consentono di verificare se un pacchetto Java, e una versione, sono disponibili per l’utilizzo in AEM as a Cloud Service. Questa funzionalità è uguale a [Individuazione dipendenze di avvio rapido locale di AEM SDK](http://localhost:4502/system/console/depfinder) in `/system/console/depfinder`.

![Developer Console - Pacchetti Java](./assets/developer-console/java-packages.png)

I pacchetti Java vengono utilizzati per risolvere i problemi che impediscono l’avvio dei bundle di ripresa a causa di importazioni non risolte o classi non risolte negli script (HTL, JSP, ecc.). Se in Pacchetti Java non viene segnalato alcun bundle, esporta un pacchetto Java (o la versione non corrisponde a quella importata da un bundle OSGi):

+ Assicurati che la versione della dipendenza Maven dall’API AEM del tuo progetto corrisponda alla versione di AEM Release dell’ambiente (e, se possibile, aggiorna tutto alla versione più recente).
+ Se nel progetto Maven vengono utilizzate dipendenze Maven aggiuntive
   + Determina se è possibile utilizzare un’API alternativa fornita dalla dipendenza API di AEM SDK.
   + Se è necessaria una dipendenza aggiuntiva, assicurati che sia fornita come bundle OSGi (anziché come Jar normale) e che sia incorporata nel pacchetto di codice del progetto, (`ui.apps`), in modo analogo a come il bundle OSGi principale è incorporato nel pacchetto `ui.apps`.

## Servlet

I servlet vengono utilizzati per fornire approfondimenti su come AEM risolve un URL in un servlet o script Java (HTL, JSP) che alla fine gestisce la richiesta. Questa funzionalità è uguale a [Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) per avvio rapido locale di AEM SDK alle `/system/console/servletresolver`.

![Developer Console - Servlet](./assets/developer-console/servlets.png)

I servlet consentono di determinare mediante debug:

+ Come un URL viene decomposto nelle sue parti indirizzabili (risorsa, selettore, estensione).
+ Quale servlet o script viene risolto da un URL, aiutando a identificare gli URL con formato errato o i servlet/script non registrati correttamente.

## Query

Le query forniscono informazioni approfondite su cosa e come vengono eseguite le query di ricerca su AEM. Questa funzionalità è la stessa della console [QuickStart locale di AEM SDK Tools > Query Performance](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html).

Le query funzionano solo quando viene selezionato un pod specifico, poiché apre la console web Prestazioni query del pod, che richiede allo sviluppatore di avere accesso al servizio AEM.

![Developer Console - Query - Spiega query](./assets/developer-console/queries__explain-query.png)

Le query consentono di eseguire il debug mediante:

+ Spiegazione del modo in cui Oak interpreta, analizza ed esegue le query. Questo è molto importante quando si tiene traccia del motivo per cui una query è lenta e si capisce come può essere velocizzata.
+ Elencare le query più popolari in esecuzione in AEM, con la possibilità di spiegarle.
+ Elenco delle query più lente in esecuzione in AEM, con la possibilità di spiegarle.
