---
title: Protezione dei siti web con regole di filtro del traffico (comprese le regole WAF)
description: Scopri le regole del filtro del traffico, inclusa la sua sottocategoria di regole del firewall per applicazioni web (WAF). Come creare, distribuire e testare le regole. Inoltre, analizza i risultati per proteggere i siti AEM.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 165
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 13%

---

# Protezione dei siti web con regole di filtro del traffico (comprese le regole WAF)

Scopri le **regole del filtro del traffico**, inclusa la sottocategoria di **regole del firewall dell&#39;applicazione Web (WAF)** in AEM as a Cloud Service (AEMCS). Scopri come creare, distribuire e testare le regole. Inoltre, analizza i risultati per proteggere i siti AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Panoramica

Ridurre il rischio di violazioni della sicurezza è una priorità assoluta per qualsiasi organizzazione. AEMCS offre la funzione delle regole del filtro del traffico, incluse le regole di WAF, per proteggere siti web e applicazioni.

Le regole del filtro del traffico vengono distribuite alla [rete CDN integrata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) e vengono valutate prima che la richiesta raggiunga l&#39;infrastruttura AEM. Con questa funzione, puoi migliorare in modo significativo la sicurezza del sito web, garantendo che solo le richieste legittime siano autorizzate ad accedere all’infrastruttura AEM.

Questa esercitazione ti guida attraverso il processo di creazione, distribuzione, test e analisi dei risultati delle regole del filtro del traffico, incluse le regole di WAF.

Ulteriori informazioni sulle regole del filtro del traffico sono disponibili in [questo articolo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en).

>[!IMPORTANT]
>
> Una sottocategoria di regole del filtro del traffico denominata &quot;regole WAF&quot; richiede una licenza di protezione WAF-DDoS o di protezione avanzata.

Ti invitiamo a fornire un feedback o a porre domande sulle regole del filtro del traffico inviando un’e-mail all’indirizzo **aemcs-waf-adopter@adobe.com**.

## Passaggio successivo

Scopri [come configurare](./how-to-setup.md) la funzionalità in modo da creare, distribuire e testare le regole del filtro del traffico. Scopri come configurare gli strumenti della dashboard dello stack **Elasticsearch, Logstash e Kibana (ELK)** per analizzare i risultati dei registri CDN di AEMCS.


