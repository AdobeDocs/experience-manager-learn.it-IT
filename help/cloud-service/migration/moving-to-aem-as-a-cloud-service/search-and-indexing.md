---
title: Ricerca e indicizzazione in AEM as a Cloud Service
description: Scopri gli indici di ricerca di AEM as a Cloud Service, come convertire le definizioni degli indici AEM 6 e come distribuire gli indici.
version: Experience Manager as a Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# Ricerca e indicizzazione

Scopri gli indici di ricerca di AEM as a Cloud Service, come convertire le definizioni degli indici AEM 6 affinché siano compatibili con AEM as a Cloud Service e come distribuire gli indici in AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Strumento Convertitore indice

![Strumento Convertitore indice](./assets/index-converter.png)

Come parte del refactoring della base di codice, utilizza lo strumento [Convertitore indice](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) per convertire le definizioni di indice Oak personalizzate in definizioni di indice compatibili con AEM as a Cloud Service.

Consulta la [documentazione del convertitore indice](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html) per informazioni sul set completo e corrente delle funzionalità del convertitore indice.

## Attività chiave

+ Utilizza lo strumento [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) per migrare i flussi di lavoro di elaborazione delle risorse e utilizzare i microservizi Asset Compute.
+ Imposta un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it) e distribuisci gli indici personalizzati. Assicurati che gli indici aggiornati siano aggiornati.
+ Distribuisci la base di codice aggiornata in un ambiente di sviluppo AEM as a Cloud Service e continua a convalidarla.
+ Se modifichi un indice predefinito **ALWAYS**, copia la definizione dell’indice più recente da un ambiente AEM as a Cloud Service in esecuzione sulla versione più recente. Modifica la definizione dell’indice copiato in base alle tue esigenze.

## Esercizio pratico

Applica la tua conoscenza sperimentando ciò che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver guardato e compreso il video precedente e i seguenti materiali:

+ [Pensare diversamente ad AEM as a Cloud Service](./introduction.md)
+ [Modernizzazione archivio](./repository-modernization.md)

Inoltre, assicurati di aver completato il precedente esercizio pratico:

+ [Esercitazione pratica sullo strumento Content Transfer (Trasferimento contenuti)](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="Esercitazione pratica archivio GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Pratico con gli indici</div>
            <p style="margin:1rem 0">
                Esplora la definizione e la distribuzione degli indici Oak in AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Prova l'indicizzazione</span>
            </a>
        </td>
    </tr>
</table>
