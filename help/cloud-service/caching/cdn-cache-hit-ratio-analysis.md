---
title: Analisi del rapporto di hit della cache CDN
description: Scopri come analizzare i registri CDN forniti da AEM as a Cloud Service. Ottieni informazioni approfondite come il rapporto hit della cache e i principali URL di tipi di cache MISS e PASS a scopo di ottimizzazione.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
exl-id: 43aa7133-7f4a-445a-9220-1d78bb913942
duration: 276
source-git-commit: 4111ae0cf8777ce21c224991b8b1c66fb01041b3
workflow-type: tm+mt
source-wordcount: '1476'
ht-degree: 0%

---

# Analisi del rapporto di hit della cache CDN

Il contenuto memorizzato nella cache della rete CDN riduce la latenza per gli utenti del sito web che non devono attendere che la richiesta torni ad Apache/Dispatcher o che venga pubblicata l’AEM. Tenendo presente questo aspetto, vale la pena ottimizzare il rapporto di hit della cache CDN per massimizzare la quantità di contenuto memorizzabile in cache sulla CDN.

Scopri come analizzare gli as a Cloud Service AEM forniti **Registri CDN** e ottenere informazioni quali **percentuale di riscontri nella cache**, e **URL principali di _SIGNORINA_ e _PASS_ tipi di cache**, a scopo di ottimizzazione.


