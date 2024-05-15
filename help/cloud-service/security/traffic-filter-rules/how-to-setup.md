---
title: Come impostare le regole del filtro del traffico, incluse le regole WAF
description: Scopri come configurare per creare, distribuire, testare e analizzare i risultati delle regole del filtro del traffico, incluse le regole WAF.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: b67bf642-3341-48d0-8ea9-5f262febf414
duration: 292
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 3%

---

# Come impostare le regole del filtro del traffico, incluse le regole WAF

Scopri **come impostare** regole del filtro del traffico, incluse le regole WAF. Scopri come creare, distribuire, testare e analizzare i risultati.

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## Configurazione

Il processo di configurazione prevede quanto segue:

- _creazione di regole_ con una struttura di progetto e un file di configurazione AEM appropriati.
- _distribuzione delle regole_ utilizzando la pipeline di configurazione di Adobe Cloud Manager.
- _regole di test_ utilizzo di vari strumenti per generare il traffico.
- _analisi dei risultati_ utilizzo dei registri CDN di AEMCS e degli strumenti del dashboard.

### Creare regole nel progetto AEM

Per creare le regole, effettua le seguenti operazioni:

1. Crea una cartella al livello principale del progetto AEM `config`.

1. All&#39;interno del `config` cartella, crea un nuovo file denominato `cdn.yaml`.

1. Aggiungi i seguenti metadati al `cdn.yaml` file:

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
```

Vedi un esempio di `cdn.yaml` file nel progetto WKND Sites delle guide dell’AEM:

![File e cartella delle regole del progetto WKND AEM](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Distribuire le regole tramite Cloud Manager {#deploy-rules-through-cloud-manager}

Per distribuire le regole, effettua le seguenti operazioni:

1. Accedi a Cloud Manager all’indirizzo [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) e seleziona l’organizzazione e il programma appropriati.

1. Accedi a _Pipeline_ scheda da _Panoramica del programma_ e fai clic su **+Aggiungi** e selezionare il tipo di pipeline desiderato.

   ![Scheda Pipeline di Cloud Manager](./assets/cloud-manager-pipelines-card.png)

   Nell’esempio precedente, a scopo dimostrativo _Aggiungi pipeline non di produzione_ viene selezionato poiché viene utilizzato un ambiente di sviluppo.

1. In _Aggiungi pipeline non di produzione_ , scegli e immetti i seguenti dettagli:

   1. Passaggio di configurazione:

      - **Tipo**: pipeline di distribuzione
      - **Nome pipeline**: Configurazione sviluppo

      ![Finestra di dialogo Pipeline di configurazione di Cloud Manager](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Passaggio del codice sorgente:

      - **Codice da distribuire**: distribuzione mirata
      - **Includi**: Configurazione
      - **Ambiente di implementazione**: nome dell’ambiente, ad esempio wknd-program-dev.
      - **Archivio**: archivio Git da cui la pipeline deve recuperare il codice; ad esempio, `wknd-site`
      - **Ramo Git**: nome del ramo dell’archivio Git.
      - **Posizione codice**: `/config`, corrispondente alla cartella di configurazione di livello principale creata nel passaggio precedente.

      ![Finestra di dialogo Pipeline di configurazione di Cloud Manager](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### Verifica le regole generando traffico

Per testare le regole, sono disponibili vari strumenti di terze parti e la tua organizzazione potrebbe avere uno strumento preferito. A scopo dimostrativo, utilizziamo i seguenti strumenti:

- [Curl](https://curl.se/) per test di base come la chiamata di un URL e la verifica del codice di risposta.

- [Vegeta](https://github.com/tsenart/vegeta) per eseguire il denial of service (DOS). Seguire le istruzioni di installazione fornite da [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) per individuare potenziali problemi e vulnerabilità di sicurezza come XSS, SQL injection e altro ancora. Seguire le istruzioni di installazione fornite da [Nikto GitHub](https://github.com/sullo/nikto).

- Verificare che gli strumenti siano installati e disponibili nel terminale eseguendo i comandi seguenti:

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### Analizzare i risultati utilizzando gli strumenti del dashboard

Dopo aver creato, distribuito e testato le regole, puoi analizzare i risultati utilizzando **Elasticsearch, Logstash e Kibana (ELK)** strumenti del dashboard. È in grado di analizzare i registri CDN di AEMCS e visualizzare i risultati sotto forma di vari grafici.

Gli strumenti del dashboard possono essere clonati direttamente dal [Archivio GitHub AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) e seguire i passaggi per installare e caricare **Regole filtro traffico (incluso WAF)** dashboard.

- Dopo aver caricato il dashboard di esempio, la pagina dello strumento del dashboard elastico dovrebbe avere un aspetto simile a quella riportata di seguito:

  ![Dashboard delle regole del filtro del traffico ELK](./assets/elk-dashboard.png)

>[!NOTE]
>
>    Poiché non sono ancora stati acquisiti registri CDN AEMCS, la dashboard è vuota.


## Passaggio successivo

Scopri come dichiarare le regole del filtro del traffico, incluse le regole WAF, in [Esempi e analisi dei risultati](./examples-and-analysis.md) capitolo, utilizzando il progetto WKND Sites dell’AEM.
