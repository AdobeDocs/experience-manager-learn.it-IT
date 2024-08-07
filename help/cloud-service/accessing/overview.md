---
title: Configurazione dell’accesso a AEM as a Cloud Service
description: AEM as a Cloud Service è il modo nativo per il cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso degli utenti, sia amministratori che utenti normali, al servizio AEM Author. Scopri in che modo gli utenti Adobe IMS, i gruppi di utenti e i profili di prodotto vengono utilizzati insieme ai gruppi e alle autorizzazioni dell’AEM per fornire accesso specifico all’istanza di authoring dell’AEM.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 26%

---

# Configurazione dell’accesso a AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introduzione ad Adobe IMS"
>abstract="AEM as a Cloud Service sfrutta Adobe IMS (Identity Management System) per gestire l’accesso al servizio AEM Author da parte dei diversi tipi di utenti, dagli amministratori agli utenti normali. Scopri in che modo gli utenti, i gruppi e i profili di prodotto di Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per gestire l’accesso al servizio AEM Author in modo granulare."

AEM as a Cloud Service è il modo nativo per il cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso al servizio AEM Author dei suoi utenti, sia amministratori che utenti normali.

![Adobe Admin Console](./assets/hero.png)

Scopri in che modo gli utenti, i gruppi e i profili di prodotto di Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per gestire l’accesso al servizio AEM Author in modo granulare.

## Utenti Adobe IMS

Gli utenti che richiedono l&#39;accesso al servizio di authoring AEM vengono gestiti come [utenti Adobe IMS](https://helpx.adobe.com/it/enterprise/using/set-up-identity.html) in [AdminConsole di Adobe](https://adminconsole.adobe.com). Scopri cosa si intende per utenti di Adobe IMS, nonché come accedervi e gestirli in Admin Console.

>[!NOTE]
>
>Quando un utente IMS viene eliminato da AdminConsole, non viene eliminato automaticamente dall’AEM, ma una volta scaduta la sessione (token) AEM NON può accedere all’AEM.


[Scopri gli utenti Adobe IMS](./adobe-ims-users.md)

## Gruppi di utenti di Adobe IMS

Gli utenti che accedono al servizio di authoring AEM devono essere organizzati in gruppi logici utilizzando [gruppi di utenti Adobe IMS](https://helpx.adobe.com/it/enterprise/using/user-groups.html) in [AdminConsole di Adobe](https://adminconsole.adobe.com). I gruppi di utenti Adobe IMS non forniscono autorizzazioni dirette o accesso all&#39;AEM (questo è il lavoro di [profili di prodotto Adobe IMS](#adobe-ims-product-profiles)), ma sono un ottimo modo per definire raggruppamenti logici di utenti che possono a loro volta essere convertiti in livelli specifici di accesso nel servizio di authoring AEM, utilizzando i gruppi e le autorizzazioni AEM.

[Informazioni sui gruppi di utenti Adobe IMS](./adobe-ims-user-groups.md)

## Profili di prodotto di Adobe IMS

I [profili di prodotto Adobe IMS](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), gestiti in [AdminConsole](https://adminconsole.adobe.com) di Adobe, sono il meccanismo che fornisce a [utenti Adobe IMS](#adobe-ims-users) l&#39;accesso per accedere al servizio Author AEM con un livello di accesso di base.

+ Il profilo di prodotto __Utenti AEM__ consente agli utenti l&#39;accesso in sola lettura all&#39;AEM tramite l&#39;appartenenza al gruppo dei collaboratori dell&#39;AEM.
+ Il profilo di prodotto __Amministratori AEM__ offre agli utenti accesso amministrativo completo all&#39;AEM.

[Scopri i profili di prodotto di Adobe IMS](./adobe-ims-product-profiles.md)

## Gruppi di utenti e autorizzazioni AEM

Adobe Experience Manager si basa sugli utenti, i gruppi di utenti e i profili di prodotto di Adobe IMS per consentire agli utenti di accedere ad AEM con autorizzazioni personalizzabili. Scopri come creare gruppi e autorizzazioni AEM e come funzionano insieme alle astrazioni di Adobe IMS per fornire un accesso fluido e personalizzabile all’AEM.

[Scopri gli utenti, i gruppi e le autorizzazioni di AEM](./aem-users-groups-and-permissions.md)

## Procedura dettagliata per l’accesso e le autorizzazioni

Adobe Una procedura dettagliata ridotta per la configurazione di utenti Adobe IMS, gruppi di utenti e profili di prodotto in AdminConsole e per sfruttare queste astrazioni di Adobe IMS in AEM Author per definire e gestire autorizzazioni specifiche basate su gruppi.

[Procedura dettagliata per l’accesso e le autorizzazioni AEM](./walk-through.md)

## Risorse Adobe Admin Console aggiuntive

La seguente documentazione tratta i dettagli e i dubbi specifici di [Adobe Admin Console](https://adminconsole.adobe.com) che possono aiutare a comprendere meglio Adobe Admin Console e a utilizzarlo per gestire gli utenti e l&#39;accesso tra i prodotti Experience Cloud.

+ [Panoramica identità Adobe Admin Console](https://helpx.adobe.com/it/enterprise/using/identity.html)
+ [Ruoli amministratore Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Ruoli sviluppatore Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)
