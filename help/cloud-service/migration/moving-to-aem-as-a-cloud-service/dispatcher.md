---
title: Configurazione di Dispatcher quando si passa a AEM as a Cloud Service
description: Scopri le modifiche di rilievo apportate AEM Dispatcher per AEM as a Cloud Service, lo strumento di conversione del Dispatcher e come utilizzare l’SDK per strumenti di Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 3%

---


# Dispatcher

Scopri AEM Dispatcher per AEM as a Cloud Service, concentrandoti sulle modifiche di rilievo apportate da Dispatcher per AEM 6, sullo strumento di conversione di Dispatcher e su come utilizzare l’SDK degli strumenti di Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Come parte del refactoring della base di codice, utilizza il [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) per reimpostare le configurazioni esistenti del Dispatcher on-premise o Adobe Managed Services su AEM configurazione del Dispatcher compatibile as a Cloud Service.

## Attività chiave

+ Utilizza la [Strumento Adobe I/O Dispatcher Converter](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) per migrare una configurazione Dispatcher esistente.
+ Fai riferimento al modulo Dispatcher da [Archetipo di progetto AEM](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) come best practice.
+ [Impostare strumenti Dispatcher locali](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) per convalidare il dispatcher, prima di eseguire il test in un ambiente di Cloud Service.

## Esercitazione pratica

Applica la tua conoscenza provando quello che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver visto e compreso il video precedente e i seguenti materiali:

+ [Strumenti di modernizzazione AEM](./aem-modernization-tools.md)
+ [Onboarding](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

Inoltre, assicurati di aver completato l&#39;esercizio pratico precedente:

+ [Esercitazione pratica su Cloud Manager](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="Esercitazione pratica dell’archivio GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Mani attive con gli strumenti di Dispatcher</div>
            <p style="margin:1rem 0">
                Scopri come utilizzare gli strumenti Dispatcher dell’SDK AEM per convalidare le configurazioni del Dispatcher e come eseguire AEM Dispatcher localmente utilizzando Docker.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Strumenti Dispatcher</span>
            </a>
        </td>
    </tr>
</table>
