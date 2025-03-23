---
title: Microservizi AEM Assets e passaggio ad AEM as a Cloud Service
description: Scopri in che modo i microservizi Asset Compute di AEM Assets as a Cloud Service consentono di generare automaticamente ed efficacemente rendering per le risorse, sostituendo questo ruolo del flusso di lavoro AEM tradizionale.
version: Experience Manager as a Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 973
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# Microservizi AEM Assets - Passaggio ad AEM as a Cloud Service

Scopri in che modo i microservizi Asset Compute di AEM Assets as a Cloud Service consentono di generare automaticamente ed efficacemente rendering per le risorse, sostituendo questo ruolo del flusso di lavoro AEM tradizionale.

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## Strumento di migrazione flusso di lavoro

![Strumento di migrazione flusso di lavoro risorse](./assets/asset-workflow-migration.png)

Come parte del refactoring della base di codice, utilizza lo strumento [Asset Workflow Migration (Migrazione flussi di lavoro risorse)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=it) per migrare i flussi di lavoro esistenti e utilizzare i microservizi Asset Compute in AEM as a Cloud Service.

## Attività chiave

+ Utilizza lo strumento [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) per migrare i flussi di lavoro di elaborazione delle risorse e utilizzare i microservizi Asset Compute.
+ Imposta un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it) e distribuisci i flussi di lavoro aggiornati. Per i flussi di lavoro complessi può essere necessario un adeguamento manuale.
+ Continua a eseguire l’iterazione in un ambiente di sviluppo locale utilizzando AEM SDK fino a quando il flusso di lavoro aggiornato non corrisponde alla parità delle funzioni.
+ Distribuisci la base di codice aggiornata in un ambiente di sviluppo AEM as a Cloud Service e continua a convalidarla.

## Esercizio pratico

Applica la tua conoscenza sperimentando ciò che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver guardato e compreso il video precedente e i seguenti materiali:

+ [Pensare diversamente ad AEM as a Cloud Service](./introduction.md)
+ [Onboarding](./onboarding.md)

Inoltre, assicurati di aver completato il precedente esercizio pratico:

+ [Esercitazione pratica su ricerca e indicizzazione](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="Esercitazione pratica archivio GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Caricamento pratico delle risorse</div>
            <p style="margin:1rem 0">
                Scopri come definire e assegnare i profili di elaborazione di AEM Assets alle cartelle e caricare le risorse in AEM utilizzando il modulo CLI npm "aem-upload".
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Prova la gestione delle risorse</span>
            </a>
        </td>
    </tr>
</table>
