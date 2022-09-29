---
title: Domande frequenti sulla migrazione dei contenuti as a Cloud Service AEM
description: Risposte alle domande più frequenti sulla migrazione dei contenuti in AEM as a Cloud Service.
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
source-git-commit: bc222867c937b7d498e7b56bebc0aac18289ad03
workflow-type: tm+mt
source-wordcount: '2289'
ht-degree: 0%

---


# Domande frequenti sulla migrazione dei contenuti as a Cloud Service AEM

Risposte alle domande più frequenti sulla migrazione dei contenuti in AEM as a Cloud Service.

## Terminologia

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**: [Analisi delle best practice](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [Strumento Content Transfer (Trasferimento contenuti)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Sistema Identity Management](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)

Utilizza il seguente modello per fornire ulteriori dettagli durante la creazione di ticket di supporto Adobe relativi al CTT.

![Modello di ticket di supporto per Adobi di migrazione dei contenuti](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

### D: Quali sono i diversi metodi per migrare i contenuti in AEM come Cloud Services?

R: Sono disponibili tre diversi metodi

+ Utilizzo dello strumento Content Transfer (AEM 6.3+ → AEMaaCS)
+ Tramite Gestione pacchetti (AEM → AEMaaCS)
+ Servizio di importazione in blocco predefinito per le risorse (S3/Azure → AEMaaCS)

### D: Esiste un limite alla quantità di contenuto che può essere trasferita utilizzando il CTT?

R: No. Il CTT come strumento potrebbe estrarre AEM sorgente e acquisire in AEMaaCS. Tuttavia, esistono limiti specifici sulla piattaforma AEMaaCS che devono essere presi in considerazione prima della migrazione.

Per ulteriori informazioni, consulta [prerequisiti per la migrazione al cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### D: Ho l&#39;ultimo rapporto BPA dal mio sistema di origine, cosa dovrei fare con esso?

R: Esporta il rapporto come CSV e caricalo su Cloud Acceleration Manager, [associato all’organizzazione IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html). Quindi passa attraverso il processo di revisione come [delineato nella fase di preparazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

Controlla la valutazione della complessità del codice e del contenuto fornita dallo strumento e prendi nota degli elementi di azione associati che portano al backlog di refactoring del codice o alla valutazione della migrazione a Cloud.

### D: È consigliabile estrarre l’autore sorgente e assimilarlo in AEMaaCS author e pubblicare?

R: È sempre consigliabile eseguire l’estrazione e l’acquisizione 1:1 tra i livelli di authoring e pubblicazione. Detto questo, è accettabile estrarre l&#39;autore di produzione sorgente e assimilarlo in Dev, Stage e Production CS.

### D: Esiste un modo per stimare il tempo necessario per migrare il contenuto dal AEM di origine ad AEMaaCS utilizzando il CTT?

R: Poiché il processo di migrazione dipende dalla larghezza della banda Internet, dall&#39;heap allocato per il processo CTT, dalla memoria disponibile e dall&#39;IO del disco che sono soggettivi a ciascun sistema di origine, si raccomanda di eseguire la prova delle migrazioni in anticipo e di estrapolare quei punti di dati per arrivare con stime.

### D: In che modo la mia sorgente AEM le prestazioni vengono influenzate se avvio il processo di estrazione del CTT?

R: Lo strumento CTT viene eseguito nel proprio processo Java™ che richiede fino a 4 gb heap, che è configurabile tramite la configurazione OSGi. Questo numero può cambiare, ma puoi unirti al processo Java™ e scoprirlo.

Se AZCopy è installato e/o l&#39;opzione / funzione di convalida pre-copia abilitata, il processo AZCopy consuma cicli della CPU.

Oltre a jvm , lo strumento utilizza anche l&#39;IO su disco per memorizzare i dati in uno spazio temporaneo e che verrà ripulito dopo il ciclo di estrazione. Oltre alla RAM, CPU e IO disco, lo strumento CTT utilizza anche la larghezza della banda di rete del sistema di origine per caricare i dati nell’archivio BLOB di Azure.

La quantità di risorse che il processo di estrazione del CTT prende dipende dal numero di nodi, dal numero di BLOB e dalla loro dimensione aggregata. È difficile fornire una formula e quindi si consiglia di eseguire una piccola prova di migrazione per determinare i requisiti di upsize del server di origine.

Se per la migrazione vengono utilizzati gli ambienti clone, questo non avrà alcun impatto sull’utilizzo delle risorse del server di produzione live, ma avrà i propri lati negativi per quanto riguarda la sincronizzazione dei contenuti tra produzione live e clone

### D: Nel mio sistema di authoring sorgente, l’SSO è configurato affinché gli utenti si autentichino nell’istanza di authoring. Devo utilizzare la funzione di mappatura utenti del CTT in questo caso?

R: La risposta breve è &quot;**Sì**&quot;.

Estrazione e acquisizione del CTT **senza** la mappatura utente esegue solo la migrazione del contenuto e dei principi associati (utenti, gruppi) dal AEM di origine ad AEMaaCS. Ma esiste un requisito che questi utenti (identità) siano presenti in Adobe IMS e abbiano (con provisioning) accesso all’istanza AEMaaCS per la corretta autenticazione. Il lavoro di [strumento di mappatura utente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) mappare l’utente AEM locale all’utente IMS in modo che l’autenticazione e le autorizzazioni funzionino insieme.

In questo caso, il provider di identità SAML è configurato contro Adobe IMS per utilizzare Federated / Enterprise ID, anziché direttamente per AEM utilizzando il gestore di autenticazione.

### D: Nel mio sistema di authoring sorgente, è configurata l’autenticazione di base per consentire agli utenti di eseguire l’autenticazione nell’istanza di authoring con gli utenti AEM locali. Devo utilizzare la funzione di mappatura utenti del CTT in questo caso?

R: La risposta breve è &quot;**Sì**&quot;.

L’estrazione e l’acquisizione del CTT senza mappatura utente esegue la migrazione del contenuto, i principi associati (utenti, gruppi) da AEM sorgente ad AEMaaCS. Ma esiste un requisito che questi utenti (identità) siano presenti in Adobe IMS e abbiano (con provisioning) accesso all’istanza AEMaaCS per la corretta autenticazione. Il lavoro di [strumento di mappatura utente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) mappare l’utente AEM locale all’utente IMS in modo che l’autenticazione e le autorizzazioni funzionino insieme.

In questo caso, gli utenti utilizzano Adobe ID personale e Adobe ID viene utilizzato dall’amministratore IMS per fornire l’accesso ad AEMaaCS.

### D: Cosa significano i termini &quot;Cancella&quot; e &quot;sovrascrivi&quot; nel contesto del CTT?

R: Nel contesto [fase di estrazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html), le opzioni consistono nella sovrascrittura dei dati nel contenitore di staging dai cicli di estrazione precedenti o nell’aggiunta del differenziale (aggiunto/aggiornato/eliminato). Il contenitore di staging non è altro che il contenitore di archiviazione BLOB associato al set di migrazione. Ogni set di migrazione ottiene il proprio contenitore di staging.

Nel contesto [fase di ingestione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html), le opzioni sono + per sostituire l’intero archivio di contenuti di AEMaaCS o sincronizzare il contenuto differenziale (aggiunto/aggiornato/eliminato) dal contenitore di migrazione di staging.

### D: Ci sono più siti web, risorse associate, utenti, gruppi nel sistema sorgente. È possibile migrarli in più fasi ad AEMaaCS?

R: Sì, è possibile ma richiede un&#39;attenta pianificazione per quanto riguarda:

+ Creazione dei set di migrazione presupponendo che i siti siano presenti nelle rispettive gerarchie
   + Verifica se è accettabile migrare tutte le risorse come parte di un set di migrazione e quindi inserire in più fasi i siti che le utilizzano
+ Nello stato corrente, il processo di acquisizione dell’autore rende l’istanza di authoring non disponibile per l’authoring dei contenuti, anche se il livello di pubblicazione può comunque servire i contenuti
   + Questo significa che fino al completamento dell’acquisizione in author, le attività di authoring dei contenuti vengono congelate

Rivedi il processo di estrazione e acquisizione integrativa come documentato prima di pianificare le migrazioni.

### D: I miei siti web saranno disponibili per gli utenti finali anche se l’acquisizione avviene nelle istanze di authoring o pubblicazione di AEMaaCS?

R: Sì. Il traffico dell’utente finale non viene interrotto dall’attività di migrazione dei contenuti. Tuttavia, l’acquisizione dell’autore blocca l’authoring dei contenuti fino al suo completamento.

### D: Il rapporto BPA mostra gli elementi relativi alle rappresentazioni originali mancanti. Devo pulirli alla fonte prima dell&#39;estrazione?

R: Sì. Il rendering originale mancante indica che il binario della risorsa non viene caricato correttamente al primo posto. Considerati come dati errati, rivedi, esegui il backup utilizzando Gestione pacchetti (come richiesto) e rimuoverli dall’AEM di origine prima di eseguire l’estrazione. I dati errati avranno risultati negativi sui passaggi di elaborazione delle risorse.

### D: Il rapporto BPA contiene elementi relativi a mancanti `jcr:content` nodo per le cartelle. Cosa dovrei fare con loro?

R: Quando `jcr:content` manca a livello di cartella, qualsiasi azione per propagare le impostazioni come profili di elaborazione, ecc. dai genitori si romperà a questo livello. Rivedere il motivo della mancanza `jcr:content`. Anche se è possibile eseguire la migrazione di queste cartelle, si prega di notare che tali cartelle degradano l&#39;esperienza utente e causano cicli di risoluzione dei problemi superflui in un secondo momento.

### D: Ho creato un set di migrazione. è possibile controllarne le dimensioni?

R: Sì, c&#39;è un [Dimensioni controllo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) che fa parte del CTT.

### D: Sto eseguendo la migrazione (estrazione, acquisizione). È possibile verificare che tutto il contenuto estratto sia acquisito in target?

R: Sì, c&#39;è un [convalida](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) che fa parte del CTT.

### D: Il mio cliente ha l’obbligo di spostare i contenuti tra ambienti AEMaaCS, ad esempio da AEMaaCS Dev ad AEMaaCS Stage o AEMaaCS Prod. Posso utilizzare lo strumento di trasferimento dei contenuti per questi casi d’uso?

R: Sfortunatamente No. Il caso d’uso del CTT è la migrazione del contenuto dall’origine in hosting on-premise/AMS AEM 6.3+ agli ambienti cloud AEMaaCS. [Leggi la documentazione CTT](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### D: Che tipo di problemi sono previsti durante l&#39;estrazione?

R: La fase di estrazione è un processo che richiede più aspetti per funzionare come previsto. Prestare attenzione ai diversi tipi di problemi che possono verificarsi e a come attenuarli aumenta il successo complessivo della migrazione dei contenuti.

La documentazione pubblica viene continuamente migliorata in base agli insegnamenti, ma ci sono alcune categorie di problemi di alto livello e possibili ragioni sottostanti.

![AEM problemi di estrazione della migrazione dei contenuti as a Cloud Service](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### D: Che tipo di problemi sono previsti durante l&#39;ingestione?

R: La fase di acquisizione si verifica completamente nella piattaforma cloud e richiede aiuto dalle risorse che hanno accesso all’infrastruttura AEMaaCS. Crea un ticket di supporto per ulteriori informazioni.

Qui sono possibili categorie di problemi (non considerarlo come un elenco esclusivo)

![AEM problemi di acquisizione della migrazione dei contenuti as a Cloud Service](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### D: Il server di origine deve disporre di una connessione Internet in uscita affinché il CTT funzioni?

R: La risposta breve è &quot;**Sì**&quot;.

Il processo CTT richiede la connettività alle risorse seguenti:

+ Il target AEM ambiente as a Cloud Service: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Servizio di archiviazione BLOB di Azure: `casstorageprod.blob.core.windows.net`
+ Endpoint I/O di mappatura utente: `usermanagement.adobe.io`

Per ulteriori informazioni, consulta la documentazione . [connettività sorgente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## Domande correlate sull’elaborazione delle risorse Dynamic Media

### D: Le risorse verranno rielaborate automaticamente dopo l’acquisizione in AEMaaCS?

R: No. Per elaborare le risorse, è necessario avviare la richiesta di rielaborazione.

### D: Le risorse verranno reindicizzate automaticamente dopo l’acquisizione in AEMaaCS?

R: Sì. Le risorse vengono reindicizzate in base alle definizioni di indice disponibili in AEMaaCS.

### D: L’AEM sorgente è integrata con Dynamic Media. Esistono elementi specifici da considerare prima della migrazione dei contenuti?

R: Sì, considera quanto segue quando l&#39;AEM di origine dispone dell&#39;integrazione Dynamic Media.

+ AEMaaCS supporta solo la modalità Scene7 di Dynamic Media. Se il sistema sorgente è in modalità ibrida, è necessaria la migrazione DM alle modalità Scene7.
+ Se l’approccio è quello di migrare dalle istanze di clone di origine, allora è sicuro disabilitare l’integrazione DM su un clone che verrebbe utilizzato per CTT. Questo passaggio serve solo ad evitare qualsiasi scrittura in DM o ad evitare il caricamento sul traffico DM.
+ Il CTT esegue la migrazione dei nodi e dei metadati di un set di migrazione da AEM sorgente ad AEMaaCS. Non eseguirà direttamente alcuna operazione su DM.

### D: Quali sono i diversi approcci di migrazione quando l’integrazione DM è presente su Source AEM?

R: Leggi la domanda e rispondi prima

(Queste sono due opzioni possibili ma non sono limitate solo a queste due). Dipende da come i clienti desiderano avvicinarsi all’UAT, al test delle prestazioni, all’ambiente disponibile e dal fatto che un clone venga utilizzato o meno per la migrazione. Considera questi due punti di partenza come punto di partenza della discussione

**Opzione 1**

Se il numero di risorse/nodi nell’ambiente di origine è inferiore (~100K), supponendo che questi possano essere migrati in un periodo di 24 + 72 ore, compresi estrazione e acquisizione, l’approccio migliore è

+ Eseguire la migrazione direttamente dalla produzione
+ Esegui un’estrazione e un’acquisizione iniziali in AEMaaCS con `wipe=true`
   + Questo passaggio esegue la migrazione di tutti i nodi e binari
+ Continua a lavorare su on-premise / AMS Prod author
+ Da ora in poi, esegui tutte le altre prove di cicli di migrazione con `wipe=true`
   + Questa operazione esegue la migrazione dell’archivio nodi completo, ma solo dei BLOB modificati rispetto agli interi BLOB. Il set di BLOB precedente è presente nell’archivio BLOB di Azure dell’istanza AEMaaCS di destinazione.
   + Utilizza questa bozza di migrazione per misurare la durata della migrazione, la mappatura degli utenti, il test, la convalida di tutte le altre funzionalità
+ Infine, prima della settimana di go-live, esegui una cancellazione=vera migrazione
   + Connetti Dynamic Media su AEMaaCS
   + Disconnetti la configurazione DM da AEM origine locale

Con questa opzione è possibile eseguire la migrazione uno a uno, che significa On-Prem Dev → AEMaaCS Dev, e così via. e sposta le configurazioni DM dai rispettivi ambienti

(Nel caso in cui la migrazione sia pianificata per essere eseguita da Clone)

**Opzione 2**

+ Crea clone dell’autore Produzione, rimuovi la configurazione DM da Clone
+ Migrare on-premise clone → AEMaaCS Dev / Stage
   + Collega brevemente la società DM di produzione ad AEMaaCS Dev/Stage a scopo di convalida
   + Quando la connessione DM è attiva, evita l’inserimento di risorse in AEMaaCS
   + Questo consente loro di convalidare le convalide specifiche CTT e DM
+ Una volta completato il test su AEMaaCS
   + Eseguire una migrazione a Cancella da On-Premise Stage ad AEMaaCS Stage

Esegui una migrazione wipe da on-premise Dev ad AEMaaCS Dev.

L’approccio di cui sopra può essere utilizzato solo per misurare la durata della migrazione, ma richiede una pulizia in un secondo momento.

### Altro materiale di riferimento

+ [Suggerimenti per la migrazione all’Experience Manager nel cloud (Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [Video della serie CTT Expert](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [Video serie di esperti su altri argomenti relativi ad AEMaaCS](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)
