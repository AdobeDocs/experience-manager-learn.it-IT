---
title: Normalizzare le richieste
description: Scopri come normalizzare le richieste trasformandole utilizzando le regole per il filtro del traffico in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '259'
ht-degree: 100%

---

# Normalizzare le richieste

Scopri come normalizzare le richieste trasformandole utilizzando le regole per il filtro del traffico in AEM as a Cloud Service.

## Perché e quando trasformare le richieste

Le trasformazioni delle richieste sono utili quando si desidera normalizzare il traffico in entrata e ridurre le varianze inutili causate da parametri di query o intestazioni non necessari. Questa tecnica viene comunemente utilizzata per:

- migliorare l’efficienza della memorizzazione nella cache eliminando i parametri non rilevanti per l’applicazione AEM.
- proteggere l’origine da abusi riducendo al minimo le permutazioni delle richieste e l’elaborazione non necessaria.
- ottimizzare o semplificare le richieste prima che vengano inoltrate ad AEM.

Queste trasformazioni vengono generalmente applicate a livello CDN, in particolare per i livelli di AEM Publish che gestiscono il traffico pubblico.

## Prerequisiti

Prima di procedere, assicurati di aver completato la configurazione richiesta come descritto nel tutorial [Come configurare il filtro del traffico e le regole di WAF](../setup.md). Inoltre, devi avere clonato e distribuito il [progetto WKND di AEM Sites](https://github.com/adobe/aem-guides-wknd) nel tuo ambiente AEM.

## Esempio: parametri di query non impostati non necessari all’applicazione

In questo esempio viene configurata una regola che **rimuove tutti i parametri di query eccetto** `search` e `campaignId` per ridurre la frammentazione della cache.

- Aggiungi la regola seguente al file `/config/cdn.yaml` del progetto WKND.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- Conferma e invia le modifiche all’archivio Git di Cloud Manager.

- Implementa le modifiche nell’ambiente AEM utilizzando la pipeline di configurazione di Cloud Manager [creata in precedenza](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Testa la regola accedendo alla pagina del sito WKND, ad esempio `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`.

- Nei registri di AEM (`aemrequest.log`), dovresti notare che la richiesta è stata trasformata in `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`, con `otherParam` rimosso.

  ![Trasformazione richiesta WKND](../assets/how-to/aemrequest-log-transformation.png)

