---
title: Configurare il progetto BPA e CAM
description: Scopri come Best Practices Analyzer e Cloud Acceleration Manager forniscono una guida personalizzata per la migrazione a AEM as a Cloud Service.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 3%

---

# Best practice Analyzer e Cloud Acceleration Manager

Scopri come Best Practices Analyzer (BPA) e Cloud Acceleration Manager (CAM) forniscono una guida personalizzata per la migrazione a AEM as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Utilizzo di BPA e CAM

![Diagramma di alto livello BPA e CAM](assets/bpa-cam-diagram.png)

Il pacchetto BPA deve essere installato su un clone dell’ambiente di produzione AEM 6.x. Il BPA genera un rapporto che può essere caricato in CAM, che fornirà indicazioni sulle attività chiave che devono essere svolte per passare a AEM as a Cloud Service.

## Attività chiave

+ Crea un clone dell’ambiente di produzione 6.x. Quando esegui la migrazione del contenuto e del codice di refactoring, l’utilizzo di un clone di un ambiente di produzione sarà utile per testare vari strumenti e modifiche.
+ Scarica l’ultimo strumento BPA da [Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html) e installa nell’ambiente clonato AEM 6.x.
+ Utilizza lo strumento BPA per generare un rapporto che può essere caricato su Cloud Acceleration Manager (CAM). CAM accessibile tramite [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ Utilizza CAM per fornire indicazioni sugli aggiornamenti da apportare alla base di codice e all&#39;ambiente correnti per passare a AEM as a Cloud Service.

## Esercitazione pratica

Applica la tua conoscenza provando quello che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver visto e compreso il video precedente e i seguenti materiali:

+ [Pensare diversamente a AEM as a Cloud Service](./introduction.md)
+ [Che cosa è AEM as a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Architettura di AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [Contenuto variabile e immutabile](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [Differenze nello sviluppo per AEM as a Cloud Service e AEM 6.x](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="Esercitazione pratica dell’archivio GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Maniera con Best Practices Analyzer</div>
            <p style="margin:1rem 0">
                Esplora Best Practices Analyzer (BPA) e controlla i risultati eseguendo l’analisi rispetto a una base di codice WKND legacy contenente violazioni di esempio.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Prova Best Practices Analyzer</span>
            </a>
        </td>
    </tr>
</table>


## Altre risorse

+ [Scarica Best Practices Analyzer](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)