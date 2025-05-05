---
title: Configurazione di Dispatcher per il passaggio ad AEM as a Cloud Service
description: Scopri le modifiche di rilievo apportate ad AEM Dispatcher per AEM as a Cloud Service, lo strumento di conversione Dispatcher e come utilizzare Dispatcher Tools SDK.
version: Experience Manager as a Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
duration: 1618
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 4%

---


# Dispatcher

Scopri AEM Dispatcher per AEM as a Cloud Service, concentrandoti sulle modifiche di rilievo apportate da Dispatcher per AEM 6, sullo strumento di conversione Dispatcher e su come utilizzare Dispatcher Tools SDK.

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Come parte del refactoring della base di codice, utilizza [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html?lang=it) per eseguire il refactoring delle configurazioni esistenti di Dispatcher on-premise o Adobe Managed Services alla configurazione di Dispatcher compatibile con AEM as a Cloud Service.

## Attività chiave

+ Utilizza lo strumento [Adobe I/O Dispatcher Converter](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) per eseguire la migrazione di una configurazione Dispatcher esistente.
+ Fai riferimento al modulo Dispatcher dall&#39;[Archetipo progetto AEM](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) come best practice.
+ [Configura gli strumenti Dispatcher locali](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=it) per convalidare Dispatcher prima di eseguire il test in un ambiente Cloud Service.

## Esercizio pratico

Applica la tua conoscenza sperimentando ciò che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver guardato e compreso il video precedente e i seguenti materiali:

+ [Strumenti AEM Modernization Tools](./aem-modernization-tools.md)
+ [Onboarding](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

Inoltre, assicurati di aver completato il precedente esercizio pratico:

+ [Esercitazione pratica su Cloud Manager](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="Esercitazione pratica archivio GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Guida pratica con gli strumenti Dispatcher</div>
            <p style="margin:1rem 0">
                Esplora l’utilizzo degli strumenti Dispatcher di AEM SDK per convalidare le configurazioni di Dispatcher e eseguire AEM Dispatcher localmente utilizzando Docker.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Prova gli strumenti di Dispatcher</span>
            </a>
        </td>
    </tr>
</table>
