---
title: AEM Assets Microservices e passaggio a AEM as a Cloud Service
description: Scopri in che modo i microservizi di asset compute di AEM Assets as a Cloud Service consentono di generare automaticamente ed efficacemente qualsiasi rendering per le risorse, sostituendo questo ruolo del flusso di lavoro AEM tradizionale.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 3%

---

# AEM Assets Microservices - Passaggio a AEM as a Cloud Service

Scopri in che modo i microservizi di asset compute di AEM Assets as a Cloud Service consentono di generare automaticamente ed efficacemente qualsiasi rendering per le risorse, sostituendo questo ruolo del flusso di lavoro AEM tradizionale.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Strumento di migrazione del flusso di lavoro

![Strumento Asset Workflow Migration (Migrazione flussi di lavoro per risorse) ](./assets/asset-workflow-migration.png)

Come parte del refactoring della base di codice, utilizza il [Strumento Asset Workflow Migration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) per migrare i flussi di lavoro esistenti e utilizzare i microservizi Asset compute in AEM as a Cloud Service.

## Attività chiave

+ Utilizza la [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) strumento per migrare i flussi di lavoro per l’elaborazione delle risorse e utilizzare i microservizi Asset compute.
+ Imposta un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it) e distribuire i flussi di lavoro aggiornati. Potrebbe essere necessaria una regolazione manuale per flussi di lavoro complessi.
+ Continua a eseguire iterazioni in un ambiente di sviluppo locale utilizzando l’SDK AEM fino a quando il flusso di lavoro aggiornato non corrisponde alla parità delle funzioni.
+ Distribuisci la base di codice aggiornata in un ambiente di sviluppo AEM as a Cloud Service e continua a eseguire la convalida.

## Esercitazione pratica

Applica la tua conoscenza provando quello che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver visto e compreso il video precedente e i seguenti materiali:

+ [Pensare diversamente a AEM as a Cloud Service](./introduction.md)
+ [Onboarding](./onboarding.md)

Inoltre, assicurati di aver completato l&#39;esercizio pratico precedente:

+ [Esercitazione pratica di ricerca e indicizzazione](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="Esercitazione pratica dell’archivio GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Mani attive con il caricamento delle risorse</div>
            <p style="margin:1rem 0">
                Scopri come definire e assegnare profili di elaborazione AEM Assets alle cartelle e caricare risorse in AEM utilizzando il modulo npm CLI "aem-upload".
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Provare la gestione delle risorse</span>
            </a>
        </td>
    </tr>
</table>
