---
title: Best practice per le regole del filtro del traffico, incluse le regole di WAF
description: Scopri le best practice consigliate per le regole del filtro del traffico, incluse le regole di WAF.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 170
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# Best practice per le regole del filtro del traffico, incluse le regole di WAF

Scopri le best practice consigliate per le regole del filtro del traffico, incluse le regole di WAF. È importante notare che le best practice descritte in questo articolo non sono esaustive e non intendono sostituirsi alle politiche e alle procedure di sicurezza aziendali.

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## Best practice generali

- Per determinare quali regole sono appropriate per la tua organizzazione, collabora con il team di sicurezza.
- Esegui sempre il test delle regole negli ambienti di sviluppo prima di distribuirle negli ambienti di staging e produzione.
- Quando si dichiarano e si convalidano le regole, iniziare sempre con `action` tipo `log` per assicurarsi che la regola non blocchi il traffico legittimo.
- Per alcune regole, la transizione da `log` a `block` deve essere puramente basata sull&#39;analisi di un traffico del sito sufficiente.
- Introduci le regole in modo incrementale e prendi in considerazione la possibilità di coinvolgere i team di test (QA, prestazioni, test di penetrazione) nel processo.
- Analizza l&#39;impatto delle regole regolarmente utilizzando gli strumenti del [dashboard](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). A seconda del volume di traffico del sito, l’analisi può essere eseguita su base giornaliera, settimanale o mensile.
- Per bloccare il traffico dannoso di cui potresti essere a conoscenza dopo l’analisi, aggiungi eventuali regole aggiuntive. Ad esempio, alcuni IP che hanno attaccato il tuo sito.
- La creazione, la distribuzione e l’analisi delle regole devono essere un processo continuo e iterativo. Non si tratta di un&#39;attività una tantum.

## Best practice per le regole del filtro del traffico

Abilita le regole del filtro del traffico riportate di seguito per il progetto AEM. Tuttavia, i valori desiderati per le proprietà `rateLimit` e `clientCountry` devono essere determinati in collaborazione con il team di sicurezza.

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
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
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
```

>[!WARNING]
>
>Per l&#39;ambiente di produzione, collaborare con il team di sicurezza Web per determinare i valori appropriati per `rateLimit`

## Best practice per le regole di WAF

Una volta che WAF è concesso in licenza e abilitato per il programma, i flag di corrispondenza del traffico di WAF vengono visualizzati nei grafici e nei registri delle richieste, anche se non sono stati dichiarati in una regola. Quindi, sei sempre a conoscenza di traffico potenzialmente nuovo dannoso e può creare regole in base alle esigenze. Osserva i flag di WAF che non si riflettono nelle regole dichiarate e prendi in considerazione la loro dichiarazione.

Considera le regole di WAF riportate di seguito per il progetto AEM. Tuttavia, i valori desiderati per la proprietà `action` e `wafFlags` devono essere determinati in collaborazione con il team di sicurezza.

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

    # Traffic Filter rules shown in above section
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
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## Riepilogo

In conclusione, questo tutorial ti ha fornito le conoscenze e gli strumenti necessari per rafforzare la sicurezza delle applicazioni web in Adobe Experience Manager as a Cloud Service (AEMCS). Con esempi pratici di regole e informazioni approfondite sull’analisi dei risultati, puoi proteggere efficacemente il tuo sito web e le tue applicazioni.



