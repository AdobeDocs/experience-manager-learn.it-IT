---
title: Protezione dei siti Web con regole per il filtro del traffico (incluse le regole WAF)
description: Scopri le regole del filtro del traffico, inclusa la sua sottocategoria di regole del firewall per applicazioni web (WAF). Come creare, distribuire e testare le regole. Inoltre, analizzare i risultati per proteggere i siti AEM.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: 87266a250eb91a82cf39c4a87e8f0119658cf4aa
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---


# Protezione dei siti Web con regole per il filtro del traffico (incluse le regole WAF)

Informazioni su **regole filtro traffico**, compresa la sottocategoria **Regole WAF (Web Application Firewall)** nell’AEM as a Cloud Service (AEMCS). Scopri come creare, distribuire e testare le regole. Inoltre, analizzare i risultati per proteggere i siti AEM.

## Panoramica

Ridurre il rischio di violazioni della sicurezza è una priorità assoluta per qualsiasi organizzazione. AEMCS offre la funzione delle regole del filtro del traffico, incluse le regole WAF, per salvaguardare siti web e applicazioni.

Le regole del filtro del traffico vengono distribuite in [CDN integrata](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) e sono valutati prima che la richiesta raggiunga l’infrastruttura dell’AEM. Con questa funzione, puoi migliorare in modo significativo la sicurezza del tuo sito web, garantendo che solo le richieste legittime siano autorizzate ad accedere all’infrastruttura AEM.

Questa esercitazione ti guida attraverso il processo di creazione, distribuzione, test e analisi dei risultati delle regole del filtro del traffico, incluse le regole WAF.

Per ulteriori informazioni sulle regole del filtro del traffico, consulta [questo articolo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en).

>[!IMPORTANT]
>
> Una sottocategoria di regole del filtro del traffico denominata &quot;regole WAF&quot; richiede una licenza WAF-DDoS Protection o Enhanced Security.

Ti invitiamo a fornire feedback o a porre domande sulle regole del filtro del traffico inviando un’e-mail **aemcs-waf-adopter@adobe.com**.

## Passaggio successivo

Scopri [come impostare](./how-to-setup.md) la funzione che ti consente di creare, distribuire e testare le regole del filtro del traffico. Informazioni sulla configurazione di **Elasticsearch, Logstash e Kibana (ELK)** impilare gli strumenti del dashboard per analizzare i risultati dei registri CDN di AEMCS.



