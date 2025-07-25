---
title: Limitazione dell’accesso
description: Scopri come limitare l’accesso bloccando richieste specifiche tramite le regole per il filtro del traffico in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '390'
ht-degree: 100%

---

# Limitazione dell’accesso

Scopri come limitare l’accesso bloccando richieste specifiche tramite le regole per il filtro del traffico in AEM as a Cloud Service.

Questo tutorial illustra come **bloccare le richieste ai percorsi interni da IP pubblici** nel servizio AEM Publish.

## Perché e quando bloccare le richieste

Il blocco del traffico consente di applicare i criteri di sicurezza organizzativi impedendo l’accesso a risorse o URL sensibili in determinate condizioni. Rispetto alla registrazione, il blocco è un’azione più rigida e deve essere utilizzato quando si è sicuri che il traffico da origini specifiche non è autorizzato o oppure è indesiderato.

Gli scenari comuni in cui il blocco è appropriato includono:

- Limitazione dell’accesso a pagine `internal` o `confidential` solo agli intervalli IP interni (ad esempio, dietro una VPN aziendale).
- Blocco del traffico da bot, scanner automatizzati o attori malevoli identificati da IP o geolocalizzazione.
- Impedimento dell’accesso a endpoint obsoleti o non protetti durante le migrazioni di staging.
- Limitazione dell’accesso agli strumenti di authoring o alle route di amministrazione nei livelli di pubblicazione.

## Prerequisiti

Prima di procedere, assicurati di aver completato la configurazione richiesta come descritto nel tutorial [Come configurare il filtro del traffico e le regole di WAF](../setup.md). Inoltre, devi avere clonato e distribuito il [progetto WKND di AEM Sites](https://github.com/adobe/aem-guides-wknd) nel tuo ambiente AEM.

## Esempio: blocco dei percorsi interni dagli IP pubblici

In questo esempio viene configurata una regola per bloccare l’accesso esterno a una pagina WKND interna, ad esempio `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, da indirizzi IP pubblici. Solo gli utenti all’interno di un intervallo IP attendibile (come una VPN aziendale) possono accedere a questa pagina.

Puoi creare una pagina interna personalizzata (ad esempio, `demo-page.html`) oppure utilizzare il [pacchetto allegato](../assets/how-to/demo-internal-pages-package.zip).

- Aggiungi la regola seguente al file `/config/cdn.yaml` del progetto WKND.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
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

- Conferma e invia le modifiche all’archivio Git di Cloud Manager.

- Implementa le modifiche nell’ambiente di sviluppo di AEM utilizzando la pipeline di configurazione di Cloud Manager [creata in precedenza](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Verifica la regola accedendo alla pagina interna del sito WKND, ad esempio `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` o utilizzando il comando cURL seguente:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Ripeti il passaggio precedente sia dall’indirizzo IP utilizzato nella regola che da un indirizzo IP diverso (ad esempio, utilizzando il telefono cellulare).

## Analisi

Per analizzare i risultati della regola `block-internal-paths`, segui gli stessi passaggi descritti nel [tutorial per la configurazione](../setup.md#cdn-logs-ingestion)

Tuttavia, questa volta dovresti visualizzare le **richieste bloccate** e i valori corrispondenti nelle colonne IP client (cli_ip), host, URL, azione (waf_action) e nome della regola (waf_match).

![Richiesta bloccata nella dashboard dello strumento ELK.](../assets/how-to/elk-tool-dashboard-blocked.png)
