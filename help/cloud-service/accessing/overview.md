---
title: Configurazione dell’accesso ad AEM as a Cloud Service
description: 'AEM as a Cloud Service è il modo nativo per il cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso degli utenti, amministratori e utenti normali, al servizio Author di AEM. Scopri in che modo gli utenti, i gruppi di utenti e i profili di prodotto Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per fornire un accesso specifico ad AEM Author.  '
feature: Users and Groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: Administration, Security
role: Administrator
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---


# Configurazione dell’accesso ad AEM as a Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introduzione ad Adobe IMS"
>abstract="AEM as a Cloud Service sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso dei suoi utenti, amministratori e utenti normali, al servizio Author di AEM. Scopri in che modo gli utenti, i gruppi e i profili di prodotto Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per fornire un accesso dettagliato al servizio di authoring di AEM."

AEM as a Cloud Service è il modo nativo per il cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso dei suoi utenti, amministratori e utenti normali, al servizio AEM Author. Scopri in che modo gli utenti, i gruppi e i profili di prodotto Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per fornire un accesso dettagliato al servizio di authoring di AEM.

## Utenti Adobe IMS

Gli utenti che richiedono l’accesso al servizio AEM Author vengono gestiti come [utenti Adobe IMS](https://helpx.adobe.com/it/enterprise/using/set-up-identity.html) in [AdminConsole di Adobe](https://adminconsole.adobe.com). Scopri gli utenti Adobe IMS e come accedervi e gestirli in Admin Console.

[Scopri gli utenti di Adobe IMS](./adobe-ims-users.md)

## Gruppi di utenti di Adobe IMS

Gli utenti che accedono al servizio AEM Author devono essere organizzati in gruppi logici utilizzando [gruppi di utenti Adobe IMS](https://helpx.adobe.com/enterprise/using/user-groups.html) in [AdminConsole di Adobe](https://adminconsole.adobe.com). I gruppi di utenti Adobe IMS non forniscono autorizzazioni dirette o accesso ad AEM (questo è il lavoro di [profili di prodotto Adobe IMS](#adobe-ims-product-profiles)), tuttavia sono un ottimo modo per definire raggruppamenti logici di utenti che possono a loro volta essere tradotti in specifici livelli di accesso nel servizio di authoring di AEM, utilizzando i gruppi e le autorizzazioni di AEM.

[Scopri i gruppi di utenti di Adobe IMS](./adobe-ims-user-groups.md)

## Profili di prodotto Adobe IMS

[I profili](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html) di prodotto Adobe IMS, gestiti in  [Adobe Admin Console](https://adminconsole.adobe.com), sono il meccanismo che fornisce agli utenti  [Adobe IMS l’accesso ](#adobe-ims-users) al servizio Author di AEM con un livello di accesso base.

+ Il profilo di prodotto __Utenti AEM__ consente agli utenti l’accesso in sola lettura ad AEM tramite l’iscrizione al gruppo di collaboratori di AEM.
+ Il profilo di prodotto __Amministratori AEM__ consente agli utenti di accedere in modo completo e amministrativo ad AEM.

[Informazioni sui profili di prodotto Adobe IMS](./adobe-ims-product-profiles.md)

## Gruppi di utenti e autorizzazioni di AEM

Adobe Experience Manager si basa sugli utenti Adobe IMS, sui gruppi di utenti e sui profili di prodotto per fornire agli utenti un accesso personalizzabile ad AEM. Scopri come creare gruppi e autorizzazioni AEM e come interagiscono con le astrazioni Adobe IMS per fornire un accesso fluido e personalizzabile ad AEM.

[Informazioni su utenti, gruppi e autorizzazioni di AEM](./aem-users-groups-and-permissions.md)

## Procedura dettagliata su accesso e autorizzazioni

Una procedura dettagliata per configurare utenti Adobe IMS, gruppi di utenti e profili di prodotto in Adobe Admin Console, e come sfruttare queste astrazioni Adobe IMS in AEM Author per definire e gestire autorizzazioni specifiche basate su gruppi.

[Procedura dettagliata sull’accesso e le autorizzazioni di AEM](./walk-through.md)

## Risorse aggiuntive per Adobe Admin Console

La seguente documentazione contiene [dettagli e problemi specifici di Adobe Admin Console](https://adminconsole.adobe.com) che possono essere utili per comprendere meglio Adobe Admin Console e utilizzarla per gestire gli utenti e accedere ai prodotti Experience Cloud.

+ [Panoramica di Adobe Admin Console Identity](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Ruoli amministratore di Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Ruoli per sviluppatori di Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)