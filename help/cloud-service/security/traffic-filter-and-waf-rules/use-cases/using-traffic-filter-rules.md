---
title: Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico
description: Scopri come proteggere i siti web di AEM da attacchi di negazione del servizio (DoS, Denial of Service) e dagli abusi dei bot utilizzando le regole standard del filtro del traffico consigliate da Adobe in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18307
thumbnail: null
exl-id: 5e235220-82f6-46e4-b64d-315f027a7024
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1780'
ht-degree: 100%

---

# Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico

Scopri come proteggere i siti web di AEM da attacchi di negazione del servizio (DoS, Denial of Service), attacchi distribuiti di negazione del servizio (DDoS, Distributed Denial of Service) e dall’abuso di bot utilizzando le **regole del filtro del traffico standard** _consigliate da Adobe_ in AEM as a Cloud Service.


>[!VIDEO](https://video.tv.adobe.com/v/3469396/?quality=12&learn=on)

## Obiettivi di apprendimento

- Rivedi le regole del filtro del traffico standard consigliato da Adobe.
- Definisci, distribuisci, testa e analizza i risultati delle regole.
- Scopri quando e come perfezionare le regole in base ai pattern di traffico.
- Scopri come utilizzare il Centro azioni di AEM per rivedere gli avvisi generati dalle regole.

### Panoramica di implementazione

I passaggi di implementazione includono:

- Aggiunta delle regole del filtro del traffico standard al file `/config/cdn.yaml` del progetto WKND di AEM.
- Conferma e invia le modifiche all’archivio Git di Cloud Manager.
- Distribuisci le modifiche nell’ambiente AEM utilizzando la pipeline di configurazione di Cloud Manager.
- Test delle regole con simulazione di un attacco DoS tramite [Vegeta](https://github.com/tsenart/vegeta)
- Analisi dei risultati mediante i registri CDN di AEMCS e lo strumento della dashboard ELK.

## Prerequisiti

Prima di procedere, assicurati di aver completato il lavoro preparatorio richiesto come descritto nel tutorial [Come configurare il filtro del traffico e le regole di WAF](../setup.md). Inoltre, devi avere clonato e distribuito il [progetto WKND di AEM Sites](https://github.com/adobe/aem-guides-wknd) nel tuo ambiente AEM.

## Azioni principali delle regole

Prima di approfondire i dettagli delle regole standard per il filtro del traffico, è necessario comprendere le azioni principali eseguite da tali regole. L’attributo `action` di ciascuna regola definisce la risposta del filtro del traffico quando le condizioni vengono soddisfatte. Le azioni includono:

- **Registro**: le regole registrano gli eventi per il monitoraggio e l’analisi, consentendo di rivedere i pattern di traffico e modificare le soglie in base alle esigenze. È specificata dall’attributo `type: log`.

- **Avviso**: le regole attivano gli avvisi quando vengono soddisfatte le condizioni, consentendo di identificare potenziali problemi. È specificata dall’attributo `alert: true`.

- **Blocco**: le regole bloccano il traffico quando vengono soddisfatte le condizioni, impedendo l’accesso al sito AEM. È specificata dall’attributo `action: block`.

## Rivedere e definire le regole

Le regole standard per il filtro del traffico consigliate da Adobe fungono da livello fondamentale per identificare pattern di traffico potenzialmente dannosi, registrando eventi come il superamento dei limiti di frequenza basati su IP e bloccando il traffico proveniente da Paesi specifici. Questi registri aiutano i team a convalidare le soglie e a prendere decisioni consapevoli per **passare in ultima analisi alle regole in modalità di blocco**, senza interrompere il traffico legittimo.

Di seguito è possibile rivedere le tre regole standard per il filtro del traffico da aggiungere al file `/config/cdn.yaml` del progetto AEM WKND:

- **Impedisci attacchi DoS a livello di Edge**: questa regola rileva potenziali attacchi di negazione del servizio (DoS, Denial of Service) nell’edge della rete CDN monitorando le richieste al secondo (RPS) dagli IP client.
- **Impedisci attacchi DoS all’origine**: questa regola rileva potenziali attacchi di negazione del servizio (DoS, Denial of Service) all’origine monitorando le richieste di recupero dagli IP client.
- **Blocca Paesi OFAC**: questa regola blocca l’accesso da specifici Paesi che rientrano nelle restrizioni previste dall’Office of Foreign Assets Control (OFAC, Ufficio di controllo dei beni stranieri).

### &#x200B;1. Impedire i DoS a livello di Edge

Questa regola **invia un avviso** quando rileva un potenziale attacco di negazione del servizio (DoS, Denial of Service) alla rete CDN. I criteri per l’attivazione di questa regola si verificano quando un client supera le **500 richieste al secondo** (media su 10 secondi) per POP CDN (punto di presenza) nell’edge.

Conta **tutte** le richieste e le raggruppa per IP client.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: prevent-dos-attacks-edge
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 500
        window: 10
        penalty: 300
        count: all
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

L’attributo `action` specifica che la regola deve registrare gli eventi e attivare un avviso quando vengono soddisfatte le condizioni. In questo modo è possibile monitorare i potenziali attacchi DoS senza bloccare il traffico legittimo. Tuttavia, l’obiettivo è infine quello di passare alla modalità di blocco di questa regola dopo aver convalidato i pattern di traffico e regolato le soglie.

### &#x200B;2. Impedisci DoS all’origine

Questa regola **invia un avviso** quando rileva un potenziale attacco di negazione del servizio (DoS, Denial of Service) all’origine. I criteri per l’attivazione di questa regola si verificano quando un client supera le **100 richieste al secondo** (media su 10 secondi) per IP client all’origine.

Conta i **recuperi** (richieste che ignorano la cache) e li raggruppa per IP client.

```yaml
...
    - name: prevent-dos-attacks-origin
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 100
        window: 10
        penalty: 300
        count: fetches
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

L’attributo `action` specifica che la regola deve registrare gli eventi e attivare un avviso quando vengono soddisfatte le condizioni. In questo modo è possibile monitorare i potenziali attacchi DoS senza bloccare il traffico legittimo. Tuttavia, l’obiettivo è infine quello di passare alla modalità di blocco di questa regola dopo aver convalidato i pattern di traffico e regolato le soglie.

### &#x200B;3. Blocco dei Paesi OFAC

Questa regola blocca l’accesso da specifici Paesi che rientrano nelle restrizioni previste dall’[OFAC](https://ofac.treasury.gov/sanctions-programs-and-country-information).
È possibile rivedere e modificare l’elenco dei Paesi in base alle esigenze.

```yaml
...
    - name: block-ofac-countries
      when:
        allOf:
          - { reqProperty: tier, in: ["author", "publish"] }
          - reqProperty: clientCountry
            in:
              - SY
              - BY
              - MM
              - KP
              - IQ
              - CD
              - SD
              - IR
              - LR
              - ZW
              - CU
              - CI
      action: block
```

L’attributo `action` specifica che la regola deve bloccare l’accesso dai Paesi specificati. In questo modo è possibile evitare l’accesso al sito AEM da aree geografiche che potrebbero comportare rischi per la sicurezza.

Il file `cdn.yaml` completo con le regole di cui sopra si presenta così:

![Regole YAML CDN di WKND](../assets/use-cases/wknd-cdn-yaml-rules.png)

## Distribuire le regole

Per distribuire le regole, segui questi passaggi:

- Conferma e invia le modifiche all’archivio Git di Cloud Manager.

- Implementa le modifiche nell’ambiente di sviluppo di AEM utilizzando la pipeline di configurazione di Cloud Manager [creata in precedenza](../setup.md#deploy-rules-using-adobe-cloud-manager).

  ![Pipeline di configurazione di Cloud Manager](../assets/use-cases/cloud-manager-config-pipeline.png)

## Test delle regole

Per verificare l’efficacia delle regole standard per il filtro del traffico, sia nell’**edge della rete CDN** che all’**origine**, simula un traffico di richiesta elevato utilizzando [Vegeta](https://github.com/tsenart/vegeta), uno strumento versatile per il testing del carico HTTP.

- Test della regola DoS a livello di Edge (limite di 500 RPS). Il comando seguente simula 200 richieste al secondo per 15 secondi, un valore superiore alla soglia di Edge (500 RPS).

  ```shell
  $echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html" | vegeta attack -rate=200 -duration=15s | vegeta report
  ```

  ![Attacco DoS all’edge con Vegeta](../assets/use-cases/vegeta-dos-attack-edge.png)

  >[!IMPORTANT]
  >
  >  Osserva i codici di stato *100%* e _200_ nel rapporto precedente. Poiché le regole sono impostate su `log` e `alert`, le richieste _non sono bloccate_, ma vengono registrate a scopo di monitoraggio, analisi e per gli avvisi.

- Test della regola DoS all’origine (limite di 100 RPS). Il comando seguente simula 110 richieste di recupero al secondo per 1 secondo, che supera la soglia di origine (100 RPS). Per simulare richieste di esclusione della cache, il file `targets.txt` viene creato con parametri di query univoci per garantire che ogni richiesta venga trattata come una richiesta di recupero.

  ```shell
  # Create targets.txt with unique query parameters
  $for i in {1..110}; do
    echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html?ts=$(date +%s)$i"
  done > targets.txt
  
  # Use the targets.txt file to simulate fetch requests
  $vegeta attack -rate=110 -duration=1s -targets=targets.txt | vegeta report
  ```

  ![Attacco DoS all’origine con Vegeta](../assets/use-cases/vegeta-dos-attack-origin.png)

  >[!IMPORTANT]
  >
  >  Osserva i codici di stato *100%* e _200_ nel rapporto precedente. Poiché le regole sono impostate su `log` e `alert`, le richieste _non sono bloccate_, ma vengono registrate a scopo di monitoraggio, di analisi e per gli avvisi.

- Per semplicità, la regola OFAC non viene testata in questa sede.

## Rivedere gli avvisi

Gli avvisi vengono generati quando vengono attivate le regole per il filtro del traffico. È possibile rivedere questi avvisi nel [Centro azioni AEM](https://experience.adobe.com/aem/actions-center).

![Centro azioni AEM WKND](../assets/use-cases/wknd-aem-action-center.png)

## Analizzare i risultati

Per analizzare i risultati delle regole per il filtro del traffico, puoi utilizzare i registri CDN di AEMCS e lo strumento della dashboard ELK. Segui le istruzioni riportate nella sezione di configurazione per l’[acquisizione dei registri CDN](../setup.md#ingest-cdn-logs) per acquisire tali registri nello stack ELK.

Nella schermata seguente puoi visualizzare i registri CDN dell’ambiente di sviluppo AEM acquisiti nello stack ELK.

![Registri CDN di WKND su ELK](../assets/use-cases/wknd-cdn-logs-elk.png)

Durante gli attacchi DoS simulati, all’interno dell’applicazione ELK, la **dashboard del traffico CDN** dovrebbe mostrare il picco in corrispondenza dell’**edge** e dell’**origine**.

I due pannelli, _RPS Edge per IP client e POP_ e _RPS origine per IP client e POP_, visualizzano le richieste al secondo (RPS) rispettivamente all’edge e all’origine, raggruppate per IP client e punto di presenza (POP).

![Dashboard del traffico Edge della rete CDN di WKND](../assets/use-cases/wknd-cdn-edge-traffic-dashboard.png)

Puoi inoltre utilizzare altri pannelli nella dashboard del traffico CDN per analizzare i pattern di traffico, ad esempio _Primi IP client_, _Primi Paesi_ e _Primi agenti utenti_. Questi pannelli consentono di identificare potenziali minacce e regolare di conseguenza le regole per il filtro del traffico.

### Integrazione Splunk

La clientela che dispone dell’[inoltro del registro Splunk abilitato](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) può creare nuove dashboard per analizzare i modelli di traffico.

Per creare dashboard in Splunk, segui i passaggi in [Dashboard Splunk per l’analisi del registro CDN di AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

La schermata seguente mostra un esempio di dashboard Splunk che visualizza il numero massimo di richieste di origine ed edge per IP, che può aiutarti a identificare potenziali attacchi DoS.

![Dashboard Splunk: numero massimo di richieste origine ed edge per IP](../assets/use-cases/splunk-dashboard-max-origin-edge-requests.png)

## Quando e come perfezionare le regole

L’obiettivo è quello di evitare il blocco del traffico legittimo, proteggendo al contempo il sito AEM da potenziali minacce. Le regole standard per il filtro del traffico sono progettate per notificare e registrare (e infine bloccare quando la modalità viene cambiata) le minacce senza bloccare il traffico legittimo.

Per perfezionare le regole, considera i seguenti passaggi:

- **Monitora i pattern di traffico**: utilizza i registri CDN e la dashboard ELK per monitorare i pattern di traffico e identificare eventuali anomalie o picchi di traffico.
- **Regola le soglie**: in base ai pattern di traffico, regola le soglie (aumenta o diminuisci i limiti di frequenza) nelle regole per adattarle meglio ad esigenze specifiche. Ad esempio, se noti che il traffico legittimo ha attivato gli avvisi, puoi aumentare i limiti di frequenza o regolare i raggruppamenti.
Per istruzioni su come scegliere i valori di soglia, consulta la tabella seguente:

  | Variante | Valore |
  | :--------- | :------- |
  | Origine | Considera il valore più alto del numero massimo di richieste di origine per IP/POP in condizioni di traffico **normali** (ovvero non la frequenza al momento di un DDoS) e aumentalo di un multiplo |
  | Edge | Considera il valore più alto del numero massimo di richieste Edge per IP/POP in condizioni di traffico **normali** (ovvero non la frequenza al momento di un DDoS) e aumentalo di un multiplo |

  Consulta anche la sezione [Scelta dei valori di soglia](../../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) per ulteriori dettagli.

- **Spostati sulle regole di blocco**: dopo aver convalidato i pattern di traffico e regolato le soglie, è necessario effettuare la transizione alle regole in modalità blocco.

## Riepilogo

In questo tutorial, hai imparato a proteggere i siti web di AEM da attacchi di negazione del servizio (DoS, Denial of Service), attacchi distribuiti di negazione del servizio (DDoS, Distributed Denial of Service) e dall’abuso di bot, utilizzando le regole standard per il filtro del traffico consigliate da Adobe in AEM as a Cloud Service.

## Regole WAF consigliate

Scopri come implementare le regole di WAF consigliate da Adobe per proteggere i siti web AEM da minacce sofisticate che utilizzano tecniche avanzate per aggirare le misure di sicurezza tradizionali.

<!-- CARDS
{target = _self}

* ./using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ../assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./using-waf-rules.md" title="Protezione dei siti web di AEM tramite le regole per il filtro del traffico WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/use-cases/using-waf-rules.png" alt="Protezione dei siti web di AEM tramite le regole per il filtro del traffico WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./using-waf-rules.md" target="_self" rel="referrer" title="Protezione dei siti web di AEM tramite le regole per il filtro del traffico WAF">Protezione dei siti web di AEM tramite le regole per il filtro del traffico WAF</a>
                    </p>
                    <p class="is-size-6">Scopri come proteggere i siti web di AEM da minacce sofisticate, tra cui DoS, DDoS e abusi di bot, utilizzando le regole per il filtro del traffico del firewall per l’applicazione web (WAF) consigliate da Adobe in AEM as a Cloud Service.</p>
                </div>
                <a href="./using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Attiva WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Casi d’uso: oltre le regole standard

Per scenari più avanzati, puoi esplorare i seguenti casi d’uso che mostrano come implementare regole per il filtro del traffico personalizzate in base a requisiti di business specifici:

<!-- CARDS
{target = _self}

* ../how-to/request-logging.md

* ../how-to/request-blocking.md

* ../how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-logging.md" title="Monitoraggio delle richieste sensibili" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/wknd-login.png" alt="Monitoraggio delle richieste sensibili"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-logging.md" target="_self" rel="referrer" title="Monitoraggio delle richieste sensibili">Monitoraggio delle richieste sensibili</a>
                    </p>
                    <p class="is-size-6">Scopri come monitorare le richieste sensibili registrandole utilizzando le regole per il filtro del traffico in AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-blocking.md" title="Limitazione dell’accesso" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/elk-tool-dashboard-blocked.png" alt="Limitazione dell’accesso"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-blocking.md" target="_self" rel="referrer" title="Limitazione dell’accesso">Limitazione dell’accesso</a>
                    </p>
                    <p class="is-size-6">Scopri come limitare l’accesso bloccando richieste specifiche tramite le regole per il filtro del traffico in AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-transformation.md" title="Normalizzare le richieste" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/aemrequest-log-transformation.png" alt="Normalizzare le richieste"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-transformation.md" target="_self" rel="referrer" title="Normalizzare le richieste">Normalizzare le richieste</a>
                    </p>
                    <p class="is-size-6">Scopri come normalizzare le richieste trasformandole utilizzando le regole per il filtro del traffico in AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ulteriori informazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Risorse aggiuntive

- [Regole iniziali consigliate](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)
