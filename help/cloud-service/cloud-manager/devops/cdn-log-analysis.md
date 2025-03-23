---
title: Strumenti di analisi del registro CDN
description: Scopri gli strumenti di analisi del registro CDN di AEM Cloud Service forniti da Adobe e come consente di ottenere informazioni sia sulle prestazioni CDN che sull’implementazione di AEM.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Architect, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Strumenti di analisi del registro CDN

Scopri gli _strumenti di analisi del registro CDN di AEM Cloud Service_ forniti da Adobe e come consentono di ottenere informazioni sia sulle prestazioni CDN che sull’implementazione di AEM.
 
>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## Panoramica

Lo [strumento di analisi del registro CDN di AEM as a Cloud Service](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) offre dashboard predefiniti che è possibile integrare con [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) o lo [stack ELK](https://www.elastic.co/elastic-stack) per il monitoraggio e l&#39;analisi in tempo reale dei registri CDN.

Utilizzando questo strumento è possibile eseguire il monitoraggio in tempo reale e il rilevamento proattivo dei problemi. In questo modo, è possibile garantire una distribuzione ottimizzata dei contenuti e adeguate misure di sicurezza contro gli attacchi Denial of Service (DoS) e Distributed Denial of Service (DDoS).

## Funzioni principali

- Analisi dei registri semplificata
- Monitoraggio in tempo reale
- Integrazione perfetta
- Dashboard per
   - Identificare potenziali minacce per la sicurezza
   - Esperienza più rapida per l&#39;utente finale

## Panoramica del dashboard

Per avviare rapidamente l’analisi del registro, Adobe fornisce dashboard predefiniti sia per lo stack Splunk che per lo stack ELK.

- **Percentuale riscontri cache CDN**: fornisce informazioni sulla percentuale di riscontri totali nella cache e sul numero totale di richieste in base allo stato HIT, PASS e MISS. Inoltre, fornisce gli URL principali HIT, PASS e MISS.

  ![Percentuale riscontri cache CDN](assets/CHR-dashboard.png)

- **Dashboard traffico CDN**: fornisce informazioni sul traffico tramite CDN e il tasso di richieste di origine, i tassi di errore 4xx e 5xx e le richieste non memorizzate nella cache. Inoltre, fornisce il massimo di richieste CND e Origin al secondo per indirizzo IP del client e ulteriori informazioni per ottimizzare le configurazioni CDN.

  ![Dashboard traffico CDN](assets/Traffic-dashboard.png)

- **Dashboard di WAF**: fornisce informazioni tramite richieste analizzate, contrassegnate e bloccate. Fornisce inoltre i migliori attacchi da parte dell’ID bandiera di WAF, i primi 100 attacchi da parte dell’IP del client, del paese e dell’agente utente e ulteriori informazioni per ottimizzare le configurazioni di WAF.

  ![Dashboard di WAF](assets/WAF-Dashboard.png)

## Integrazione Splunk

Per le organizzazioni che sfruttano [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) e che hanno abilitato l&#39;inoltro dei registri AEMCS alle proprie istanze Splunk, è possibile importare rapidamente dashboard predefiniti. Questa configurazione facilita l’analisi accelerata dei registri, fornendo informazioni fruibili per ottimizzare le implementazioni di AEM e attenuare le minacce alla sicurezza, come gli attacchi DOS.

È possibile iniziare a utilizzare la guida [Dashboard Splunk per l&#39;analisi del registro CDN di AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).


## Integrazione ELK

Lo stack [ELK](https://www.elastic.co/elastic-stack), che comprende Elasticsearch, Logstash e Kibana, è un&#39;altra potente opzione per l&#39;analisi del registro. È utile per le organizzazioni che non hanno accesso a una configurazione Splunk o alle funzionalità di inoltro del registro. La configurazione locale dello stack ELK è semplice, la strumentazione fornisce il file Docker Compose per iniziare rapidamente. Quindi puoi importare le dashboard predefinite e acquisire i registri CDN scaricati utilizzando Adobe Cloud Manager.

È possibile iniziare a utilizzare il contenitore Docker [ELK per la guida Analisi registro CDN di AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis).
