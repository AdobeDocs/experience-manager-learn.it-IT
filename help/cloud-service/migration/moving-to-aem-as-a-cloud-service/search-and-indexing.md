---
title: Ricerca e indicizzazione in AEM as a Cloud Service
description: Scopri AEM indici di ricerca di as a Cloud Service, come convertire le definizioni AEM 6 e come distribuire gli indici.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 2%

---

# Ricerca e indicizzazione

Scopri AEM indici di ricerca di as a Cloud Service, come convertire AEM 6 definizioni di indice in modo AEM compatibile as a Cloud Service e come distribuire gli indici in AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Strumento Convertitore indice

![Strumento Convertitore indice](./assets/index-converter.png)

Come parte del refactoring della base di codice, utilizza il [Strumento di conversione indice](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) per convertire le definizioni personalizzate dell&#39;indice Oak in definizioni di indici compatibili as a Cloud Service.

## Attività chiave

+ Utilizza la [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) strumento per migrare i flussi di lavoro per l’elaborazione delle risorse e utilizzare i microservizi Asset compute.
+ Imposta un [ambiente di sviluppo locale](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=it) e distribuire gli indici personalizzati. Assicurati che gli indici aggiornati siano aggiornati.
+ Distribuisci la base di codice aggiornata in un ambiente di sviluppo AEM as a Cloud Service e continua a eseguire la convalida.
+ Se modifichi un indice preconfigurato **SEMPRE** copia la definizione di indice più recente da un ambiente as a Cloud Service AEM in esecuzione nella versione più recente. Modifica la definizione dell&#39;indice copiata in base alle tue esigenze.

## Esercitazione pratica

Applica la tua conoscenza provando quello che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver visto e compreso il video precedente e i seguenti materiali:

+ [Pensare diversamente a AEM as a Cloud Service](./introduction.md)
+ [Modernizzazione archivio](./repository-modernization.md)

Inoltre, assicurati di aver completato l&#39;esercizio pratico precedente:

+ [Esercitazione pratica sullo strumento Content Transfer (Trasferimento contenuti)](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="Esercitazione pratica dell’archivio GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Hands-on con indici</div>
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
