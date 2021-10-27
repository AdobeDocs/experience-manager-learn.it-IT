---
title: Configurare il progetto BPA e CAM
description: Scopri come Best Practice Analyzer e Cloud Acceleration Manager forniscono una guida personalizzata per la migrazione a AEM as a Cloud Service.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 2%

---

# Best practice Analyzer e Cloud Acceleration Manager

Scopri come Best Practice Analyzer (BPA) e Cloud Acceleration Manager (CAM) forniscono una guida personalizzata per la migrazione a AEM as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Valutare la prontezza

![Diagramma di alto livello BPA e CAM](assets/bpa-cam-diagram.png)

Il pacchetto BPA deve essere installato su un clone dell’ambiente di produzione AEM 6.x. Il BPA genera un rapporto che può essere caricato in CAM, che fornirà indicazioni sulle attività chiave che devono essere svolte per passare a AEM as a Cloud Service.

### Attività chiave

* Crea un clone dell’ambiente di produzione 6.x. Quando esegui la migrazione del contenuto e del codice di refactoring, l’utilizzo di un clone di un ambiente di produzione sarà utile per testare vari strumenti e modifiche.
* Scarica l’ultimo strumento BPA da [Portale di distribuzione software](https://experience.adobe.com/#/downloads/content/software-distribution/it/aemcloud.html) e installa nell’ambiente clonato AEM 6.x.
* Utilizza lo strumento BPA per generare un rapporto che può essere caricato su Cloud Acceleration Manager (CAM). CAM accessibile tramite [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* Utilizza CAM per fornire indicazioni sugli aggiornamenti da apportare alla base di codice e all&#39;ambiente correnti per passare a AEM as a Cloud Service.
