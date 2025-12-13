---
title: Analisi del rapporto di hit della cache CDN
description: Scopri come analizzare i registri CDN forniti da AEM as a Cloud Service. Ottieni informazioni approfondite come il rapporto hit della cache e i principali URL di tipi di cache MISS e PASS a scopo di ottimizzazione.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
exl-id: 43aa7133-7f4a-445a-9220-1d78bb913942
duration: 276
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1476'
ht-degree: 0%

---

# Analisi del rapporto di hit della cache CDN

Il contenuto memorizzato nella cache della rete CDN riduce la latenza per gli utenti del sito web che non devono attendere che la richiesta torni al servizio di pubblicazione di Apache/Dispatcher o AEM. Tenendo presente questo aspetto, vale la pena ottimizzare il rapporto di hit della cache CDN per massimizzare la quantità di contenuto memorizzabile in cache sulla CDN.

Scopri come analizzare i **registri CDN** forniti da AEM as a Cloud Service e ottenere informazioni quali **proporzioni di hit della cache** e **URL principali di _tipi di cache MISS_ e _PASS_**, a scopo di ottimizzazione.


I registri CDN sono disponibili in formato JSON, che contiene vari campi tra cui `url`, `cache`. Per ulteriori informazioni, vedere [Formato di registro CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=en#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). Il campo `cache` fornisce informazioni sullo stato _della cache_ e i valori possibili sono HIT, MISS o PASS. Esaminiamo i dettagli dei valori possibili.

| Stato della cache </br> valore possibile | Descrizione |
|------------------------------------|:-----------------------------------------------------:|
| HIT | I dati richiesti sono _trovati nella cache CDN e non è necessario eseguire una richiesta di recupero_ al server AEM. |
| SIGNORINA | I dati richiesti sono _non trovati nella cache CDN e devono essere richiesti_ dal server AEM. |
| PASS | I dati richiesti sono _impostati in modo esplicito per non essere memorizzati nella cache_ e per essere sempre recuperati dal server AEM. |

Ai fini di questa esercitazione, il [progetto AEM WKND](https://github.com/adobe/aem-guides-wknd) viene distribuito nell&#39;ambiente AEM as a Cloud Service e viene attivato un piccolo test delle prestazioni utilizzando [Apache JMeter](https://jmeter.apache.org/).

Questo tutorial è strutturato in modo da illustrare il processo seguente:

1. Download dei registri CDN tramite Cloud Manager
1. L’analisi di questi registri CDN può essere eseguita con due approcci: un dashboard installato localmente o un notebook Splunk o Jupityer accessibile in remoto (per chi ha una licenza di Adobe Experience Platform)
1. Ottimizzazione della configurazione della cache CDN

## Scarica registri CDN

Per scaricare i registri CDN, effettua le seguenti operazioni:

1. Accedi a Cloud Manager all&#39;indirizzo [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) e seleziona l&#39;organizzazione e il programma.

1. Per un ambiente AEMCS desiderato, seleziona **Scarica registri** dal menu con i puntini di sospensione.

   ![Scarica registri - Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. Nella finestra di dialogo **Scarica registri**, seleziona il servizio **Pubblica** dal menu a discesa, quindi fai clic sull&#39;icona di download accanto alla riga **CDN**.

   ![Registri CDN - Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


Se il file di registro scaricato è di _today_, l&#39;estensione del file è `.log`. In caso contrario, per i file di registro passati l&#39;estensione è `.log.gz`.

## Analizzare i registri CDN scaricati

Per ottenere informazioni approfondite, ad esempio il rapporto hit della cache e i principali URL dei tipi di cache di tipo MISS e PASS, analizza il file di registro CDN scaricato. Queste informazioni consentono di ottimizzare la [configurazione della cache CDN](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching) e migliorare le prestazioni del sito.

Per analizzare i registri CDN, questa esercitazione presenta tre opzioni:

1. **Elasticsearch, Logstash e Kibana (ELK)**: è possibile installare localmente gli strumenti del dashboard [ELK](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md).
1. **Splunk**: [Gli strumenti del dashboard Splunk](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md) richiedono l&#39;accesso a Splunk e [l&#39;inoltro dei registri AEMCS è abilitato](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) per acquisire i registri CDN.
1. **Jupyter Notebook**: è possibile accedervi in remoto come parte di [Adobe Experience Platform](https://experienceleague.adobe.com/en/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data) senza installare software aggiuntivo, per i clienti che hanno concesso in licenza Adobe Experience Platform.

### Opzione 1: utilizzo degli strumenti del dashboard ELK

Lo [stack ELK](https://www.elastic.co/elastic-stack) è un insieme di strumenti che forniscono una soluzione scalabile per la ricerca, l&#39;analisi e la visualizzazione dei dati. È costituito da Elasticsearch, Logstash e Kibana.

Per identificare i dettagli chiave, utilizziamo il progetto [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Questo progetto fornisce un contenitore Docker dello stack ELK e un dashboard Kibana preconfigurato per analizzare i registri CDN.

1. Segui i passaggi da [Impostare il contenitore Docker ELK](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container) e assicurati di importare il dashboard Kibana **Rapporto riscontri cache CDN**.

1. Per identificare la percentuale di riscontri nella cache CDN e i primi URL, effettua le seguenti operazioni:

   1. Copiare i file di registro CDN scaricati all&#39;interno della cartella dei registri specifica dell&#39;ambiente, ad esempio `ELK/logs/stage`.

   1. Apri il dashboard **Rapporto hit cache CDN** facendo clic sull&#39;angolo in alto a sinistra _Menu di navigazione > Analytics > Dashboard > Rapporto hit cache CDN_.

      ![Percentuale riscontri cache CDN - Dashboard Kibana](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. Seleziona l’intervallo di tempo desiderato dall’angolo in alto a destra.

      ![Intervallo di tempo - Dashboard Kibana](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. Il dashboard **Rapporto riscontri cache CDN** è auto-esplicativo.

   1. Nella sezione _Analisi totale richieste_ sono visualizzati i dettagli seguenti:
      - Rapporti cache per tipo di cache
      - Conteggi cache per tipo di cache

      ![Analisi totale richieste - Dashboard Kibana](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. L&#39;_Analisi per richiesta o tipi MIME_ visualizza i dettagli seguenti:
      - Rapporti cache per tipo di cache
      - Conteggi cache per tipo di cache
      - Primi URL mancanti e passati

      ![Analisi per tipo di richiesta o MIME - Dashboard Kibana](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### Filtraggio per nome di ambiente o ID programma

Per filtrare i registri acquisiti per nome dell’ambiente, segui i passaggi seguenti:

1. Nel dashboard Rapporto riscontri cache CDN, fai clic sull&#39;icona **Aggiungi filtro**.

   ![Filtro - Dashboard Kibana](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. Nel modale **Aggiungi filtro**, seleziona il campo `aem_env_name.keyword` dal menu a discesa, quindi l&#39;operatore `is` e il nome dell&#39;ambiente desiderato per il campo successivo e infine fai clic su _Aggiungi filtro_.

   ![Aggiungi filtro - Dashboard Kibana](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### Filtraggio per nome host

Per filtrare i registri acquisiti per nome host, segui i passaggi seguenti:

1. Nel dashboard Rapporto riscontri cache CDN, fai clic sull&#39;icona **Aggiungi filtro**.

   ![Filtro - Dashboard Kibana](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. Nel modale **Aggiungi filtro**, seleziona il campo `host.keyword` dal menu a discesa, quindi l&#39;operatore `is` e il nome host desiderati per il campo successivo e infine fai clic su _Aggiungi filtro_.

   ![Filtro host - Dashboard Kibana](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

Allo stesso modo, aggiungi altri filtri al dashboard in base ai requisiti di analisi.

### Opzione 2: utilizzo degli strumenti del dashboard Splunk

[Splunk](https://www.splunk.com/) è un popolare strumento di analisi dei registri che consente di aggregare, analizzare i registri e creare visualizzazioni a scopo di monitoraggio e risoluzione dei problemi.

Per identificare i dettagli chiave, utilizziamo il progetto [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Questo progetto fornisce un dashboard Splunk per analizzare i registri CDN.

1. Segui i passaggi da [Dashboard Splunk per l&#39;analisi del registro CDN di AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md) e assicurati di importare il dashboard Splunk con **Proporzioni hit cache CDN**.
1. Se necessario, aggiornare i valori del filtro _Index, Source Type e altri_ nel dashboard Splunk.

   ![Dashboard Splunk](assets/cdn-logs-analysis/splunk-CHR-dashboard.png){width="500" zoomable="yes"}

>[!NOTE]
>
>L’interfaccia utente e i grafici nel dashboard di splunk sono diversi dal dashboard ELK, tuttavia, i dettagli delle chiavi sono simili.

### Opzione 3: utilizzo di Jupyter Notebook

Per coloro che preferiscono non installare il software localmente (ovvero, gli strumenti del dashboard ELK della sezione precedente), esiste un&#39;altra opzione, ma richiede una licenza per Adobe Experience Platform.

[Jupyter Notebook](https://jupyter.org/) è un&#39;applicazione Web open source che consente di creare documenti contenenti codice, testo e visualizzazione. Viene utilizzato per la trasformazione dei dati, la visualizzazione e la modellazione statistica. È possibile accedervi in remoto [come parte di Adobe Experience Platform](https://experienceleague.adobe.com/en/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data).

#### Download del file del blocco appunti Python interattivo

Scarica innanzitutto il file [AEM-as-a-CloudService - CDN Logs Analysis - Jupyter Notebook](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb), utile per l&#39;analisi dei registri CDN. Questo file &quot;Interactive Python Notebook&quot; è auto-esplicativo, tuttavia, gli elementi di rilievo di ogni sezione sono:

- **Installa librerie aggiuntive**: installa le librerie Python `termcolor` e `tabulate`.
- **Carica registri CDN**: carica il file di registro CDN utilizzando il valore della variabile `log_file`. Assicurarsi di aggiornarne il valore. Trasforma inoltre questo registro CDN in [DataFrame Pandas](https://pandas.pydata.org/docs/reference/frame.html).
- **Esegui analisi**: il primo blocco di codice è _Visualizza risultati analisi per richieste totali, HTML, JS/CSS e immagini_. Fornisce i grafici percentuale hit della cache, a barre e a torta.
Il secondo blocco di codice è _Primi 5 URL di richiesta non recapitati e passati per HTML, JS/CSS e Image_. Vengono visualizzati gli URL e i relativi conteggi in formato tabella.

#### Esecuzione del blocco appunti Jupyter

Quindi, esegui Jupyter Notebook in Adobe Experience Platform, seguendo questi passaggi:

1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com/), nella home page > **sezione Accesso rapido** > fai clic su **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. Nella home page di Adobe Experience Platform > sezione Data Science >, fai clic sulla voce di menu **Notebooks**. Per avviare l&#39;ambiente Jupyter Notebooks, fare clic sulla scheda **JupyterLab**.

   ![Aggiornamento del valore del file di registro del blocco appunti](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. Nel menu JupyterLab, utilizzando l&#39;icona **Carica file**, caricare il file di registro CDN scaricato e il file `aemcs_cdn_logs_analysis.ipynb`.

   ![Carica file - JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. Aprire il file `aemcs_cdn_logs_analysis.ipynb` facendo doppio clic.

1. Nella sezione **Load CDN Log File** del blocco appunti, aggiornare il valore `log_file`.

   ![Aggiornamento del valore del file di registro del blocco appunti](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. Per eseguire la cella selezionata e avanzare, fare clic sull&#39;icona **Riproduci**.

   ![Aggiornamento del valore del file di registro del blocco appunti](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. Dopo aver eseguito la cella di codice **Visualizza risultati analisi per totali, HTML, JS/CSS e richieste di immagini**, l&#39;output visualizza i grafici percentuale di hit della cache, a barre e a torta.

   ![Aggiornamento del valore del file di registro del blocco appunti](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. Dopo aver eseguito i **Primi 5 URL di richieste MISS e PASS per la cella di codice HTML, JS/CSS e Image**, nell&#39;output vengono visualizzati i primi 5 URL di richieste MISS e PASS.

   ![Aggiornamento del valore del file di registro del blocco appunti](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

Puoi migliorare Jupyter Notebook per analizzare i registri CDN in base alle tue esigenze.

## Ottimizzazione della configurazione della cache CDN

Dopo aver analizzato i registri CDN, puoi ottimizzare la configurazione della cache CDN per migliorare le prestazioni del sito. La best practice di AEM prevede un rapporto hit cache del 90% o superiore.

Per ulteriori informazioni, vedere [Ottimizzare la configurazione della cache CDN](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching).

Il progetto AEM WKND dispone di una configurazione CDN di riferimento. Per ulteriori informazioni, vedere [Configurazione CDN](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) dal file `wknd.vhost`.
