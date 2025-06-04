---
title: Utilizzare ModSecurity per proteggere il sito AEM da attacchi DoS
description: Scopri come abilitare ModSecurity per proteggere il tuo sito dall’attacco DoS (Denial of Service) utilizzando il ModSecurity CRS (Core Rule Set) di OWASP.
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
jira: KT-10385
thumbnail: KT-10385.png
doc-type: Article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
duration: 783
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1171'
ht-degree: 100%

---

# Utilizzare ModSecurity per proteggere il sito AEM dagli attacchi DoS

Scopri come abilitare ModSecurity per proteggere il tuo sito dagli attacchi DoS (Denial of Service) utilizzando il **ModSecurity CRS (Core Rule Set) di OWASP** nel Publish Dispatcher di Adobe Experience Manager (AEM).


>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## Panoramica

La fondazione [OWASP (Open Web Application Security Project®)](https://owasp.org/) fornisce la [**OWASP Top 10**](https://owasp.org/www-project-top-ten/) che delinea i dieci problemi di sicurezza più critici per le applicazioni web.

ModSecurity è una soluzione open-source e multipiattaforma che fornisce protezione da una serie di attacchi contro le applicazioni web. Inoltre, consente il monitoraggio del traffico HTTP, la registrazione e l’analisi in tempo reale.

OWSAP® fornisce anche il [ModSecurity CRS (Core Rule Set ) OWASP®](https://github.com/coreruleset/coreruleset). Il CRS è un set di regole generiche di **rilevamento degli attacchi** da utilizzare con ModSecurity. Il CRS mira quindi a proteggere le applicazioni web da un’ampia gamma di attacchi, tra cui i dieci più pericolosi di OWASP, con un minimo di falsi avvisi.

Questo tutorial illustra come abilitare e configurare la regola CRS **DOS-PROTECTION** per proteggere il sito da un potenziale attacco DoS.

>[!TIP]
>
>È importante notare che la rete [CDN](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) gestita di AEM as a Cloud Service soddisfa i requisiti di prestazioni e sicurezza della maggior parte dei clienti. Tuttavia, ModSecurity fornisce un ulteriore livello di sicurezza e consente regole e configurazioni specifiche per il cliente.

## Aggiungere CRS al modulo di progetto Dispatcher

1. Scarica ed estrai il [ModSecurity CRS di OWASP più recente](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. Crea le cartelle `modsec/crs` in `dispatcher/src/conf.d/` nel codice del progetto AEM. Ad esempio, nella copia locale del [progetto AEM del sito WKND](https://github.com/adobe/aem-guides-wknd).

   ![Cartella CRS nel codice progetto AEM - ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. Copia la cartella `coreruleset-X.Y.Z/rules` dal pacchetto di rilascio CRS scaricato nella cartella `dispatcher/src/conf.d/modsec/crs`.
1. Copia il file `coreruleset-X.Y.Z/crs-setup.conf.example` dal pacchetto di rilascio CRS scaricato nella cartella `dispatcher/src/conf.d/modsec/crs` e rinominalo in `crs-setup.conf`.
1. Disattiva tutte le regole CRS copiate da `dispatcher/src/conf.d/modsec/crs/rules` rinominandole come `XXXX-XXX-XXX.conf.disabled`. Puoi utilizzare i comandi riportati di seguito per rinominare tutti i file contemporaneamente.

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   Consulta le regole CRS e i file di configurazione rinominati nel codice del progetto WKND.

   ![Regole CRS disabilitate nel codice del progetto AEM - ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## Abilitare e configurare la regola di protezione DoS (Denial of Service)

Per abilitare e configurare la regola di protezione DoS (Denial of Service), segui i passaggi seguenti:

1. Abilita la regola di protezione DoS rinominando `REQUEST-912-DOS-PROTECTION.conf.disabled` in `REQUEST-912-DOS-PROTECTION.conf` (o rimuovi `.disabled` dall’estensione del nome della regola) all’interno della cartella `dispatcher/src/conf.d/modsec/crs/rules`.
1. Configura la regola definendo le variabili **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT**.
   1. Crea un file `crs-setup.custom.conf` all’interno della cartella `dispatcher/src/conf.d/modsec/crs`.
   1. Aggiungi lo snippet di regola seguente al file appena creato.

   ```
   # The Denial of Service (DoS) protection against clients making requests too quickly.
   # When a client is making more than 25 requests (excluding static files) within
   # 60 seconds, this is considered a 'burst'. After two bursts, the client is
   # blocked for 600 seconds.
   SecAction \
       "id:900700,\
       phase:1,\
       nolog,\
       pass,\
       t:none,\
       setvar:'tx.dos_burst_time_slice=60',\
       setvar:'tx.dos_counter_threshold=25',\
       setvar:'tx.dos_block_timeout=600'"    
   ```

In questa configurazione di regola di esempio, **DOS_COUNTER_THRESHOLD** è 25, **DOS_BURST_TIME_SLICE** è 60 secondi e **DOS_BLOCK_TIMEOUT** è 600 secondi. Con questa configurazione, se entro 60 secondi si verificano più di due occorrenze di 25 richieste, esclusi i file statici, queste vengono qualificate come attacchi DoS e il client richiedente viene bloccato per 600 secondi (o 10 minuti).

>[!WARNING]
>
>Per definire i valori appropriati per le tue esigenze, collabora con il team di sicurezza web.

## Inizializzare il CRS

Per inizializzare il CRS, rimuovere i falsi positivi comuni e aggiungere eccezioni locali per il sito, effettua le seguenti operazioni:

1. Per inizializzare il CRS, rimuovi `.disabled` dal file **REQUEST-901-INITIALIZATION**. In altre parole, rinomina il file `REQUEST-901-INITIALIZATION.conf.disabled` in `REQUEST-901-INITIALIZATION.conf`.
1. Per rimuovere i falsi positivi più comuni come il ping IP locale (127.0.0.1), rimuovi `.disabled` dal file **REQUEST-905-COMMON-EXCEPTIONS**.
1. Per aggiungere eccezioni locali come i percorsi specifici della piattaforma AEM o del tuo sito, rinomina `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` in `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. Aggiungi le eccezioni per il percorso specifico della piattaforma AEM al file appena rinominato.

   ```
   ########################################################
   # AEM as a Cloud Service exclusions                    #
   ########################################################
   # Ignoring AEM-CS Specific internal and reserved paths
   
   SecRule REQUEST_URI "@beginsWith /systemready" \
       "id:1010,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"    
   
   SecRule REQUEST_URI "@beginsWith /system/probes" \
       "id:1011,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   SecRule REQUEST_URI "@beginsWith /gitinit-status" \
       "id:1012,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   ########################################################
   # ADD YOUR SITE related exclusions                     #
   ########################################################
   ...
   ```

1. Rimuovi inoltre `.disabled` da **REQUEST-910-IP-REPUTATION.conf.disabled** per il controllo del blocco della reputazione IP e `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` per il controllo del punteggio delle anomalie.

>[!TIP]
>
>Durante la configurazione in AEM 6.5, assicurati di sostituire i percorsi precedenti con i rispettivi percorsi AMS o locali che verificano lo stato di AEM (percorsi heartbeat).

## Aggiungere una configurazione ModSecurity Apache

Per abilitare ModSecurity (o modulo Apache `mod_security`), effettua le seguenti operazioni:

1. Crea `modsecurity.conf` in `dispatcher/src/conf.d/modsec/modsecurity.conf` con le configurazioni chiave seguenti.

   ```
   # Include the baseline crs setup
   Include conf.d/modsec/crs/crs-setup.conf
   
   # Include your customizations to crs setup if exist
   IncludeOptional conf.d/modsec/crs/crs-setup.custom.conf
   
   # Select all available CRS rules:
   #Include conf.d/modsec/crs/rules/*.conf
   
   # Or alternatively list only specific ones you want to enable e.g.
   Include conf.d/modsec/crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   Include conf.d/modsec/crs/rules/REQUEST-901-INITIALIZATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-905-COMMON-EXCEPTIONS.conf
   Include conf.d/modsec/crs/rules/REQUEST-910-IP-REPUTATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf
   Include conf.d/modsec/crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
   
   # Start initially with engine off, then switch to detection and observe, and when sure enable engine actions
   #SecRuleEngine Off
   #SecRuleEngine DetectionOnly
   SecRuleEngine On
   
   # Remember to use relative path for logs:
   SecDebugLog logs/httpd_mod_security_debug.log
   
   # Start with low debug level
   SecDebugLogLevel 0
   #SecDebugLogLevel 1
   
   # Start without auditing
   SecAuditEngine Off
   #SecAuditEngine RelevantOnly
   #SecAuditEngine On
   
   # Tune audit accordingly:
   SecAuditLogRelevantStatus "^(?:5|4(?!04))"
   SecAuditLogParts ABIJDEFHZ
   SecAuditLogType Serial
   
   # Remember to use relative path for logs:
   SecAuditLog logs/httpd_mod_security_audit.log
   
   # You might still use /tmp for temporary/work files:
   SecTmpDir /tmp
   SecDataDir /tmp
   ```

1. Seleziona il `.vhost` desiderato dal modulo Dispatcher `dispatcher/src/conf.d/available_vhosts` del progetto AEM, ad esempio `wknd.vhost`, e aggiungi la voce seguente all’esterno del blocco `<VirtualHost>`.

   ```
   # Enable the ModSecurity and OWASP CRS
   <IfModule mod_security2.c>
       Include conf.d/modsec/modsecurity.conf
   </IfModule>
   
   ...
   
   <VirtualHost *:80>
       ServerName    "publish"
       ...
   </VirtualHost>
   ```

Puoi rivedere tutte le configurazioni di _ModSecurity CRS_ e _DOS-PROTECTION_ sopra indicate nel ramo [tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) del progetto AEM del sito WKND.

### Convalidare la configurazione di Dispatcher

Quando si lavora con AEM as a Cloud Service, prima di implementare le modifiche apportate alla _configurazione di Dispatcher_, è consigliabile convalidarle localmente utilizzando lo script `validate` degli [strumenti Dispatcher di AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=it).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## Distribuzione

Implementa le configurazioni Dispatcher convalidate localmente utilizzando la pipeline [Livello web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=it#web-tier-config) o [Full stack](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=it#full-stack-code) di Cloud Manager. È inoltre possibile utilizzare l’[ambiente di sviluppo rapido](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=it) per velocizzare i tempi.

## Verificare

Per verificare la protezione DoS, in questo esempio inviamo più di 50 richieste (soglia di 25 richieste per due occorrenze) entro un intervallo di 60 secondi. Tuttavia, queste richieste devono passare attraverso la CDN [incorporata](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) di AEM as a Cloud Service o qualsiasi [altra CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=it#point-to-point-CDN) utilizzata per il tuo sito web.

Una tecnica per ottenere il pass-through CDN consiste nell’aggiungere un parametro di query con un **nuovo valore casuale in ogni richiesta di pagina del sito**.

Per attivare un numero maggiore di richieste (50 o più) in un breve periodo (ad esempio 60 secondi), è possibile utilizzare lo strumento Apache [JMeter](https://jmeter.apache.org/) o [Benchmark o ab](https://httpd.apache.org/docs/2.4/programs/ab.html?lang=it).

### Simulare un attacco DoS utilizzando lo script JMeter

Per simulare un attacco DoS utilizzando JMeter, effettua i passaggi seguenti:

1. [Scarica Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi) e [installalo](https://jmeter.apache.org/usermanual/get-started.html#install) localmente.
1. [Esegui](https://jmeter.apache.org/usermanual/get-started.html#running) localmente utilizzando lo script `jmeter` dalla directory `<JMETER-INSTALL-DIR>/bin`.
1. Apri lo script JMX [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) di esempio in JMeter utilizzando il menu **Apri** dello strumento.

   ![Aprire lo script JMX di esempio di WKND per testare attacchi DoS - ModSecurity](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. Nell’esempio di richiesta, aggiorna il valore del campo **Server Name or IP** per _Home Page_ e _Adventure Page_, in modo che corrisponda all’URL del tuo ambiente AEM di test. Rivedi altri dettagli dello script JMeter di esempio.

   ![Richiesta HTTP con nome del server AEM in JMeter - ModSecurity](assets/modsecurity-crs/aem-server-name-http-request.png)

1. Esegui lo script premendo il pulsante **Start** dal menu dello strumento. Lo script invia 50 richieste HTTP (5 utenti e 10 loop) alle pagine _Home Page_ e _Adventure Page_ del sito WKND. In totale, quindi, 100 richieste a file non statici, qualificandosi come attacco DoS secondo la configurazione personalizzata della regola CRS **DOS-PROTECTION**.

   ![Eseguire lo script JMeter - ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. Il listener **View Results in Table** di JMeter mostra lo stato di risposta **non riuscito** per le richieste a partire dal numero 53.

   ![Risposta non riuscita nella tabella dei risultati di JMeter - ModSecurity](assets/modsecurity-crs/failed-response-jmeter.png)

1. Per le richieste non riuscite viene restituito il **codice di risposta HTTP 503**. È possibile visualizzare i dettagli utilizzando il listener **View Results in Table** di JMeter.

   ![Risposta 503 di JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### Rivedere i registri

La configurazione del logger ModSecurity registra i dettagli dell’incidente di attacco DoS. Per visualizzare i dettagli, effettua le seguenti operazioni:

1. Scarica e apri il file di registro `httpderror` di **Publish Dispatcher**.
1. Cerca la parola `burst` nel file di registro per visualizzare le righe di **errore**

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. Rivedi i dettagli come _indirizzo IP client_, azione, messaggio di errore e dettagli della richiesta.

## Impatto sulle prestazioni di ModSecurity

L’abilitazione di ModSecurity e delle regole associate ha alcune implicazioni in termini di prestazioni, quindi è importante considerare quali regole sono necessarie, ridondanti e ignorate. Collabora con i tuoi esperti di sicurezza web per abilitare e personalizzare le regole CRS.

### Regole aggiuntive

Questo tutorial abilita e personalizza solo la regola CRS **DOS-PROTECTION** a scopo dimostrativo. È consigliata collaborare con esperti di sicurezza web per comprendere, rivedere e configurare le regole appropriate.
