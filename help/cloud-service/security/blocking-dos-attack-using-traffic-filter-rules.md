---
title: Blocco degli attacchi DoS e DDoS tramite le regole del filtro del traffico
description: Scopri come bloccare gli attacchi DoS e DDoS utilizzando le regole del filtro del traffico sulla rete CDN fornita da AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1924'
ht-degree: 100%

---

# Blocco degli attacchi DoS e DDoS tramite le regole del filtro del traffico

Scopri come bloccare gli attacchi Denial of Service (DoS) e Distributed Denial of Service (DDoS) utilizzando le regole del **filtro del traffico con il limite di frequenza** e altre strategie nella rete CDN gestita da AEM as a Cloud Service (AEMCS). Questi attacchi causano picchi di traffico sulla rete CDN e potenzialmente sul servizio AEM Publish (ovvero l’origine) e possono influire sulla reattività e sulla disponibilità del sito.

Questa esercitazione funge da guida su _come analizzare i modelli di traffico e configurare le [regole del filtro del traffico](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ per mitigare tali attacchi. Il tutorial descrive anche come [configurare gli avvisi](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) in modo da ricevere una notifica quando si sospetta un attacco.

## Informazioni sui meccanismi di protezione

Ecco le informazioni sulle protezioni DDoS predefinite per il tuo sito web AEM:

- **Memorizzazione in cache:** con criteri di memorizzazione nella cache validi, l’impatto di un attacco DDoS è più limitato perché la rete CDN impedisce alla maggior parte delle richieste di andare all’origine e causare il peggioramento delle prestazioni.
- **Scalabilità automatica:** i servizi di authoring e pubblicazione AEM eseguono la scalabilità automatica per gestire i picchi di traffico, anche se possono ancora essere influenzati da improvvisi e massicci aumenti del traffico.
- **Blocco:** la rete CDN di Adobe blocca il traffico verso l’origine se supera un limite definito da Adobe da un indirizzo IP specifico, per ciascun PoP (punto di presenza) della rete CDN.
- **Avvisi:** il centro azioni invia una notifica di avviso di picco di traffico all’origine quando il traffico supera un determinato limite. Questo avviso viene attivato quando il traffico verso un determinato PoP della rete CDN supera una frequenza di richieste _definita da Adobe_ per indirizzo IP. Per ulteriori informazioni, consulta [Avvisi sulle regole del filtro del traffico](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts).

Queste protezioni integrate devono essere considerate una linea di base per la capacità di un’organizzazione di ridurre al minimo l’impatto sulle prestazioni di un attacco DDoS. Poiché ogni sito web ha caratteristiche di prestazioni diverse ed è possibile notare un peggioramento delle prestazioni prima che venga raggiunto il limite di frequenza definito da Adobe, si consiglia di estendere le protezioni predefinite tramite la _configurazione della clientela_.

Esaminiamo alcune misure aggiuntive consigliate che la clientela può adottare per proteggere i propri siti web dagli attacchi DDoS:

- Dichiarare le **regole del filtro del traffico con limite di frequenza** per bloccare il traffico che supera una determinata velocità da un singolo indirizzo IP, per PoP. Si tratta in genere di una soglia inferiore rispetto al limite di frequenza definito da Adobe.
- Configurare gli **avvisi** sulle regole del filtro del traffico con limite di frequenza tramite un’“azione di avviso” in modo che, quando la regola viene attivata, venga inviata una notifica al Centro azioni.
- Aumentare la copertura della cache dichiarando le **trasformazioni della richiesta** per ignorare i parametri di query.

### Variazioni delle regole del traffico con limite di frequenza {#rate-limit-variations}

Esistono due varianti delle regole del traffico con limite di frequenza:

1. Edge: blocca le richieste in base alla frequenza di tutto il traffico (incluso quello che può essere gestito dalla cache della rete CDN), per un determinato IP, per PoP.
1. Origine: blocca le richieste in base alla frequenza del traffico verso l’origine, per un determinato IP, per PoP.

## Customer Journey

I passaggi seguenti riflettono il probabile processo con il quale ciascun cliente dovrebbe proteggere i propri siti web.

1. Riconoscere la necessità di una regola del filtro del traffico con limite di frequenza. Questo potrebbe essere il risultato della ricezione dell’avviso predefinito di picco del traffico di Adobe verso l’origine, oppure potrebbe essere una decisione proattiva per prendere precauzioni e ridurre il rischio di un DDoS riuscito.
1. Analizzare i modelli di traffico utilizzando una dashboard, se il sito è già attivo, per determinare le soglie ottimali per le regole del filtro del traffico con il limite di frequenza. Se il sito non è ancora attivo, scegliere i valori in base alle aspettative del traffico.
1. Utilizzando i valori del passaggio precedente, configurare le regole del filtro del traffico con limite di frequenza. Assicurarsi di abilitare gli avvisi corrispondenti in modo da ricevere notifiche ogni volta che la soglia viene raggiunta.
1. Ricevere avvisi sulle regole del filtro del traffico ogni volta che si verificano picchi di traffico, per ricevere informazioni utili e capire se l’organizzazione è un bersaglio potenziale di attacchi da parte di attori malevoli.
1. Agisci sull’avviso, in base alle esigenze. Analizza il traffico per determinare se il picco riflette richieste legittime anziché un attacco. Aumenta le soglie se il traffico è legittimo, o abbassale in caso contrario.

Il resto di questo tutorial ti guida attraverso questo processo.

## Riconoscere la necessità di configurare le regole {#recognize-the-need}

Come accennato in precedenza, per impostazione predefinita Adobe blocca il traffico sulla CDN che supera una determinata frequenza. Tuttavia, alcuni siti web potrebbero riscontrare un calo delle prestazioni al di sotto di tale soglia. Pertanto, è necessario configurare le regole del filtro del traffico con limite di frequenza.

Idealmente, puoi configurare le regole prima della pubblicazione in produzione. In concreto, molte organizzazioni dichiarano le regole in modo reattivo solo una volta avvisate di un picco di traffico che indica un probabile attacco.

Adobe invia un avviso di picco di traffico all’origine come [Notifica Centro azioni](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/operations/actions-center) quando viene superata una soglia predefinita di traffico da un singolo indirizzo IP per un determinato PoP. Se hai ricevuto un avviso di questo tipo, ti consigliamo di configurare una regola del filtro del traffico con limite di frequenza. Questo avviso predefinito è diverso dagli avvisi che devono esplicitamente essere abilitati dalla clientela durante la definizione delle regole del filtro del traffico, che saranno trattate in una sezione successiva.

## Analisi dei modelli di traffico {#analyze-traffic}

Se il sito è già attivo, puoi analizzare i modelli di traffico utilizzando i registri CDN e le dashboard fornite da Adobe.

- **Dashboard sul traffico CDN**: fornisce informazioni sul traffico tramite CDN e la frequenza di richieste di origine, le percentuali di errore 4xx e 5xx e le richieste non memorizzate nella cache. Fornisce inoltre il numero massimo di richieste CND e di origine al secondo per indirizzo IP del client e ulteriori informazioni per ottimizzare le configurazioni CDN.

- **Rapporto hit della cache CDN**: fornisce informazioni sul rapporto hit della cache totale e sul numero totale di richieste in base allo stato HIT, PASS e MISS. Inoltre, fornisce gli URL principali HIT, PASS e MISS.

Configura gli strumenti della dashboard utilizzando _una delle opzioni seguenti_:

### ELK - Configurazione degli strumenti della dashboard

Gli strumenti della dashboard **Elasticsearch, Logstash e Kibana (ELK)** forniti da Adobe possono essere utilizzati per analizzare i registri CDN. Questi strumenti includono una dashboard che visualizza i modelli di traffico e semplifica la determinazione delle soglie ottimali per le regole di filtro del traffico con limite di frequenza.

- Clona l’archivio GitHub [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling).
- Configura gli strumenti seguendo i passaggi presenti in [Come configurare il contenitore ELK Docker](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container).
- Come parte della configurazione, importa il file `traffic-filter-rules-analysis-dashboard.ndjson` per visualizzare i dati. La dashboard _Traffico CDN_ include visualizzazioni che mostrano il numero massimo di richieste per IP/POP nell’origine e nella CDN Edge.
- Dalla scheda _Ambienti_ di [Cloud Manager](https://my.cloudmanager.adobe.com/), scarica i registri CDN del servizio di pubblicazione AEMCS.

  ![Download dei registri CDN di Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > Le nuove richieste compariranno nei registri CDN entro 5 minuti.

### Splunk - Configurazione degli strumenti della dashboard

La clientela che dispone dell’[inoltro del registro Splunk abilitato](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) può creare nuove dashboard per analizzare i modelli di traffico.

Per creare dashboard in Splunk, segui i passaggi in [Dashboard Splunk per l’analisi del registro CDN di AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

### Visualizzazione dei dati

Nelle dashboard ELK e Splunk sono disponibili le seguenti visualizzazioni:

- **Edge RPS per IP client e POP**: questa visualizzazione mostra il numero massimo di richieste per IP/POP **nella CDN di Edge**. Il picco nella visualizzazione indica il numero massimo di richieste.

  **Dashboard ELK**:
  ![Dashboard ELK - Numero massimo richieste per IP/POP](./assets/elk-edge-max-per-ip-pop.png)

  **Dashboard Splunk**:
  ![Dashboard Splunk - Numero massimo richieste per IP/POP](./assets/splunk-edge-max-per-ip-pop.png)

- **RPS origine per IP client e POP**: questa visualizzazione mostra il numero massimo di richieste per IP/POP **all’origine**. Il picco nella visualizzazione indica il numero massimo di richieste.

  **Dashboard ELK**:
  ![Dashboard ELK - Numero massimo di richieste di origine per IP/POP](./assets/elk-origin-max-per-ip-pop.png)

  **Dashboard Splunk**:
  ![Dashboard Splunk - Numero massimo di richieste di origine per IP/POP](./assets/splunk-origin-max-per-ip-pop.png)

## Scelta dei valori di soglia

I valori di soglia per le regole di filtro del traffico con limite di frequenza devono essere basati sull’analisi precedente e assicurarsi che il traffico legittimo non venga bloccato. Per istruzioni su come scegliere i valori di soglia, consulta la tabella seguente:

| Variante | Valore |
| :--------- | :------- |
| Origine | Considera il valore più alto del numero massimo di richieste di origine per IP/POP in condizioni di traffico **normali** (ovvero non la frequenza al momento di un DDoS) e aumentalo di un multiplo |
| Edge | Considera il valore più alto del numero massimo di richieste Edge per IP/POP in condizioni di traffico **normali** (ovvero non la frequenza al momento di un DDoS) e aumentalo di un multiplo |

Il multiplo da utilizzare dipende dalle aspettative di picchi normali di traffico dovuti a traffico organico, campagne e altri eventi. Può essere ragionevole utilizzare un multiplo tra 5 e 10.

Se il sito non è ancora attivo, non ci sono dati da analizzare e devi effettuare un’ipotesi plausibile dei valori appropriati da impostare come le regole del filtro del traffico con limite di frequenza. Ad esempio:

| Variante | Valore |
|------------------------------ |:-----------:|
| Edge | 500 |
| Origine | 100 |

## Configurazione delle regole {#configure-rules}

Configura le regole di **filtro del traffico con limite di frequenza** nel file `/config/cdn.yaml` del progetto AEM, con valori basati sulla discussione precedente. Se necessario, rivolgiti al team di sicurezza web per assicurarti che i valori del limite di frequenza siano appropriati e non blocchino il traffico legittimo.

Per ulteriori informazioni, consulta [Creare regole nel progetto AEM](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project).

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
```

Tieni presente che vengono dichiarate sia le regole di origine che le regole Edge e che la proprietà dell’avviso è impostata su `true` in modo da poter ricevere gli avvisi ogni volta che viene raggiunta la soglia, che probabilmente indica un attacco.

Si consiglia di impostare inizialmente il tipo di azione su registro, in modo da poter monitorare il traffico per alcune ore o giorni e assicurarsi che il traffico legittimo non superi queste frequenze. Dopo alcuni giorni, modifica in modalità blocco.

Per distribuire le modifiche all’ambiente AEMCS, segui i passaggi seguenti:

- Esegui il commit e invia le modifiche precedenti all’archivio Git di Cloud Manager.
- Distribuisci le modifiche nell’ambiente AEMCS utilizzando la pipeline di configurazione di Cloud Manager. Per ulteriori dettagli, consulta [Distribuire le regole tramite Cloud Manager](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).
- Per verificare che la **regola del filtro del traffico con limite di frequenza** funzioni come previsto, puoi simulare un attacco come descritto nella sezione [Simulazione di un attacco](#attack-simulation). Limita il numero di richieste a un valore superiore al valore del limite di frequenza impostato nella regola.

### Configurazione delle regole di trasformazione delle richieste {#configure-request-transform-rules}

Oltre alle regole del filtro del traffico con limite di frequenza, si consiglia di utilizzare [trasformazioni della richiesta](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) per annullare l’impostazione dei parametri di query non necessari all’applicazione, al fine di ridurre al minimo i modi per ignorare la cache tramite tecniche di busting della cache. Ad esempio, se desideri consentire solo i parametri di query `search` e `campaignId`, puoi dichiarare la seguente regola:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  requestTransformations:
    rules:
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## Ricevere avvisi delle regole del filtro del traffico {#receiving-alerts}

Come accennato in precedenza, se la regola del filtro del traffico include *alert: true*, viene ricevuto un avviso quando la regola corrisponde.

## Intervenire sugli avvisi {#acting-on-alerts}

A volte, l’avviso è informativo e dà un’idea della frequenza degli attacchi. Vale la pena analizzare i dati CDN utilizzando la dashboard descritta in precedenza, per verificare se il picco di traffico è dovuto a un attacco e non solo a un aumento del volume di traffico legittimo. In quest’ultimo caso, prendi in considerazione di aumentare la soglia.

## Simulazione di un attacco{#attack-simulation}

Questa sezione descrive i metodi per simulare un attacco DoS, che può essere utilizzato per generare dati per le dashboard utilizzate in questo tutorial e per convalidare che ogni regola configurata blocchi correttamente gli attacchi.

>[!CAUTION]
>
> Non utilizzare questi passaggi in un ambiente di produzione. I seguenti passaggi sono solo a scopo di simulazione.
>
>Se hai ricevuto un avviso che indica un picco di traffico, passa alla sezione [Analisi dei modelli di traffico](#analyzing-traffic-patterns).

Per simulare un attacco, è possibile utilizzare strumenti come [Apache Benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html?lang=it), [Apache JMeter](https://jmeter.apache.org/), [Vegeta](https://github.com/tsenart/vegeta) e altri.

### Richieste Edge

Utilizzando il seguente comando [Vegeta](https://github.com/tsenart/vegeta) puoi effettuare molte richieste al tuo sito web:

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=60s | vegeta report
```

Il comando precedente effettua 120 richieste in 5 secondi e genera un rapporto. Supponendo che il sito web non abbia limitazioni di frequenza, questo può causare un picco nel traffico.

### Richieste di origine

Per ignorare la cache CDN ed effettuare richieste all’origine (servizio AEM Publish), puoi aggiungere all’URL un parametro di query univoco. Consulta lo script Apache JMeter di esempio in [Simulare un attacco DoS utilizzando lo script JMeter](https://experienceleague.adobe.com/it/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)