I registri CDN sono disponibili in formato JSON, che contiene vari campi tra cui `url`, `cache`. Per ulteriori informazioni, vedere [Formato registro CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=en#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). Il `cache` fornisce informazioni su _stato della cache_ e i suoi valori possibili sono HIT, MISS o PASS. Esaminiamo i dettagli dei valori possibili.

| Stato della cache </br> Valore possibile | Descrizione |
|------------------------------------|:-----------------------------------------------------:|
| HIT | I dati richiesti sono _trovato nella cache CDN e non richiede di effettuare un recupero_ richiesta al server AEM. |
| SIGNORINA | I dati richiesti sono _non trovato nella cache CDN e deve essere richiesto_ dal server AEM. |
| PASS | I dati richiesti sono _impostato in modo esplicito per non essere memorizzato in cache_ e devono essere sempre recuperati dal server AEM. |

Ai fini di questa esercitazione, il [Progetto WKND AEM](https://github.com/adobe/aem-guides-wknd) viene implementato nell’ambiente as a Cloud Service dell’AEM e viene attivato un piccolo test delle prestazioni utilizzando [Apache JMeter](https://jmeter.apache.org/).

Questo tutorial è strutturato in modo da illustrare il processo seguente:

1. Download dei registri CDN tramite Cloud Manager
1. L’analisi di questi registri CDN può essere eseguita con due approcci: un dashboard installato localmente o un notebook Splunk o Jupityer accessibile in remoto (per chi ha una licenza di Adobe Experience Platform)
1. Ottimizzazione della configurazione della cache CDN

## Scarica registri CDN

Per scaricare i registri CDN, effettua le seguenti operazioni:

1. Accedi a Cloud Manager all’indirizzo [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) e seleziona la tua organizzazione e il tuo programma.

1. Per un ambiente AEMCS desiderato, seleziona **Scarica registri** dal menu con i puntini di sospensione.

   ![Scaricare i registri - Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. In **Scarica registri** , seleziona la **Pubblica** Servizio dal menu a discesa, quindi fai clic sull&#39;icona di download accanto al **CDN** riga.

   ![Registri CDN - Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


Se il file di registro scaricato proviene da _oggi_ l&#39;estensione del file è `.log` in caso contrario, per i file di registro passati l’estensione è `.log.gz`.

## Analizzare i registri CDN scaricati

Per ottenere informazioni approfondite, ad esempio il rapporto hit della cache e i principali URL dei tipi di cache di tipo MISS e PASS, analizza il file di registro CDN scaricato. Queste informazioni aiutano a ottimizzare [Configurazione cache CDN](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching) e migliorare le prestazioni del sito.

Per analizzare i registri CDN, questa esercitazione presenta tre opzioni:

1. **Elasticsearch, Logstash e Kibana (ELK)**: Il [Strumenti per dashboard ELK](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) può essere installato localmente.
1. **Splunk**: Il [Strumenti dashboard Splunk](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md) richiede l’accesso a Splunk e [Inoltro registro AEMCS abilitato](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) per acquisire i registri CDN.
1. **Jupyter Notebook**: è accessibile da remoto tramite [Adobe Experience Platform](https://experienceleague.adobe.com/en/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data) senza installare software aggiuntivo, per i clienti che hanno concesso in licenza Adobe Experience Platform.

### Opzione 1: utilizzo degli strumenti del dashboard ELK

Il [Stack ELK](https://www.elastic.co/elastic-stack) è un insieme di strumenti che forniscono una soluzione scalabile per cercare, analizzare e visualizzare i dati. È costituito da Elasticsearch, Logstash e Kibana.

Per identificare i dettagli chiave, utilizziamo [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) progetto. Questo progetto fornisce un contenitore Docker dello stack ELK e un dashboard Kibana preconfigurato per analizzare i registri CDN.

1. Segui i passaggi da [Come impostare il contenitore ELK Docker](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container) e assicurati di importare **Percentuale riscontri cache CDN** Dashboard Kibana.

1. Per identificare la percentuale di riscontri nella cache CDN e i primi URL, effettua le seguenti operazioni:

   1. Copia i file di registro CDN scaricati all’interno della cartella dei registri specifica dell’ambiente, ad esempio, `ELK/logs/stage`.

   1. Apri **Percentuale riscontri cache CDN** dashboard facendo clic sull’angolo superiore sinistro _Menu di navigazione > Analytics > Dashboard > Rapporto riscontri cache CDN_.

      ![Percentuale riscontri cache CDN - Dashboard Kibana](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. Seleziona l’intervallo di tempo desiderato dall’angolo in alto a destra.

      ![Intervallo di tempo - Dashboard Kibana](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. Il **Percentuale riscontri cache CDN** la dashboard non richiede spiegazioni.

   1. Il _Analisi richiesta totale_ mostra i dettagli seguenti:
      - Rapporti cache per tipo di cache
      - Conteggi cache per tipo di cache

      ![Analisi delle richieste - Dashboard Kibana - Totale](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. Il _Analisi per tipo di richiesta o MIME_ visualizza i dettagli seguenti:
      - Rapporti cache per tipo di cache
      - Conteggi cache per tipo di cache
      - Primi URL mancanti e passati

      ![Analisi per tipo di richiesta o MIME - Dashboard Kibana](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### Filtraggio per nome di ambiente o ID programma

Per filtrare i registri acquisiti per nome dell’ambiente, segui i passaggi seguenti:

1. Nel dashboard Rapporto riscontri cache CDN, fai clic su **Aggiungi filtro** icona.

   ![Filtro - Dashboard Kibana](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. In **Aggiungi filtro** modale, seleziona la `aem_env_name.keyword` dal menu a discesa e `is` e il nome dell’ambiente desiderato per il campo successivo, infine fai clic su _Aggiungi filtro_.

   ![Aggiungi filtro - Dashboard Kibana](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### Filtraggio per nome host

Per filtrare i registri acquisiti per nome host, segui i passaggi seguenti:

1. Nel dashboard Rapporto riscontri cache CDN, fai clic su **Aggiungi filtro** icona.

   ![Filtro - Dashboard Kibana](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. In **Aggiungi filtro** modale, seleziona la `host.keyword` dal menu a discesa e `is` e il nome host desiderato per il campo successivo, infine fai clic su _Aggiungi filtro_.

   ![Filtro host - Dashboard Kibana](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

Allo stesso modo, aggiungi altri filtri al dashboard in base ai requisiti di analisi.

### Opzione 2: utilizzo degli strumenti del dashboard Splunk

Il [Splunk](https://www.splunk.com/) è un popolare strumento di analisi dei registri che consente di aggregare, analizzare i registri e creare visualizzazioni a scopo di monitoraggio e risoluzione dei problemi.

Per identificare i dettagli chiave, utilizziamo [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) progetto. Questo progetto fornisce un dashboard Splunk per analizzare i registri CDN.

1. Segui i passaggi da [Dashboard Splunk per l’analisi del registro CDN di AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md) e assicurati di importare **Percentuale riscontri cache CDN** Dashboard Splunk.
1. Se necessario, aggiorna il _Indice, tipo di origine e altro_ filtrare i valori nel dashboard Splunk.

   ![Dashboard Splunk](assets/cdn-logs-analysis/splunk-CHR-dashboard.png){width="500" zoomable="yes"}

>[!NOTE]
>
>L’interfaccia utente e i grafici nel dashboard di splunk sono diversi dal dashboard ELK, tuttavia, i dettagli delle chiavi sono simili.

### Opzione 3: utilizzo di Jupyter Notebook

Per coloro che preferiscono non installare il software localmente (ovvero, gli strumenti del dashboard ELK della sezione precedente), esiste un&#39;altra opzione, ma richiede una licenza per Adobe Experience Platform.

Il [Jupyter Notebook](https://jupyter.org/) è un’applicazione web open-source che consente di creare documenti contenenti codice, testo e visualizzazione. Viene utilizzato per la trasformazione dei dati, la visualizzazione e la modellazione statistica. È possibile accedervi da remoto [come parte di Adobe Experience Platform](https://experienceleague.adobe.com/en/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data).

#### Download del file del blocco appunti Python interattivo

Scarica innanzitutto il file [AEM-as-a-CloudService - Analisi dei registri CDN - Notebook Jupyter](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb) , che facilita l’analisi dei registri CDN. Questo file &quot;Interactive Python Notebook&quot; è auto-esplicativo, tuttavia, gli elementi di rilievo di ogni sezione sono:

- **Installare librerie aggiuntive**: installa il `termcolor` e `tabulate` Librerie Python.
- **Carica registri CDN**: carica il file di registro CDN utilizzando `log_file` valore della variabile; assicurati di aggiornarne il valore. Trasforma anche questo accesso CDN in [DataFrame Pandas](https://pandas.pydata.org/docs/reference/frame.html).
- **Eseguire analisi**: il primo blocco di codice è _Visualizzazione dei risultati dell’analisi per richieste totali, HTML, JS/CSS e immagini_; fornisce i grafici percentuale di hit della cache, a barre e a torta.
Il secondo blocco di codice è _Primi 5 URL di richieste MISS e PASS per HTML, JS/CSS e Image_; visualizza gli URL e i relativi conteggi in formato tabella.

#### Esecuzione del blocco appunti Jupyter

Quindi, esegui Jupyter Notebook in Adobe Experience Platform, seguendo questi passaggi:

1. Accedi a [Adobe Experience Cloud](https://experience.adobe.com/), nella home page > **Accesso rapido** sezione > fai clic su **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. Nella home page di Adobe Experience Platform > sezione Data Science > , fai clic sul pulsante **Notebook** voce di menu. Per avviare l’ambiente Jupyter Notebooks, fai clic su **JupyterLab** scheda.

   ![Aggiornamento del valore del file di registro del notebook](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. Nel menu JupyterLab, utilizzando **Carica file** , carica il file di registro CDN scaricato e `aemcs_cdn_logs_analysis.ipynb` file.

   ![Carica file - JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. Apri `aemcs_cdn_logs_analysis.ipynb` facendo doppio clic.

1. In **Carica file di registro CDN** del blocco appunti, aggiornare `log_file` valore.

   ![Aggiornamento del valore del file di registro del notebook](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. Per eseguire la cella selezionata e avanzare, fare clic sul pulsante **Play** icona.

   ![Aggiornamento del valore del file di registro del notebook](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. Dopo aver eseguito **Visualizzazione dei risultati dell’analisi per richieste totali, HTML, JS/CSS e immagini** cella del codice, l’output mostra i grafici percentuale hit della cache, a barre e a torta.

   ![Aggiornamento del valore del file di registro del notebook](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. Dopo aver eseguito **Primi 5 URL di richieste MISS e PASS per HTML, JS/CSS e Image** , nell&#39;output vengono visualizzati i primi 5 URL di richiesta MISS e PASS.

   ![Aggiornamento del valore del file di registro del notebook](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

Puoi migliorare Jupyter Notebook per analizzare i registri CDN in base alle tue esigenze.

## Ottimizzazione della configurazione della cache CDN

Dopo aver analizzato i registri CDN, puoi ottimizzare la configurazione della cache CDN per migliorare le prestazioni del sito. La best practice per l’AEM prevede un rapporto di hit della cache pari o superiore al 90%.

Per ulteriori informazioni, consulta [Ottimizza configurazione cache CDN](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching).

Il progetto WKND dell’AEM ha una configurazione CDN di riferimento. Per ulteriori informazioni, consulta [Configurazione CDN](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) dal `wknd.vhost` file.
