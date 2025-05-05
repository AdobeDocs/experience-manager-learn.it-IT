---
title: Informazioni sulla prevenzione DoS/DoS
description: Scopri come prevenire e attenuare gli attacchi DoS e DDoS contro AEM.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# Informazioni sulla prevenzione DoS/DDoS in AEM

Scopri le opzioni disponibili per prevenire e attenuare gli attacchi DoS e DDoS nell’ambiente AEM. Prima di immergerti nei meccanismi di prevenzione, una breve panoramica di [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) e [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- Gli attacchi DoS (Denial of Service) e DDoS (Distributed Denial of Service) sono entrambi tentativi dannosi di interrompere il normale funzionamento di un server, servizio o rete mirati, rendendolo inaccessibile agli utenti a cui è destinato.
- Gli attacchi DoS in genere provengono da un&#39;unica sorgente, mentre gli attacchi DoS provengono da più sorgenti.
- Gli attacchi DDoS sono spesso di dimensioni maggiori rispetto agli attacchi DoS a causa delle risorse combinate di più dispositivi di attacco.
- Questi attacchi vengono effettuati inondando il bersaglio con traffico eccessivo e sfruttando le vulnerabilità nei protocolli di rete.

La tabella seguente descrive come prevenire e attenuare gli attacchi DoS e DDoS:

<table>
    <tbody>
        <tr>
            <td><strong>Meccanismo di prevenzione</strong></td>
            <td><strong>Descrizione</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5 (on-premise)</strong></td>
        </tr>
        <tr>
            <td>Firewall applicazione Web (WAF)</td>
            <td>Una soluzione di sicurezza progettata per proteggere le applicazioni web da vari tipi di attacchi.</td>
            <td>
            <a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">Licenza di protezione WAF-DDoS</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> o <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF tramite contratto AMS.</td>
            <td>Il WAF preferito</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity (o "mod_security", modulo Apache) è una soluzione open-source e multipiattaforma che fornisce protezione da una serie di attacchi contro le applicazioni web.<br/> In AEM as a Cloud Service, questo è applicabile solo al servizio AEM Publish, in quanto non vi sono server web Apache e AEM Dispatcher davanti al servizio AEM Author.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">Abilita ModSecurity </a></td>
        </tr>
        <tr>
            <td>Regole filtro traffico</td>
            <td>Le regole del filtro del traffico possono essere utilizzate per bloccare o consentire le richieste a livello CDN.</td>
            <td><a href="https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Esempio di regole del filtro del traffico</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">Funzionalità di limitazione delle regole di AWS</a> o <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a>.</td>
            <td>La soluzione preferita</td>
        </tr>
    </tbody>
</table>

## Analisi post-incidente e miglioramento continuo

Anche se non esiste un flusso standard universale per identificare e prevenire gli attacchi DoS/DDoS e dipende dal processo di sicurezza della tua organizzazione. L&#39;analisi **post-incidente e il miglioramento continuo** è un passaggio cruciale del processo. Di seguito sono riportate alcune best practice da considerare:

- Identificare la causa principale dell&#39;attacco DoS/DDoS effettuando un&#39;analisi post-incidente, inclusi la revisione dei registri, del traffico di rete e delle configurazioni di sistema.
- Migliorare i meccanismi di prevenzione sulla base dei risultati dell&#39;analisi post-incidente.

