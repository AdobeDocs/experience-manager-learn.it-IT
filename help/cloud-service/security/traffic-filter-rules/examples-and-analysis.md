---
title: Esempi e analisi dei risultati delle regole del filtro del traffico, incluse le regole WAF
description: Scopri diverse regole del filtro del traffico, inclusi esempi di regole WAF. Inoltre, come analizzare i risultati utilizzando i registri CDN AEM as a Cloud Service (AEMCS).
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
source-git-commit: ceb498f751ffc50d0022a16b63f9f52594bc507e
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 0%

---

# Esempi e analisi dei risultati delle regole del filtro del traffico, incluse le regole WAF

Scopri come dichiarare vari tipi di regole del filtro del traffico e analizzare i risultati utilizzando i registri CDN e gli strumenti della dashboard di Adobe Experience Manager as a Cloud Service (AEMCS).

In questa sezione verranno illustrati alcuni esempi pratici delle regole del filtro del traffico, incluse le regole WAF. Scoprirai come registrare, consentire e bloccare le richieste in base a URI (o percorso), indirizzo IP, numero di richieste e diversi tipi di attacco utilizzando [Progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

Inoltre, scoprirai come utilizzare gli strumenti della dashboard che acquisiscono i registri CDN di AEMCS per visualizzare le metriche essenziali attraverso gli esempi di dashboard forniti da Adobe.

Per allinearsi ai requisiti specifici, puoi migliorare e creare dashboard personalizzate, ottenendo informazioni più approfondite e ottimizzando le configurazioni delle regole per i siti AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## Esempi

Esaminiamo vari esempi di regole del filtro del traffico, incluse le regole WAF. Accertarsi di aver completato il processo di configurazione richiesto come descritto nella precedente [come impostare](./how-to-setup.md) e che hai clonato [Progetto AEM WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### Registrazione delle richieste

Inizia per **richieste di registrazione dei percorsi di accesso e disconnessione WKND** contro il servizio di pubblicazione AEM.

- Aggiungi la seguente regola al file del progetto WKND `/config/cdn.yaml` file.

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
    # On AEM Publish service log WKND Login and Logout requests 
      - name: publish-auth-requests
        when:
          allOf:
            - reqProperty: tier
              matches: publish
            - reqProperty: path
              in:
                - /system/sling/login/j_security_check
                - /system/sling/logout
        action: log
```

- Esegui il commit e invia le modifiche all’archivio Git di Cloud Manager.

- Distribuire le modifiche all’ambiente di sviluppo AEM tramite Cloud Manager `Dev-Config` pipeline di configurazione [creato in precedenza](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Pipeline di configurazione di Cloud Manager](./assets/cloud-manager-config-pipeline.png)

- Verifica la regola effettuando l’accesso e la disconnessione dal sito WKND del programma sul servizio di pubblicazione (ad esempio, `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). È possibile utilizzare `asmith/asmith` come nome utente e password.

  ![Accesso WKND](./assets/wknd-login.png)

#### Analisi di{#analyzing}

Analizziamo i risultati della `publish-auth-requests` scaricando i registri CDN di AEMCS da Cloud Manager e utilizzando [strumenti dashboard](how-to-setup.md#analyze-results-using-elk-dashboard-tool), configurato nel capitolo precedente.

- Da [Cloud Manager](https://my.cloudmanager.adobe.com/)di **Ambienti** , scarica AEMCS **Pubblica** dei registri CDN del servizio.

  ![Download dei registri CDN di Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    La visualizzazione delle nuove richieste nei registri CDN potrebbe richiedere fino a 5 minuti.

- Copia il file di registro scaricato (ad esempio, `publish_cdn_2023-10-24.log` nella schermata seguente) in `logs/dev` cartella del progetto dello strumento dashboard elastico.

  ![Cartella registri dello strumento ELK](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Aggiornare la pagina dello strumento Dashboard elastica.
   - In alto **Filtro globale** , modificare la sezione `aem_env_name.keyword` filtra e seleziona la `dev` valore dell’ambiente.

     ![Filtro globale dello strumento ELK](./assets/elk-tool-global-filter.png)

   - Per modificare l’intervallo di tempo, fai clic sull’icona del calendario nell’angolo in alto a destra e seleziona l’intervallo di tempo desiderato.

     ![Intervallo tempo strumento ELK](./assets/elk-tool-time-interval.png)

- Rivedi il dashboard aggiornato  **Richieste analizzate**, **Richieste con flag**, e **Dettagli delle richieste contrassegnate** pannelli. Per le voci di registro CDN corrispondenti, deve mostrare i valori dell’IP client (cli_ip), dell’host, dell’URL, dell’azione (waf_action) e del nome della regola (waf_match) di ciascuna voce.

  ![Dashboard dello strumento ELK](./assets/elk-tool-dashboard.png)


### Blocco delle richieste

In questo esempio, aggiungiamo una pagina in una _interno_ cartella nel percorso `/content/wknd/internal` nel progetto WKND implementato. Dichiara quindi una regola del filtro del traffico che **blocca il traffico** alle pagine secondarie da qualsiasi luogo diverso da un indirizzo IP specificato corrispondente all’organizzazione (ad esempio, una VPN aziendale).

Puoi creare una pagina interna personalizzata (ad esempio, `demo-page.html`) o utilizzare il [pacchetto allegato](./assets/demo-internal-pages-package.zip).

- Aggiungi la seguente regola nel file del progetto WKND `/config/cdn.yaml` file:

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

    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
      - name: block-internal-paths
        when:
          allOf:
            - reqProperty: path
              matches: /content/wknd/internal
            - reqProperty: clientIp
              notIn: [192.150.10.0/24]
        action: block
```

- Esegui il commit e invia le modifiche all’archivio Git di Cloud Manager.

- Implementare le modifiche all’ambiente di sviluppo AEM utilizzando [creato in precedenza](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` pipeline di configurazione in Cloud Manager.

- Verifica la regola accedendo alla pagina interna del sito WKND, ad esempio `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` o utilizzando il comando CURL riportato di seguito:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Ripeti il passaggio precedente sia dall’indirizzo IP utilizzato nella regola che da un indirizzo IP diverso (ad esempio, utilizzando il telefono cellulare).

#### Analisi di

Per analizzare i risultati del `block-internal-paths` regola, segui gli stessi passaggi descritti in [esempio precedente](#analyzing).

Tuttavia, questa volta dovresti vedere **Richieste bloccate** e i valori corrispondenti nelle colonne IP client (cli_ip), host, URL, azione (waf_action) e nome regola (waf_match).

![Richiesta blocco dashboard dello strumento ELK](./assets/elk-tool-dashboard-blocked.png)


### Previeni gli attacchi DoS

Facciamo **impedire attacchi DoS** bloccando le richieste provenienti da un indirizzo IP che effettua 100 richieste al secondo, causandone il blocco per 5 minuti.

- Aggiungi quanto segue [regola filtro traffico limite di frequenza](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) nel progetto WKND di `/config/cdn.yaml` file.

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
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block     
```

>[!WARNING]
>
>Per l’ambiente di produzione, collabora con il team di sicurezza web per determinare i valori appropriati per `rateLimit`,

- Eseguire il commit, inviare e distribuire le modifiche come indicato nella [esempi precedenti](#logging-requests).

- Per simulare l&#39;attacco DoS, utilizzare quanto segue [Vegeta](https://github.com/tsenart/vegeta) comando.

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  Questo comando effettua 120 richieste per 5 secondi ed restituisce un rapporto. Come puoi vedere, il tasso di successo è del 32,5%; per il resto viene ricevuto un codice di risposta HTTP 406, a dimostrazione che il traffico era bloccato.

  ![Attacco Vegeta DoS](./assets/vegeta-dos-attack.png)

#### Analisi di

Per analizzare i risultati del `prevent-dos-attacks` regola, segui gli stessi passaggi descritti in [esempio precedente](#analyzing).

Questa volta dovresti vederne molti **Richieste bloccate** e i valori corrispondenti nelle colonne client IP (cli_ip), host, url, action (waf_action) e rule-name (waf_match).

![Richiesta DoS dashboard strumento ELK](./assets/elk-tool-dashboard-dos.png)

Inoltre, il **Primi 100 attacchi per IP client, paese e agente utente** I pannelli mostrano ulteriori dettagli che possono essere utilizzati per ottimizzare ulteriormente la configurazione delle regole.

![Dashboard ELK Tool: 100 richieste DoS principali](./assets/elk-tool-dashboard-dos-top-100.png)

### Regole WAF

Gli esempi di regole del filtro del traffico finora possono essere configurati da tutti i clienti Sites e Forms.

Esaminiamo ora l’esperienza di un cliente che ha ottenuto una licenza di sicurezza avanzata o protezione WAF-DDoS, che consente di configurare regole avanzate per proteggere i siti web da attacchi più sofisticati.

Prima di continuare, abilitare la protezione WAF-DDoS per il programma, come descritto nella documentazione relativa alle regole del filtro del traffico [passaggi di configurazione](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup).

#### Senza WAFFlags

Vediamo innanzitutto l’esperienza anche prima che vengano dichiarate le regole WAF. Quando il WAF-DDoS è abilitato nel programma, i registri CDN per impostazione predefinita registrano qualsiasi corrispondenza di traffico dannoso, in modo da avere le informazioni giuste per trovare le regole appropriate.

Iniziamo attaccando il sito WKND senza aggiungere una regola WAF (o utilizzando `wafFlags` proprietà ) e analizzare i risultati.

- Per simulare un attacco, utilizzare [Nikto](https://github.com/sullo/nikto) comando sottostante, che invia circa 700 richieste dannose in 6 minuti.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Simulazione attacco Nikto](./assets/nikto-attack.png)

  Per informazioni sulla simulazione di un attacco, vedere [Nikto - Sintonizzazione scansione](https://github.com/sullo/nikto/wiki/Scan-Tuning) che indica come specificare il tipo di attacchi di test da includere o escludere.

##### Analisi di

Per analizzare i risultati della simulazione dell&#39;attacco, seguire gli stessi passaggi descritti nella [esempio precedente](#analyzing).

Tuttavia, questa volta dovresti vedere **Richieste con flag** e i valori corrispondenti nelle colonne client IP (cli_ip), host, url, action (waf_action) e rule-name (waf_match). Queste informazioni ti consentono di analizzare i risultati e ottimizzare la configurazione della regola.

![Richiesta contrassegnata WAF dashboard dashboard strumento ELK](./assets/elk-tool-dashboard-waf-flagged.png)

Nota come **Distribuzione flag WAF** e **Attacchi principali** i pannelli mostrano ulteriori dettagli, che possono essere utilizzati per ottimizzare ulteriormente la configurazione della regola.

![Richiesta flag attacchi WAF dashboard strumenti ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![Richiesta attacchi principali WAF dashboard strumento ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Con WAFFlags

Aggiungiamo ora una regola WAF che contiene `wafFlags` proprietà come parte del `action` proprietà e **bloccare le richieste di attacco simulato**.

Dal punto di vista della sintassi, le regole WAF sono simili a quelle viste in precedenza, tuttavia, le `action` la proprietà fa riferimento a uno o più `wafFlags` valori. Per ulteriori informazioni su `wafFlags`, rivedi [Elenco flag WAF](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list) sezione.

- Aggiungi la seguente regola nel file del progetto WKND `/config/cdn.yaml` file. Nota come `block-waf-flags` La regola include alcuni dei wafFlags visualizzati nella dashboard quando vengono attaccati con traffico dannoso simulato. In effetti, è buona prassi nel tempo analizzare i registri per determinare quali nuove regole dichiarare, man mano che il panorama delle minacce si evolve.

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
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - SIGSCI-IP
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8        
```

- Eseguire il commit, inviare e distribuire le modifiche come indicato nella [esempi precedenti](#logging-requests).

- Per simulare un attacco, utilizza lo stesso [Nikto](https://github.com/sullo/nikto) come prima.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Analisi di

Ripeti gli stessi passaggi descritti in [esempio precedente](#analyzing).

Questa volta dovresti visualizzare le voci in **Richieste bloccate** e i valori corrispondenti nelle colonne client IP (cli_ip), host, url, action (waf_action) e rule-name (waf_match).

![Richiesta blocco WAF dashboard dashboard strumenti ELK](./assets/elk-tool-dashboard-waf-blocked.png)

Inoltre, il **Distribuzione flag WAF** e **Attacchi principali** mostra ulteriori dettagli.

![Richiesta flag attacchi WAF dashboard strumenti ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![Richiesta attacchi principali WAF dashboard strumento ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Analisi completa

In quanto sopra _analisi_ sezioni, hai imparato ad analizzare i risultati di regole specifiche utilizzando lo strumento del dashboard. Puoi esplorare ulteriormente l’analisi dei risultati utilizzando altri pannelli della dashboard, tra cui:


- Richieste analizzate, contrassegnate e bloccate
- Distribuzione dei flag WAF nel tempo
- Attivazione delle regole del filtro del traffico nel tempo
- Primi attacchi per ID flag WAF
- Filtro del traffico attivato più alto
- Primi 100 autori di attacchi per IP client, paese e agente utente

![Analisi completa del dashboard dello strumento ELK](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![Analisi completa del dashboard dello strumento ELK](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## Passaggio successivo

Acquisisci familiarità con gli elementi consigliati [best practice](./best-practices.md) ridurre il rischio di violazioni della sicurezza.

## Risorse aggiuntive

[Sintassi delle regole del filtro del traffico](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[Formato registro CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)
