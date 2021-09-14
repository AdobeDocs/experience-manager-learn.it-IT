---
title: Configurazione dell’accesso a AEM come Cloud Service
description: AEM come Cloud Service è il modo nativo del cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso degli utenti, amministratori e utenti normali, al servizio Author di AEM. Scopri in che modo gli utenti, i gruppi di utenti e i profili di prodotto Adobe IMS vengono utilizzati insieme a gruppi AEM e autorizzazioni per fornire un accesso specifico ad AEM Author.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Configurazione dell’accesso a AEM come Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introduzione ad Adobe IMS"
>abstract="AEM as a Cloud Service sfrutta l’Adobe IMS (Identity Management System) per facilitare l’accesso dei suoi utenti, amministratori e utenti normali, al servizio AEM Author. Scopri in che modo gli utenti, i gruppi e i profili di prodotto di Adobe IMS vengono utilizzati insieme ai gruppi AEM e alle autorizzazioni per fornire un accesso dettagliato al servizio di authoring di AEM."

AEM come Cloud Service è il modo nativo del cloud di sfruttare le applicazioni AEM e, come tale, sfrutta l’Adobe IMS (Identity Management System) per facilitare l’accesso dei suoi utenti, amministratori e utenti normali, al servizio AEM Author.

![Adobe Admin Console](./assets/hero.png)

Scopri in che modo gli utenti, i gruppi e i profili di prodotto Adobe IMS vengono utilizzati insieme ai gruppi AEM e alle autorizzazioni per fornire un accesso dettagliato al servizio di authoring di AEM.

## Adobe utenti IMS

Gli utenti che richiedono l’accesso al servizio AEM Author vengono gestiti come [utenti Adobe IMS](https://helpx.adobe.com/it/enterprise/using/set-up-identity.html) in [AdminConsole di Adobe](https://adminconsole.adobe.com). Scopri gli Adobi sugli utenti IMS e come accedervi e gestirli in Admin Console.

[Scopri di più sugli utenti Adobe IMS](./adobe-ims-users.md)

## Adobe di gruppi di utenti IMS

Gli utenti che accedono al servizio AEM Author devono essere organizzati in gruppi logici utilizzando [Adobi gruppi di utenti IMS](https://helpx.adobe.com/enterprise/using/user-groups.html) in [Adobe AdminConsole](https://adminconsole.adobe.com). I gruppi di utenti Adobe IMS non forniscono autorizzazioni dirette o accesso a AEM (questo è il lavoro di [profili di prodotto Adobe IMS](#adobe-ims-product-profiles)), tuttavia sono un ottimo modo per definire raggruppamenti logici di utenti che possono a loro volta essere convertiti a specifici livelli di accesso nel servizio di authoring di AEM, utilizzando AEM gruppi e autorizzazioni.

[Scopri i gruppi di utenti Adobe IMS](./adobe-ims-user-groups.md)

## Profili di prodotto Adobe IMS

[I profili](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html) di prodotto Adobe IMS, gestiti in  [Adobe Admin Console](https://adminconsole.adobe.com), sono il meccanismo che fornisce agli utenti di  [Adobe IMS l’accesso ](#adobe-ims-users) al servizio Author di AEM con un livello di accesso di base.

+ Il profilo di prodotto __AEM Utenti__ consente agli utenti l&#39;accesso in sola lettura a AEM tramite l&#39;iscrizione al gruppo di collaboratori AEM.
+ Il profilo di prodotto __AEM Administrators__ consente agli utenti l&#39;accesso completo e amministrativo a AEM.

[Scopri i profili di prodotto Adobe IMS](./adobe-ims-product-profiles.md)

## AEM gruppi di utenti e autorizzazioni

Adobe Experience Manager si basa su utenti Adobe IMS, gruppi di utenti e profili di prodotto per fornire agli utenti un accesso personalizzabile a AEM. Scopri come creare gruppi AEM e autorizzazioni e come funzionano insieme alle astrazioni IMS di Adobe per fornire un accesso diretto e personalizzabile ai AEM.

[Informazioni su AEM utente, gruppi e autorizzazioni](./aem-users-groups-and-permissions.md)

## Procedura dettagliata su accesso e autorizzazioni

Una procedura dettagliata per configurare gli utenti, i gruppi di utenti e i profili di prodotto Adobe IMS in Adobe Admin Console, e come sfruttare queste astrazioni Adobe IMS in AEM Author per definire e gestire autorizzazioni specifiche basate su gruppi.

[Procedura dettagliata sull’accesso AEM e le autorizzazioni](./walk-through.md)

## Risorse Adobe Admin Console aggiuntive

La documentazione seguente descrive i dettagli e i problemi specifici di [Adobe Admin Console](https://adminconsole.adobe.com) che possono contribuire a una migliore comprensione di Adobe Admin Console e a usarlo per gestire gli utenti e l&#39;accesso tra i prodotti Experience Cloud.

+ [Panoramica di Adobe Admin Console Identity](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Ruoli amministratore di Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Ruoli sviluppatore di Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)
