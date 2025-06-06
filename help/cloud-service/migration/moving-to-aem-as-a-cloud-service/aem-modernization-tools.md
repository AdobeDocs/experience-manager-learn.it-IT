---
title: Utilizzo degli strumenti di modernizzazione AEM per il passaggio ad AEM as a Cloud Service
description: Scopri come gli Strumenti di modernizzazione AEM vengono utilizzati per aggiornare un progetto AEM esistente e il contenuto affinché siano compatibili con AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
duration: 2502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 1%

---


# Strumenti AEM Modernization Tools

Scopri come gli strumenti di modernizzazione AEM vengono utilizzati per aggiornare un contenuto AEM Sites esistente affinché sia compatibile con AEM as a Cloud Service e in linea con le best practice.

## Convertitore all-in-one

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## Conversione pagina

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## Conversione del componente

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## Importazione criteri

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## Utilizzo degli strumenti di modernizzazione AEM

![Ciclo di vita degli strumenti di modernizzazione AEM](./assets/aem-modernization-tools.png)

Gli strumenti di modernizzazione AEM convertono automaticamente le pagine AEM esistenti composte da modelli statici legacy, componenti di base e parsys, per utilizzare approcci moderni quali modelli modificabili, componenti WCM core di AEM e contenitori di layout.

## Attività chiave

+ Clona la produzione di AEM 6.x per eseguire gli strumenti di modernizzazione AEM su
+ Scarica e installa gli [strumenti di modernizzazione AEM più recenti](https://github.com/adobe/aem-modernize-tools/releases/latest) sul clone di produzione AEM 6.x tramite Gestione pacchetti

+ [Il convertitore struttura pagine](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) aggiorna il contenuto della pagina esistente da modello statico a modello modificabile mappato utilizzando i contenitori di layout
   + Definire le regole di conversione utilizzando la configurazione OSGi
   + Eseguire il convertitore struttura pagina rispetto alle pagine esistenti

+ [Il convertitore componenti](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) aggiorna il contenuto della pagina esistente da modello statico a modello modificabile mappato utilizzando i contenitori di layout
   + Definire le regole di conversione tramite definizioni di nodi JCR/XML
   + Eseguire lo strumento Component Converter sulle pagine esistenti

+ [Importazione criteri](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) crea criteri dalla configurazione di progettazione
   + Definire le regole di conversione utilizzando le definizioni dei nodi JCR/XML
   + Eseguire Importazione criteri in base alle definizioni di progettazione esistenti
   + Applicare i criteri importati a componenti e contenitori AEM

## Esercizio pratico

Applica la tua conoscenza sperimentando ciò che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver guardato e compreso il video precedente e i seguenti materiali:

+ [Pensare diversamente ad AEM as a Cloud Service](./introduction.md)
+ [Modernizzazione dell’archivio](./repository-modernization.md)
+ [Contenuto mutabile e immutabile](../../developing/basics/mutable-immutable.md)
+ [Struttura del progetto AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=it)

Inoltre, assicurati di aver completato il precedente esercizio pratico:

+ [Esercizio pratico su BPA e CAM](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="Esercitazione pratica archivio GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Modernizzazione di AEM</div>
            <p style="margin:1rem 0">
                Scopri come utilizzare gli strumenti di modernizzazione AEM per aggiornare un sito WKND legacy in base alle best practice di AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Prova gli strumenti di modernizzazione AEM</span>
            </a>
        </td>
    </tr>
</table>

## Altre risorse

+ [Scarica strumenti di modernizzazione AEM](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentazione sugli strumenti di modernizzazione AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - Introduzione alla suite di modernizzazione AEM](https://helpx.adobe.com/it/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. Distribuisci il sito wknd-legacy appena modernizzato sul SDK AEM locale. AEM ASK disponibile per il download qui:
   + [Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
