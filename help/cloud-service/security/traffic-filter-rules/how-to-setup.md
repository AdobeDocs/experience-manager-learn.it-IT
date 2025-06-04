---
title: Come impostare le regole di filtro del traffico, incluse le regole WAF
description: Scopri come impostare le regole di filtro del traffico, incluse le regole WAF, per crearle, implementarle, testarle e analizzarne i risultati.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '575'
ht-degree: 100%

---

# Come impostare le regole di filtro del traffico, incluse le regole WAF

Scopri **come impostare** le regole di filtro del traffico, incluse le regole WAF. Scopri come crearle, distribuirle, testarle e analizzarne i risultati.

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## Configurazione

Il processo di configurazione prevede quanto segue:

- _Creazione delle regole_ con una struttura di progetto e un file di configurazione AEM appropriati.
- _Implementazione delle regole_ tramite la pipeline di configurazione di Adobe Cloud Manager.
- _Verifica delle regole_ utilizzando vari strumenti per generare traffico.
- _Analisi dei risultati_ tramite registri CDN di AEMCS e strumenti della dashboard.

### Creare regole nel progetto AEM

Per creare le regole, effettua le seguenti operazioni:

1. Al livello principale del progetto AEM, crea una cartella `config`.

1. Nella cartella `config` crea un nuovo file denominato `cdn.yaml`.

1. Aggiungi al file `cdn.yaml` i seguenti metadati:

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

Vedi un esempio del file `cdn.yaml` all’interno del progetto del sito WKND in AEM Guides:

![File e cartella delle regole del progetto WKND di AEM](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Implementare le regole tramite Cloud Manager {#deploy-rules-through-cloud-manager}

Per implementare le regole, effettua le seguenti operazioni:

1. Accedi a Cloud Manager all’indirizzo [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) e seleziona l’organizzazione e il programma appropriati.

1. Passa alla scheda _Pipeline_ dalla pagina _Panoramica del programma_, fai clic sul pulsante **+Aggiungi** e seleziona il tipo di pipeline desiderato.

   ![Scheda delle pipeline di Cloud Manager](./assets/cloud-manager-pipelines-card.png)

   Nell’esempio precedente, l’opzione _Aggiungi pipeline non di produzione_ è selezionata a scopo di demo, poiché viene utilizzato un ambiente di sviluppo.

1. Nella finestra di dialogo _Aggiungi pipeline non di produzione_, scegli e immetti i seguenti dettagli:

   1. Passaggio di configurazione:

      - **Tipo**: Pipeline di implementazione
      - **Nome pipeline**: Dev-Config

      ![Finestra di dialogo Pipeline di configurazione di Cloud Manager](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Passaggio per il codice sorgente:

      - **Codice da implementare**: Distribuzione mirata
      - **Includi**: Config
      - **Ambiente di implementazione**: nome dell’ambiente, ad esempio wknd-program-dev.
      - **Archivio**: l’archivio Git da cui la pipeline deve recuperare il codice, ad esempio `wknd-site`
      - **Ramo Git**: nome del ramo dell’archivio Git.
      - **Posizione codice**: `/config`, corrisponde alla cartella di configurazione di primo livello creata nel passaggio precedente.

      ![Finestra di dialogo Pipeline di configurazione di Cloud Manager](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### Testare le regole generando traffico

Per testare le regole, sono disponibili vari strumenti di terze parti e la tua organizzazione potrebbe averne uno preferito. A scopo dimostrativo, utilizzeremo i seguenti strumenti:

- [Curl](https://curl.se/) per test di base, ad esempio per richiamare un URL e controllare il codice di risposta.

- [Vegeta](https://github.com/tsenart/vegeta) per l’esecuzione di Denial of Service (DoS). Segui le istruzioni di installazione dal [GitHub di Vegeta](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) per trovare potenziali problemi e vulnerabilità di sicurezza come XSS, SQL Injection e altro ancora. Segui le istruzioni di installazione dal [GitHub di Nikto](https://github.com/sullo/nikto).

- Verifica che gli strumenti siano installati e disponibili nel terminale eseguendo i comandi seguenti:

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### Analizzare i risultati utilizzando gli strumenti della dashboard

Dopo aver creato, distribuito e testato le regole, puoi analizzare i risultati utilizzando i registri **CDN** e **AEMCS-CDN-Log-Analysis-Tooling**. Gli strumenti forniscono un set di dashboard per visualizzare i risultati per gli stack Splunk e ELK (Elasticsearch, Logstash e Kibana).

Gli strumenti della dashboard possono essere clonati direttamente dall’archivio GitHub [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Quindi, segui le istruzioni per installare e caricare le dashboard **Traffico CDN** e **WAF** per lo strumento di osservabilità preferito.

In questo tutorial verrà utilizzato lo stack ELK. Per la configurazione dello stack ELK segui le istruzioni del contenitore [Docker ELK per l’analisi del registro CDN AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md).

- Dopo aver caricato la dashboard di esempio, la pagina dello strumento della dashboard Elastic dovrebbe avere un aspetto simile al seguente:

  ![Dashboard delle regole del filtro del traffico ELK](./assets/elk-dashboard.png)

>[!NOTE]
>
>    Poiché non sono ancora stati acquisiti registri CDN AEMCS, la dashboard è vuota.


## Passaggio successivo

Scopri come dichiarare le regole del filtro del traffico, incluse le regole di WAF nel capitolo [Esempi e analisi dei risultati](./examples-and-analysis.md), utilizzando il progetto AEM del sito WKND.
