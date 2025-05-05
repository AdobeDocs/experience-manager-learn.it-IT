---
title: Usa ModSecurity per proteggere il tuo sito AEM da attacchi DoS
description: Scopri come abilitare ModSecurity per proteggere il tuo sito dall’attacco Denial of Service (DoS) utilizzando OWASP ModSecurity Core Rule Set (CRS).
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
workflow-type: tm+mt
source-wordcount: '1171'
ht-degree: 0%

---

# Utilizzare ModSecurity per proteggere il sito AEM dagli attacchi DoS

Scopri come abilitare ModSecurity per proteggere il tuo sito dagli attacchi Denial of Service (DoS) utilizzando **OWASP ModSecurity Core Rule Set (CRS)** nel Dispatcher di pubblicazione Adobe Experience Manager (AEM).


>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## Panoramica

La base [Open Web Application Security Project® (OWASP)](https://owasp.org/) fornisce la [**OWASP Top 10**](https://owasp.org/www-project-top-ten/) che descrive i dieci problemi di sicurezza più critici per le applicazioni web.

ModSecurity è una soluzione open-source e multipiattaforma che fornisce protezione da una serie di attacchi contro le applicazioni web. Consente inoltre il monitoraggio del traffico HTTP, la registrazione e l’analisi in tempo reale.

OWSAP® fornisce anche [OWASP® ModSecurity Core Rule Set (CRS)](https://github.com/coreruleset/coreruleset). CRS è un set di regole generiche di **rilevamento degli attacchi** da utilizzare con ModSecurity. Il CRS mira quindi a proteggere le applicazioni web da un’ampia gamma di attacchi, tra cui i Top Ten di OWASP, con un minimo di falsi avvisi.

Questo tutorial illustra come abilitare e configurare la regola CRS **DOS-PROTECTION** per proteggere il sito da un potenziale attacco DoS.

>[!TIP]
>
>È importante notare che la [rete CDN gestita](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=it) di AEM as a Cloud Service soddisfa i requisiti di prestazioni e sicurezza della maggior parte dei clienti. Tuttavia, ModSecurity fornisce un ulteriore livello di sicurezza e consente regole e configurazioni specifiche per il cliente.

## Aggiungere CRS al modulo di progetto Dispatcher

1. Scarica ed estrai il [set di regole core di OWASP ModSecurity più recente](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. Crea le cartelle `modsec/crs` in `dispatcher/src/conf.d/` nel codice del progetto AEM. Ad esempio, nella copia locale del [progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd).

   ![Cartella CRS nel codice progetto AEM - ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. Copiare la cartella `coreruleset-X.Y.Z/rules` dal pacchetto di rilascio CRS scaricato nella cartella `dispatcher/src/conf.d/modsec/crs`.
1. Copiare il file `coreruleset-X.Y.Z/crs-setup.conf.example` dal pacchetto di rilascio CRS scaricato nella cartella `dispatcher/src/conf.d/modsec/crs` e rinominarlo in `crs-setup.conf`.
1. Disattivare tutte le regole CRS copiate da `dispatcher/src/conf.d/modsec/crs/rules` rinominandole come `XXXX-XXX-XXX.conf.disabled`. È possibile utilizzare i comandi riportati di seguito per rinominare tutti i file contemporaneamente.

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   Vedi regole CRS e file di configurazione rinominati nel codice del progetto WKND.

   ![Regole CRS disabilitate nel codice progetto AEM - ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## Abilitare e configurare la regola di protezione Denial of Service (DoS)

Per abilitare e configurare la regola di protezione Denial of Service (DoS), effettuare le operazioni riportate di seguito.

1. Abilitare la regola di protezione DoS rinominando `REQUEST-912-DOS-PROTECTION.conf.disabled` in `REQUEST-912-DOS-PROTECTION.conf` (o rimuovere `.disabled` dall&#39;estensione del nome della regola) all&#39;interno della cartella `dispatcher/src/conf.d/modsec/crs/rules`.
1. Configura la regola definendo le variabili **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT**.
   1. Creare un file `crs-setup.custom.conf` nella cartella `dispatcher/src/conf.d/modsec/crs`.
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

In questa configurazione della regola di esempio, **DOS_COUNTER_THRESHOLD** è 25, **DOS_BURST_TIME_SLICE** è 60 secondi e **DOS_BLOCK_TIMEOUT** è 600 secondi. Questa configurazione identifica più di due occorrenze di 25 richieste, esclusi i file statici, entro 60 secondi sono qualificati come attacchi DoS, causando il blocco del client richiedente per 600 secondi (o 10 minuti).

>[!WARNING]
>
>Per definire i valori appropriati per le proprie esigenze, collaborare con il team di sicurezza Web.

## Inizializzare CRS

Per inizializzare il CRS, rimuovere i falsi positivi comuni e aggiungere eccezioni locali per il sito, effettua le seguenti operazioni:

1. Per inizializzare CRS, rimuovere `.disabled` dal file **REQUEST-901-INITIALIZATION**. In altre parole, rinominare il file `REQUEST-901-INITIALIZATION.conf.disabled` in `REQUEST-901-INITIALIZATION.conf`.
1. Per rimuovere i falsi positivi comuni come il ping IP locale (127.0.0.1), rimuovere `.disabled` dal file **REQUEST-905-COMMON-EXCEPTIONS**.
1. Per aggiungere eccezioni locali come la piattaforma AEM o i percorsi specifici del sito, rinominare `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` in `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. Aggiungi eccezioni di percorso specifiche per la piattaforma AEM al file appena rinominato.

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

1. Rimuovere inoltre `.disabled` da **REQUEST-910-IP-REPUTATION.conf.disabled** per il controllo del blocco della reputazione IP e `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` per il controllo del punteggio delle anomalie.

>[!TIP]
>
>Durante la configurazione in AEM 6.5, assicurati di sostituire i percorsi precedenti con i rispettivi percorsi AMS o locali che verificano lo stato di AEM (percorsi heartbeat).

## Aggiungi configurazione Apache ModSecurity

Per abilitare ModSecurity (o modulo Apache `mod_security`), effettuare le seguenti operazioni:

1. Crea `modsecurity.conf` alle `dispatcher/src/conf.d/modsec/modsecurity.conf` con le configurazioni chiave seguenti.

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

1. Seleziona il `.vhost` desiderato dal modulo Dispatcher del progetto AEM `dispatcher/src/conf.d/available_vhosts`, ad esempio `wknd.vhost`, aggiungi la voce seguente all&#39;esterno del blocco `<VirtualHost>`.

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

Tutte le configurazioni di _ModSecurity CRS_ e _DOS-PROTECTION_ sopra indicate sono disponibili nel ramo [tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) del progetto AEM WKND Sites per la revisione.

### Convalida configurazione Dispatcher

Quando si lavora con AEM as a Cloud Service, prima di distribuire le _modifiche alla configurazione di Dispatcher_, è consigliabile convalidarle localmente utilizzando lo script `validate` degli [strumenti Dispatcher di AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=it).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## Distribuzione

Distribuisci le configurazioni Dispatcher convalidate localmente utilizzando la pipeline Cloud Manager [Livello Web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=it&#web-tier-config) o [Full Stack](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=it&#full-stack-code). È inoltre possibile utilizzare l&#39;[ambiente di sviluppo rapido](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=it) per velocizzare i tempi di risposta.

## Verifica

Per verificare la protezione DoS, in questo esempio inviamo più di 50 richieste (25 soglie di richiesta per due occorrenze) entro un intervallo di 60 secondi. Tuttavia, queste richieste devono passare attraverso l&#39;AEM as a Cloud Service [predefinito](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=it) o qualsiasi [altro CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=it&#point-to-point-CDN) che occupa il tuo sito Web.

Una tecnica per ottenere il pass-through CDN consiste nell&#39;aggiungere un parametro di query con un **nuovo valore casuale in ogni richiesta di pagina del sito**.

Per attivare un numero maggiore di richieste (50 o più) in un breve periodo (ad esempio 60 secondi), è possibile utilizzare lo strumento Apache [JMeter](https://jmeter.apache.org/) o [Benchmark o ab](https://httpd.apache.org/docs/2.4/programs/ab.html).

### Simulare un attacco DoS utilizzando lo script JMeter

Per simulare un attacco DoS utilizzando JMeter, procedere come segue:

1. [Scarica Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi) e [installalo](https://jmeter.apache.org/usermanual/get-started.html#install) localmente
1. [Esegui](https://jmeter.apache.org/usermanual/get-started.html#running) localmente utilizzando lo script `jmeter` dalla directory `<JMETER-INSTALL-DIR>/bin`.
1. Aprire lo script JMX [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) di esempio in JMeter utilizzando il menu dello strumento **Apri**.

   ![Apri script di prova attacchi DoS WKND di esempio - ModSecurity](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. Aggiorna il valore del campo **Nome server o IP** in _Home Page_ e _Adventure Page_ HTTP Request sampler che corrispondono all&#39;URL dell&#39;ambiente AEM di prova. Rivedi altri dettagli dello script JMeter di esempio.

   ![Richiesta HTTP nome server AEM JMetere - ModSecurity](assets/modsecurity-crs/aem-server-name-http-request.png)

1. Eseguire lo script premendo il pulsante **Start** dal menu Strumenti. Lo script invia 50 richieste HTTP (5 utenti e 10 conteggi di loop) rispetto alla _home page_ e alla _pagina di avventura_ del sito WKND. In totale, quindi, 100 richieste a file non statici qualificano l&#39;attacco DoS per la configurazione personalizzata della regola CRS **DOS-PROTECTION**.

   ![Esegui script JMeter - ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. Il listener JMeter **Visualizza risultati nella tabella** mostra lo stato di risposta **Non riuscito** per il numero di richiesta ~ 53 e versioni successive.

   ![Risposta non riuscita nella visualizzazione dei risultati in Table JMeter - ModSecurity](assets/modsecurity-crs/failed-response-jmeter.png)

1. Per le richieste non riuscite viene restituito il codice di risposta HTTP **503**. È possibile visualizzare i dettagli utilizzando il listener JMeter **Visualizza struttura risultati**.

   ![Risposta 503 JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### Registri di revisione

La configurazione del logger ModSecurity registra i dettagli dell’incidente di attacco DoS. Per visualizzare i dettagli, effettua le seguenti operazioni:

1. Scarica e apri il file di registro `httpderror` di **Pubblica Dispatcher**.
1. Cerca la parola `burst` nel file di registro per visualizzare le righe di **errore**

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. Rivedi i dettagli come _indirizzo IP client_, azione, messaggio di errore e dettagli della richiesta.

## Impatto sulle prestazioni di ModSecurity

L’abilitazione di ModSecurity e delle regole associate ha alcune implicazioni in termini di prestazioni, quindi è importante considerare quali regole sono necessarie, ridondanti e saltate. Collabora con i tuoi esperti di sicurezza web per abilitare e personalizzare le regole CRS.

### Regole aggiuntive

Questa esercitazione abilita e personalizza solo la regola CRS **DOS-PROTECTION** a scopo dimostrativo. Si consiglia di collaborare con esperti di sicurezza web per comprendere, esaminare e configurare le regole appropriate.
