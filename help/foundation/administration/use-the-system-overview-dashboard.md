---
title: Utilizzare il Pannello Panoramica di sistema in AEM
description: Nelle versioni precedenti degli amministratori AEM era necessario esaminare diverse posizioni per ottenere un quadro completo dell'istanza AEM. La Panoramica del sistema mira a risolvere questo problema fornendo una visualizzazione di alto livello della configurazione, dell'hardware e dello stato dell'istanza AEM, il tutto da un'unica dashboard.
version: 6.4, 6.5
feature: null
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
translation-type: tm+mt
source-git-commit: c9a11bcb01a5ec9f7390deab68e6d0e1dec273de
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---


# Utilizzare il Pannello Panoramica di sistema

Adobe Experience Manager (AEM) [!UICONTROL System Overview] (Panoramicadel sistema) fornisce una visualizzazione di alto livello della configurazione, dell&#39;hardware e dello stato dell&#39;istanza AEM da un&#39;unica dashboard.

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. È possibile accedere alla Panoramica del sistema da: **AEM Start** > **[!UICONTROL Strumenti]** > **[!UICONTROL Operazioni]** > Panoramica **[!UICONTROL del sistema]**

   Direttamente a **`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. Per esportare le informazioni contenute in Panoramica  del sistema, fare clic sul pulsante [!UICONTROL Scarica] . Le informazioni sono inoltre esposte tramite il seguente [!DNL REST] endpoint:
1. Di seguito è riportato un esempio di output del JSON esportato da [!UICONTROL System Overview]:

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
