---
title: Blocco di attacchi DoS, DDoS e sofisticati tramite le regole del filtro del traffico
description: Scopri come bloccare attacchi DoS, DDoS e sofisticati tramite le regole del filtro del traffico in AEM as a Cloud Service.
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
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '609'
ht-degree: 100%

---

# Blocco di attacchi DoS, DDoS e sofisticati tramite le regole del filtro del traffico

Scopri come bloccare attacchi di negazione del servizio (DoS, Denial of Service), attacchi distribuiti di negazione del servizio (DDoS, Distributed Denial of Service) e attacchi sofisticati utilizzando le **regole per il filtro del traffico** nella rete CDN gestita da AEM as a Cloud Service (AEMCS).

Questi attacchi causano picchi di traffico sulla rete CDN e potenzialmente sul servizio AEM Publish (ovvero l’origine) e possono influire sulla reattività e sulla disponibilità del sito.

Questo articolo fornisce una panoramica delle protezioni predefinite per il sito web AEM e di come estenderle tramite configurazione da parte della clientela. Descrive inoltre come analizzare i pattern di traffico e configurare regole standard per il filtro del traffico per bloccare tali attacchi.

## Protezioni predefinite in AEM as a Cloud Service

Ecco le informazioni sulle protezioni DDoS predefinite per il tuo sito web AEM:

- **Memorizzazione in cache:** con criteri di memorizzazione nella cache validi, l’impatto di un attacco DDoS è più limitato perché la rete CDN impedisce alla maggior parte delle richieste di andare all’origine e causare il peggioramento delle prestazioni.
- **Scalabilità automatica:** i servizi di authoring e pubblicazione AEM eseguono la scalabilità automatica per gestire i picchi di traffico, anche se possono ancora essere influenzati da improvvisi e massicci aumenti del traffico.
- **Blocco:** la rete CDN di Adobe blocca il traffico verso l’origine se supera un limite definito da Adobe da un indirizzo IP specifico, per ciascun PoP (punto di presenza) della rete CDN.
- **Avvisi:** il centro azioni invia una notifica di avviso di picco di traffico all’origine quando il traffico supera un determinato limite. Questo avviso viene attivato quando il traffico verso un determinato PoP della rete CDN supera una frequenza di richieste _definita da Adobe_ per indirizzo IP. Per ulteriori informazioni, consulta [Avvisi sulle regole del filtro del traffico](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts).

Queste protezioni integrate devono essere considerate una linea di base per la capacità di un’organizzazione di ridurre al minimo l’impatto sulle prestazioni di un attacco DDoS. Poiché ogni sito web ha caratteristiche di prestazioni diverse ed è possibile notare un degrado delle prestazioni prima che venga raggiunto il limite di frequenza definito da Adobe, si consiglia di estendere le protezioni predefinite tramite la _configurazione della clientela_.

## Estensione della protezione con le regole per il filtro del traffico

Esaminiamo alcune misure aggiuntive consigliate che la clientela può adottare per proteggere i propri siti web dagli attacchi DDoS:

- Implementa le [regole standard per il filtro del traffico](./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md) consigliate da Adobe per identificare i pattern di traffico potenzialmente dannosi tramite la registrazione e l’invio di avvisi in caso di comportamenti sospetti.
- Utilizza il componente aggiuntivo **Protezione WAF-DDoS** o **Sicurezza avanzata** e implementa le [regole per il filtro del traffico WAF](./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md) consigliate da Adobe per difenderti da attacchi sofisticati, inclusi quelli che utilizzano protocolli avanzati o tecniche basate sul payload.
- Aumenta la copertura della cache configurando le [trasformazioni della richiesta](./traffic-filter-and-waf-rules/how-to/request-transformation.md) per ignorare i parametri di query non necessari.

## Introduzione

Esplora i seguenti tutorial per configurare le regole consigliate da Adobe per il blocco degli attacchi.

<!-- CARDS
{target = _self}

* ./traffic-filter-and-waf-rules/setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./traffic-filter-and-waf-rules/assets/setup/rules-setup.png}
  {cta = Start Now}

* ./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/setup.md" title="Come configurare le regole di filtro del traffico incluse le regole WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/setup/rules-setup.png" alt="Come configurare le regole di filtro del traffico incluse le regole WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" title="Come configurare le regole di filtro del traffico incluse le regole WAF">Come configurare le regole per il filtro del traffico incluse le regole WAF</a>
                    </p>
                    <p class="is-size-6">Scopri come configurare, distribuire, testare e analizzare i risultati delle regole di filtro del traffico, incluse le regole WAF.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Inizia ora</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" title="Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png" alt="Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico">Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico</a>
                    </p>
                    <p class="is-size-6">Scopri come proteggere i siti web di AEM dall’abuso di DoS, DDoS e bot utilizzando le regole standard per il filtro del traffico consigliate da Adobe in AEM as a Cloud Service.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Applica le regole</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" title="Protezione dei siti web di AEM tramite le regole per il filtro del traffico WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png" alt="Protezione dei siti web di AEM tramite le regole per il filtro del traffico WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protezione dei siti web di AEM tramite le regole per il filtro del traffico WAF">Protezione dei siti web di AEM tramite le regole per il filtro del traffico WAF</a>
                    </p>
                    <p class="is-size-6">Scopri come proteggere i siti web di AEM da minacce sofisticate, tra cui DoS, DDoS e abusi di bot, utilizzando le regole per il filtro del traffico del firewall per l’applicazione web (WAF) consigliate da Adobe in AEM as a Cloud Service.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Attiva WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
