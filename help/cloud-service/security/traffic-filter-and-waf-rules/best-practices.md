---
title: Best practice per le regole del filtro del traffico che includono regole WAF
description: Scopri le best practice consigliate per la configurazione delle regole per il filtro del traffico, incluse le regole di WAF in AEM as a Cloud Service, per migliorare la sicurezza e ridurre i rischi.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 100%

---

# Best practice per le regole del filtro del traffico che includono regole WAF

Scopri le best practice consigliate per la configurazione delle regole per il filtro del traffico, incluse le regole di WAF in AEM as a Cloud Service, per migliorare la sicurezza e ridurre i rischi.

>[!IMPORTANT]
>
>Le best practice descritte in questo articolo non sono esaustive e non intendono sostituirsi ai criteri e alle procedure di sicurezza aziendali.

## Best practice generali

- Inizia con il [set consigliato](./overview.md#adobe-recommended-rules) di regole standard per il filtro del traffico e WAF fornite da Adobe e regolale in base alle esigenze specifiche dell’applicazione e al panorama delle minacce.
- Collabora con il tuo team di sicurezza per determinare quali regole sono in linea con il livello di sicurezza e i requisiti di conformità della tua organizzazione.
- Prima di promuoverle nell’ambiente di staging e produzione, testa sempre le regole nuove o aggiornate negli ambienti di sviluppo.
- Durante la dichiarazione e la convalida delle regole, inizia con il tipo `action` `log` per osservare il comportamento senza bloccare il traffico legittimo.
- Spostati da `log` a `block` solo dopo aver analizzato dati sul traffico sufficienti e aver confermato che non vi sono richieste valide interessate.
- Introduci regole in modo incrementale, coinvolgendo i team di test di controllo qualità, prestazioni e sicurezza per identificare gli effetti collaterali indesiderati.
- Rivedi e analizza regolarmente l’efficacia delle regole utilizzando lo [strumento della dashboard](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). La frequenza di revisione (giornaliera, settimanale, mensile) deve essere allineata con il volume di traffico del sito e il profilo di rischio.
- Perfeziona in modo costante le regole in base all’intelligence relativa alle nuove minacce, comportamento del traffico e risultati di audit.

## Best practice per le regole del filtro del traffico

- Utilizza le [regole standard per il filtro del traffico consigliate da Adobe](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) come linea di base, che includono regole per Edge, protezione dell’origine e restrizioni basate su OFAC.
- Rivedi regolarmente gli avvisi e i registri per identificare pattern di abuso o configurazioni errate.
- Regola i valori di soglia per i limiti di frequenza in base ai pattern di traffico dell’applicazione e al comportamento degli utenti.

  Per istruzioni su come scegliere i valori di soglia, consulta la tabella seguente:

  | Variante | Valore |
  | :--------- | :------- |
  | Origine | Considera il valore più alto del numero massimo di richieste di origine per IP/POP in condizioni di traffico **normali** (ovvero non la frequenza al momento di un DDoS) e aumentalo di un multiplo |
  | Edge | Considera il valore più alto del numero massimo di richieste Edge per IP/POP in condizioni di traffico **normali** (ovvero non la frequenza al momento di un DDoS) e aumentalo di un multiplo |

  Consulta anche la sezione [Scelta dei valori di soglia](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) per ulteriori dettagli.

- Spostati sull’azione `block` solo dopo aver verificato che l’azione `log` non influisca sul traffico legittimo.

## Best practice per le regole WAF

- Inizia con le [regole di WAF consigliate](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) da Adobe, che includono regole per bloccare gli IP danneggiati noti, rilevano gli attacchi DDoS e mitigano l’abuso di bot.
- Il flag WAF `ATTACK` dovrebbe notificare potenziali minacce. Assicurati che non siano presenti falsi positivi prima di passare a `block`.
- Se le regole WAF consigliate non coprono minacce specifiche, è consigliabile creare regole personalizzate in base ai requisiti univoci dell’applicazione. Consulta l’elenco completo dei [flag WAF](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) nella documentazione.

## Implementare le regole

Scopri come implementare le regole per il filtro del traffico e le regole WAF in AEM as a Cloud Service:

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico">Protezione dei siti Web di AEM tramite le regole standard per il filtro del traffico</a>
                    </p>
                    <p class="is-size-6">Scopri come proteggere i siti web di AEM dall’abuso di DoS, DDoS e bot utilizzando le regole standard per il filtro del traffico consigliate da Adobe in AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Applica le regole</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="Protezione dei siti web di AEM tramite le regole di WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Protezione dei siti web di AEM tramite le regole di WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protezione dei siti web di AEM tramite le regole di WAF">Protezione dei siti web di AEM tramite le regole di WAF</a>
                    </p>
                    <p class="is-size-6">Scopri come proteggere i siti web di AEM da minacce sofisticate, tra cui DoS, DDoS e abusi di bot utilizzando le regole del firewall per l’applicazione web (WAF) consigliate da Adobe in AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Attiva WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Risorse aggiuntive

- [Regole per il filtro del traffico che includono le regole WAF](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Informazioni sulla prevenzione da DoS/DDoS in AEM](https://experienceleague.adobe.com/it/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Bloccare gli attacchi DoS e DDoS utilizzando le regole per il filtro del traffico](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)
