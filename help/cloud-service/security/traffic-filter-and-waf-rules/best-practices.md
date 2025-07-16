---
title: Best practice per le regole del filtro del traffico, incluse le regole WAF
description: Scopri le best practice consigliate per la configurazione delle regole del filtro del traffico, incluse le regole di WAF in AEM as a Cloud Service, per migliorare la sicurezza e ridurre i rischi.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# Best practice per le regole del filtro del traffico, incluse le regole WAF

Scopri le best practice consigliate per la configurazione delle regole del filtro del traffico, incluse le regole di WAF in AEM as a Cloud Service, per migliorare la sicurezza e ridurre i rischi.

>[!IMPORTANT]
>
>Le best practice descritte in questo articolo non sono esaustive e non intendono sostituirsi alle politiche e alle procedure di sicurezza dell&#39;utente.

## Best practice generali

- Inizia con il [set consigliato](./overview.md#adobe-recommended-rules) di regole standard per il filtro del traffico e WAF fornite da Adobe e regolale in base alle esigenze specifiche dell&#39;applicazione e al panorama delle minacce.
- Collabora con il tuo team di sicurezza per determinare quali regole sono in linea con la postura di sicurezza e i requisiti di conformità della tua organizzazione.
- Prima di promuoverle nell’ambiente di staging e produzione, verifica sempre le regole nuove o aggiornate negli ambienti di sviluppo.
- Durante la dichiarazione e la convalida delle regole, iniziare con il tipo `action` `log` per osservare il comportamento senza bloccare il traffico legittimo.
- Spostarsi da `log` a `block` solo dopo aver analizzato dati sul traffico sufficienti e aver confermato che non vi sono richieste valide interessate.
- Introduce regole in modo incrementale, coinvolgendo i team di test di controllo qualità, prestazioni e sicurezza per identificare gli effetti collaterali non desiderati.
- Rivedi e analizza regolarmente l&#39;efficacia delle regole utilizzando [strumenti del dashboard](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). La frequenza di revisione (giornaliera, settimanale, mensile) deve essere allineata con il volume di traffico del sito e il profilo di rischio.
- Affina continuamente le regole in base a informazioni sulle nuove minacce, comportamento del traffico e risultati di audit.

## Best practice per le regole del filtro del traffico

- Utilizza le [regole del filtro del traffico standard consigliate da Adobe](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) come linea di base, che includono regole per Edge, protezione dell&#39;origine e restrizioni basate su OFAC.
- Rivedi gli avvisi e i registri regolarmente per identificare modelli di abuso o configurazione errata.
- Regola i valori di soglia per i limiti di velocità in base ai pattern di traffico dell’applicazione e al comportamento degli utenti.

  Per istruzioni su come scegliere i valori di soglia, consulta la tabella seguente:

  | Variante | Valore |
  | :--------- | :------- |
  | Origine | Considera il valore più alto del numero massimo di richieste di origine per IP/POP in condizioni di traffico **normali** (ovvero non la frequenza al momento di un DDoS) e aumentalo di un multiplo |
  | Edge | Considera il valore più alto del numero massimo di richieste Edge per IP/POP in condizioni di traffico **normali** (ovvero non la frequenza al momento di un DDoS) e aumentalo di un multiplo |

  Vedi anche la sezione [scelta dei valori di soglia](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) per ulteriori dettagli.

- Spostarsi all&#39;azione `block` solo dopo aver verificato che l&#39;azione `log` non influisce sul traffico legittimo.

## Best practice per le regole WAF

- Inizia con le [regole di WAF consigliate](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) da Adobe, che includono regole per bloccare gli IP danneggiati noti, rilevare gli attacchi DDoS e mitigare l&#39;abuso di bot.
- Il flag WAF `ATTACK` dovrebbe segnalare potenziali minacce. Verificare che non siano presenti falsi positivi prima di passare a `block`.
- Se le regole WAF consigliate non coprono minacce specifiche, è consigliabile creare regole personalizzate in base ai requisiti univoci dell’applicazione. Vedi l&#39;elenco completo dei [flag WAF](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) nella documentazione.

## Norme di attuazione

Scopri come implementare le regole del filtro del traffico e le regole WAF in AEM as a Cloud Service:

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
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protezione dei siti web di AEM tramite le regole standard per il filtro del traffico">Protezione dei siti Web di AEM tramite le regole del filtro del traffico standard</a>
                    </p>
                    <p class="is-size-6">Scopri come proteggere i siti web di AEM dall’abuso di DoS, DDoS e bot utilizzando le regole standard consigliate da Adobe per il filtro del traffico in AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Applica regole</span>
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
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protezione dei siti web di AEM tramite le regole di WAF">Protezione dei siti Web di AEM tramite le regole di WAF</a>
                    </p>
                    <p class="is-size-6">Scopri come proteggere i siti web di AEM da minacce sofisticate, tra cui attacchi DoS, DDoS e bot utilizzando le regole di WAF (Web Application Firewall) consigliate da Adobe in AEM as a Cloud Service.</p>
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

- [Regole filtro del traffico, incluse le regole di WAF](https://experienceleague.adobe.com/it/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Informazioni sulla prevenzione DoS/DDoS in AEM](https://experienceleague.adobe.com/it/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Blocco di attacchi DoS e DDoS tramite le regole del filtro del traffico](https://experienceleague.adobe.com/it/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

