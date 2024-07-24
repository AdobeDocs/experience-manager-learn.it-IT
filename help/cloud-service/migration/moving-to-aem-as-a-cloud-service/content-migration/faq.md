---
title: Domande frequenti sulla migrazione dei contenuti AEM as a Cloud Service
description: Risposte alle domande frequenti sulla migrazione dei contenuti ad AEM as a Cloud Service.
version: Cloud Service
doc-type: article
topic: Migration
feature: Migration
role: Architect, Developer
level: Beginner
jira: KT-11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
duration: 399
source-git-commit: e29eaefb20d466126d0d31ad8eb598b63a0cebcd
workflow-type: tm+mt
source-wordcount: '1884'
ht-degree: 0%

---

# Domande frequenti sulla migrazione dei contenuti AEM as a Cloud Service

Risposte alle domande frequenti sulla migrazione dei contenuti ad AEM as a Cloud Service.

## Terminologia

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html?lang=it)
+ **BPA**: [Analisi delle best practice](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html?lang=it)
+ **CTT**: [Strumento Trasferimento contenuti](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=it)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Sistema Identity Management](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**: [Dynamic Medie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

Utilizza il modello seguente per fornire ulteriori dettagli durante la creazione dei ticket di supporto Adobe relativi a CTT.

![Modello ticket supporto Adobe migrazione contenuti](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## Domande generali sulla migrazione dei contenuti

### D: Quali sono i diversi metodi per migrare i contenuti all’AEM come Cloud Service?

Sono disponibili tre metodi diversi

+ Utilizzo dello strumento Content Transfer (AEM 6.3+ → AEMaaCS)
+ Tramite Gestione pacchetti (AEM → AEMaaCS)
+ Servizio di importazione in blocco predefinito per Assets (S3/Azure → AEMaaCS)

### D: Esiste un limite alla quantità di contenuto che può essere trasferito utilizzando CTT?

No. Il CTT come strumento potrebbe estrarre dall’origine dell’AEM e inserirlo in AEMaaCS. Tuttavia, esistono limiti specifici alla piattaforma AEMaaCS che devono essere considerati prima della migrazione.

Per ulteriori informazioni, consulta [prerequisiti per la migrazione cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html?lang=it).

### D: Ho il report BPA più recente dal mio sistema di origine, cosa devo fare con esso?

Esporta il rapporto come CSV e caricalo in Cloud Acceleration Manager, [associato alla tua organizzazione IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html). Seguire quindi il processo di revisione come [descritto nella fase di preparazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

Rivedi la valutazione della complessità del codice e dei contenuti fornita dallo strumento e prendi nota delle azioni associate che portano al backlog di refactoring del codice o alla valutazione della migrazione cloud.

### D: Si consiglia di estrarre dall’autore dell’origine e di acquisire in AEMaaCS author e publish?

Si consiglia sempre di eseguire l’estrazione e l’acquisizione 1:1 tra i livelli di authoring e pubblicazione. Detto questo, è accettabile estrarre l’autore della produzione sorgente e inserirlo in Dev, Stage e Production CS.

### D: Esiste un modo per stimare il tempo necessario per migrare i contenuti dall’AEM di origine ad AEMaaCS utilizzando il CTT?

Poiché il processo di migrazione dipende dalla larghezza di banda Internet, dall’heap allocato per il processo CTT, dalla memoria disponibile e dall’I/O su disco, che sono soggetti a ciascun sistema di origine, si consiglia di eseguire in anticipo le migrazioni Proof Of (Bozza di migrazione) ed estrapolare tali punti di dati per ottenere delle stime.

### D: In che modo le prestazioni dell’AEM di origine vengono influenzate se avvio il processo di estrazione CTT?

Lo strumento CTT viene eseguito nel proprio processo Java™ che occupa fino a 4 gb di heap, configurabile tramite la configurazione OSGi. Questo numero può cambiare, ma puoi usare il processo Java™ e scoprirlo.

Se AZCopy è installato e/o l&#39;opzione/funzione di convalida Pre Copy è abilitata, il processo AZCopy utilizza cicli di CPU.

Oltre a jvm , lo strumento utilizza anche l’I/O su disco per memorizzare i dati su uno spazio temporaneo transitorio che verrà pulito dopo il ciclo di estrazione. Oltre a RAM, CPU e I/O disco, lo strumento CTT utilizza anche la larghezza di banda di rete del sistema di origine per caricare i dati nell’archivio BLOB di Azure.

La quantità di risorse impiegate dal processo di estrazione CTT dipende dal numero di nodi, dal numero di BLOB e dalla loro dimensione aggregata. Poiché è difficile fornire una formula, si consiglia di eseguire una piccola verifica della migrazione per determinare i requisiti di upsize del server di origine.

Se per la migrazione vengono utilizzati ambienti clone, questo non influirà sull’utilizzo delle risorse del server di produzione live, ma presenta alcuni aspetti negativi per quanto riguarda la sincronizzazione dei contenuti tra produzione live e clone

### D: Cosa significano i termini &quot;cancella&quot; e &quot;sovrascrivi&quot; nel contesto del CTT?

Nel contesto della [fase di estrazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=en#extraction-setup-phase), le opzioni sono sovrascrivere i dati nel contenitore di staging dai cicli di estrazione precedenti o aggiungervi il differenziale (aggiunto/aggiornato/eliminato). Il contenitore di staging non è un elemento valido, ma è il contenitore di archiviazione BLOB associato al set di migrazione. Ogni set di migrazione ottiene il proprio contenitore di staging.

Nel contesto della [fase di acquisizione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/ingesting-content.html), le opzioni sono + per sostituire l&#39;intero archivio dei contenuti di AEMaaCS o sincronizzare il contenuto differenziale (aggiunto/aggiornato/eliminato) dal contenitore di migrazione dell&#39;area di gestione temporanea.

### D: nel sistema di origine sono presenti più siti web, risorse associate, utenti e gruppi. È possibile migrarli in più fasi in AEMaaCS?

Sì, è possibile ma richiede un&#39;attenta pianificazione per quanto riguarda:

+ Creazione dei set di migrazione presupponendo che i siti, le risorse siano nelle rispettive gerarchie
   + Verifica se è accettabile migrare tutte le risorse come parte di un set di migrazione e quindi portare siti che le utilizzano in fasi
+ Nello stato corrente, il processo di acquisizione dell’autore rende l’istanza di authoring non disponibile per l’authoring dei contenuti anche se il livello di pubblicazione può ancora gestire i contenuti
   + Questo significa che finché l’acquisizione non viene completata nell’ambiente di authoring, le attività di authoring dei contenuti vengono congelate
+ La migrazione degli utenti non viene più eseguita, anche se i gruppi lo sono.

Prima di pianificare le migrazioni, controlla il processo di estrazione e acquisizione integrativa come documentato.

### D: I miei siti web saranno disponibili per gli utenti finali anche se l’acquisizione avviene nelle istanze di authoring o pubblicazione di AEMaaCS?

Sì. Il traffico degli utenti finali non viene interrotto dall’attività di migrazione dei contenuti. Tuttavia, l’acquisizione dell’autore blocca l’authoring dei contenuti fino al suo completamento.

### D: il rapporto BPA mostra gli elementi relativi alle rappresentazioni originali mancanti. Devo ripulirli nell’origine prima dell’estrazione?

Sì. La rappresentazione originale mancante indica che il binario della risorsa non è caricato correttamente. Considerandoli come dati errati, rivedi, esegui il backup utilizzando Gestione pacchetti (come richiesto) e rimuovili dall’AEM sorgente prima di eseguire l’estrazione. I dati errati avranno risultati negativi sui passaggi di elaborazione delle risorse.

### D: Il report BPA contiene elementi correlati a un nodo `jcr:content` mancante per le cartelle. Cosa dovrei fare con loro?

Quando `jcr:content` non è presente a livello di cartella, qualsiasi azione per propagare le impostazioni, ad esempio i profili di elaborazione e così via. da genitori si interromperà a questo livello. Rivedi il motivo per cui mancano `jcr:content`. Anche se è possibile eseguire la migrazione di queste cartelle, si noti che le cartelle danneggiano l&#39;esperienza utente e causano cicli di risoluzione dei problemi non necessari in un secondo momento.

### D: ho creato un set di migrazione. è possibile controllarne la dimensione?

Sì, esiste una funzionalità [Verifica dimensione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) che fa parte del CTT.

### D: eseguo la migrazione (estrazione, acquisizione). È possibile verificare che tutto il contenuto estratto sia stato acquisito in Target?

Sì, esiste una funzionalità [validation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html?lang=it) che fa parte di CTT.

### D: il mio cliente ha l’esigenza di spostare i contenuti tra ambienti AEMaaCS, ad esempio da AEMaaCS Dev a AEMaaCS Stage o a AEMaaCS Prod. Posso utilizzare lo strumento di trasferimento dei contenuti per questi casi d’uso?

Sfortunatamente, no. Il caso d’uso di CTT consiste nella migrazione dei contenuti dall’origine AEM 6.3+ in sede/AMS agli ambienti cloud AEMaaCS. [Consulta la documentazione CTT](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=it).

### D: Che tipo di problemi sono previsti durante l&#39;estrazione?

La fase di estrazione è un processo coinvolto che richiede il funzionamento previsto di più aspetti. Conoscere i diversi tipi di problemi che possono verificarsi e come attenuarli aumenta il successo complessivo della migrazione dei contenuti.

La documentazione pubblica viene continuamente migliorata in base ai risultati, ma ecco alcune categorie di problemi di alto livello e possibili ragioni sottostanti.

![Problemi di estrazione della migrazione dei contenuti di AEM as a Cloud Service](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### D: Che tipo di problemi si prevedono durante l’acquisizione?

La fase di acquisizione si verifica completamente nella piattaforma cloud e richiede aiuto da parte delle risorse che hanno accesso all’infrastruttura AEMaaCS. Crea un ticket di supporto per ulteriore assistenza.

Di seguito sono elencate le possibili categorie di problemi (non considerarla come un elenco esclusivo)

![Problemi di acquisizione della migrazione dei contenuti di AEM as a Cloud Service](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### D: il server di origine deve disporre di una connessione Internet in uscita per il funzionamento di CTT?

La risposta breve è &quot;**Sì**&quot;.

Il processo CTT richiede connettività alle risorse seguenti:

+ Ambiente AEM as a Cloud Service di destinazione: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Servizio di archiviazione BLOB di Azure: `casstorageprod.blob.core.windows.net`

Per ulteriori informazioni sulla [connettività di origine](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity), consultare la documentazione.

## Domande relative a Dynamic Medie sull’elaborazione delle risorse

### D: Le risorse verranno rielaborate automaticamente dopo l’acquisizione in AEMaaCS?

No. Per elaborare le risorse, è necessario avviare la richiesta di rielaborazione.

### D: Le risorse verranno reindicizzate automaticamente dopo l’acquisizione in AEMaaCS?

Sì. Le risorse vengono reindicizzate in base alle definizioni dell’indice disponibili in AEMaaCS.

### D: L’AEM di origine è integrato con Dynamic Medie. Ci sono aspetti specifici da considerare prima di migrare i contenuti?

Sì, considera quanto segue quando l’AEM di origine ha l’integrazione con Dynamic Medie.

+ AEMaaCS supporta solo la modalità Scene7 di Dynamic Medie. Se il sistema di origine è in modalità ibrida, è necessaria la migrazione di DM alle modalità Scene7.
+ Se l’approccio prevede la migrazione dalle istanze del clone sorgente, è sicuro disabilitare l’integrazione DM sul clone che verrebbe utilizzata per CTT. Questo passaggio ha lo scopo puramente di evitare qualsiasi scrittura in DM o di evitare il carico sul traffico DM.
+ CTT migra i nodi, i metadati di un set di migrazione dall’AEM di origine ad AEMaaCS. Non eseguirà alcuna operazione direttamente su DM.

### D: Quali sono i diversi approcci di migrazione quando l’integrazione DM è presente sull’AEM di Source?

Leggi le domande e le risposte precedenti prima di

(Si tratta di due opzioni possibili, ma non sono limitate a queste due sole opzioni). Dipende dal modo in cui il cliente desidera avvicinarsi all’UAT, dai test delle prestazioni, dall’ambiente disponibile e dal fatto che un clone venga utilizzato o meno per la migrazione. Considerate questi due punti come punto di partenza per la discussione

**Opzione 1**

Se il numero di risorse o nodi nell’ambiente di origine si trova all’estremità inferiore (~100K), supponendo che sia possibile migrarli per un periodo di 24 + 72 ore, comprese l’estrazione e l’acquisizione, l’approccio migliore è

+ Eseguire direttamente la migrazione dalla produzione
+ Esegui un&#39;estrazione e un&#39;acquisizione iniziali in AEMaaCS con `wipe=true`
   + Questo passaggio migra tutti i nodi e i binari
+ Continua a lavorare on-premise / Autore di prodotti AMS
+ Da ora in poi, esegui tutti gli altri cicli di verifica della migrazione con `wipe=true`
   + Nota: questa operazione migra l&#39;archivio nodi completo, ma solo i BLOB modificati anziché quelli interi. Il set precedente di BLOB è presente nell’archivio BLOB di Azure dell’istanza AEMaaCS di destinazione.
   + Utilizza questa prova delle migrazioni per misurare la durata della migrazione, il test e la convalida di tutte le altre funzionalità
+ Infine, prima della settimana di pubblicazione, effettua una cancellazione=migrazione effettiva
   + Connettere Dynamic Medie su AEMaaCS
   + Disconnetti configurazione DM da origine locale AEM

Con questa opzione è possibile eseguire la migrazione da uno a uno, ovvero Sviluppo on-premise → Sviluppo AEMaaCS e così via. e spostare le configurazioni DM dai rispettivi ambienti

(nel caso in cui si pianifichi l’esecuzione della migrazione da Clone)

**Opzione 2**

+ Crea un clone dell’istanza di authoring di produzione, rimuovi la configurazione DM da Clone
+ Migrazione on-premise del clone → AEMaaCS Dev / Stage
   + Connettere brevemente la società DM di produzione a AEMaaCS Dev/Stage a scopo di convalida
   + Quando la connessione DM è attiva, evita l’acquisizione di risorse in AEMaaCS
   + Questo consente loro di convalidare le convalide CTT e DM specifiche
+ Una volta completato il test su AEMaaCS
   + Eseguire una migrazione Wipe da on-premise Stage ad AEMaaCS Stage

Esegui una migrazione Wipe (Cancella) da On-Premise Dev a AEMaaCS Dev.

L&#39;approccio di cui sopra può essere utilizzato solo per misurare la durata della migrazione, ma richiede di pulirla in un secondo momento.

## Risorse aggiuntive

+ [Suggerimenti per la migrazione all&#39;Experience Manager nel cloud ( Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [Video serie esperti CTT](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [Video della serie Expert su altri argomenti di AEMaaCS](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/aem-experts-series.html)
