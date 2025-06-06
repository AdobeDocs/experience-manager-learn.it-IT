---
title: Migrazione dei contenuti tramite lo strumento Content Transfer (Trasferimento contenuti)
description: Scopri come lo strumento Content Transfer consente di migrare i contenuti ad AEM as a Cloud Service da AEM 6.
version: Experience Manager as a Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
duration: 1362
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 3%

---


# Strumento trasferimento contenuti

Scopri come lo strumento Content Transfer consente di migrare i contenuti ad AEM as a Cloud Service da AEM 6.3+.

>[!VIDEO](https://video.tv.adobe.com/v/3454755?quality=12&learn=on&captions=ita)

## Utilizzo dello strumento Content Transfer (Trasferimento contenuti)

![Ciclo di vita dello strumento Content Transfer](../assets/content-transfer-tool.png)

Lo strumento Content Transfer (Trasferimento contenuti) è installato in AEM 6.3+ e trasferisce i contenuti ad AEM as a Cloud Service.

## Attività chiave

+ Scarica lo [strumento Content Transfer più recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=2).
+ Trasferisci il contenuto finale di AEM Author 6.3+ al servizio AEM as a Cloud Service Author.
   + Installa lo strumento Content Transfer (Trasferimento contenuti) in AEM 6.3+ Author contenente il contenuto finale da trasferire.
   + Esegui lo strumento Content Transfer (Trasferimento contenuti) in batch, trasferendo set di contenuti.
+ Trasferisci il contenuto finale di AEM Publish 6.3+ al servizio di pubblicazione AEM as a Cloud Service.
   + Installa lo strumento Content Transfer (Trasferimento contenuti) nella pubblicazione AEM 6.3+ contenente il contenuto finale da trasferire.
   + Esegui lo strumento Content Transfer (Trasferimento contenuti) in batch, trasferendo set di contenuti.
+ Facoltativamente, contenuti &quot;integrativi&quot; su AEM as a Cloud Service, trasferendo nuovi contenuti dall’ultimo trasferimento di contenuti

## Esercizio pratico

Applica la tua conoscenza sperimentando ciò che hai imparato con questo esercizio pratico.

Prima di provare l&#39;esercizio pratico, assicurati di aver guardato e compreso il video precedente e i seguenti materiali:

+ [Strumenti AEM Modernization Tools](../aem-modernization-tools.md)
+ [Onboarding](../onboarding.md)
+ [Cloud Manager](../cloud-manager.md)

Inoltre, assicurati di aver completato il precedente esercizio pratico:

+ [Esercitazione pratica su Dispatcher](../dispatcher.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content"><img alt="Esercitazione pratica archivio GitHub" src="../assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Strumento di trasferimento dei contenuti</div>
            <p style="margin:1rem 0">
                Scopri come lo strumento Content Transfer (Trasferimento contenuti) può spostare automaticamente il contenuto da AEM 6 ad AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Prova lo strumento Content Transfer</span>
            </a>
        </td>
    </tr>
</table>

## Altre risorse

+ [Download dello strumento Content Transfer](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=tipo di software%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=2)
+ [Video descrittivo del servizio di importazione in blocco](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html?lang=it)

