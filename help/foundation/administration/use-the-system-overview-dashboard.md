---
title: Utilizzare il dashboard Panoramica del sistema in AEM
description: Nelle versioni precedenti, gli amministratori dell’AEM dovevano esaminare più sedi per avere un quadro completo dell’istanza dell’AEM. La Panoramica del sistema ha lo scopo di risolvere questo problema fornendo una panoramica di alto livello della configurazione, dell’hardware e dello stato dell’istanza dell’AEM, il tutto da un singolo dashboard.
version: 6.4, 6.5
topics: administration, operations, monitoring
feature: Operations
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: Administration
role: Admin
level: Beginner
exl-id: af8f499c-4955-44b5-8f21-085263ca31a3
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Utilizzare il dashboard Panoramica sistema

Adobe Experience Manager (AEM) [!UICONTROL Panoramica del sistema] fornisce una vista di alto livello della configurazione, dell’hardware e dello stato dell’istanza dell’AEM, il tutto da un singolo dashboard.

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. La panoramica del sistema è accessibile da: **Inizio AEM** > **[!UICONTROL Strumenti]** > **[!UICONTROL Operazioni]** > **[!UICONTROL Panoramica del sistema]**

   Direttamente a **`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. Le informazioni provenienti dal [!UICONTROL Panoramica del sistema] possono essere esportati facendo clic sul pulsante [!UICONTROL Scarica] pulsante. Le informazioni sono esposte anche tramite i seguenti [!DNL REST] endpoint:
1. Di seguito è riportato un output di esempio del JSON esportato da [!UICONTROL Panoramica del sistema]:

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
