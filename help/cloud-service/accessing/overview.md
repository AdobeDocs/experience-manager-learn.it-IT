---
title: Configurazione dell’accesso a AEM as a Cloud Service
description: AEM as a Cloud Service è il modo nativo del cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso degli utenti, amministratori e utenti normali, al servizio AEM Author. Scopri in che modo gli utenti, i gruppi di utenti e i profili di prodotto Adobe IMS vengono utilizzati insieme a gruppi AEM e autorizzazioni per fornire un accesso specifico ad AEM Author.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 26%

---

# Configurazione dell’accesso a AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introduzione ad Adobe IMS"
>abstract="AEM as a Cloud Service sfrutta Adobe IMS (Identity Management System) per gestire l’accesso al servizio AEM Author da parte dei diversi tipi di utenti, dagli amministratori agli utenti normali. Scopri in che modo gli utenti, i gruppi e i profili di prodotto di Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per gestire l’accesso al servizio AEM Author in modo granulare."

AEM as a Cloud Service è il modo nativo del cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso dei suoi utenti, amministratori e utenti normali, al servizio Author di AEM.

![Adobe Admin Console](./assets/hero.png)

Scopri in che modo gli utenti, i gruppi e i profili di prodotto di Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per gestire l’accesso al servizio AEM Author in modo granulare.

## Utenti Adobe IMS

Gli utenti che richiedono l’accesso al servizio AEM Author vengono gestiti come [Utenti Adobe IMS](https://helpx.adobe.com/it/enterprise/using/set-up-identity.html) in [Admin Console di Adobe](https://adminconsole.adobe.com). Scopri cosa si intende per utenti di Adobe IMS, nonché come accedervi e gestirli in Admin Console.

>[!NOTE]
>
>Quando un utente IMS viene eliminato da AdminConsole, non viene eliminato automaticamente da AEM, ma una volta scaduta AEM sessione (token) NON può accedere a AEM.


[Scopri gli utenti di Adobe IMS](./adobe-ims-users.md)

## Gruppi di utenti di Adobe IMS

Gli utenti che accedono al servizio AEM Author devono essere organizzati in gruppi logici utilizzando [Gruppi di utenti di Adobe IMS](https://helpx.adobe.com/it/enterprise/using/user-groups.html) in [Admin Console di Adobe](https://adminconsole.adobe.com). I gruppi di utenti di Adobe IMS non forniscono autorizzazioni dirette o l’accesso a AEM (questo è il lavoro di [Profili di prodotto Adobe IMS](#adobe-ims-product-profiles)), tuttavia, sono un ottimo modo per definire raggruppamenti logici di utenti che possono a loro volta essere tradotti in specifici livelli di accesso nel servizio AEM Author, utilizzando gruppi AEM e autorizzazioni.

[Scopri i gruppi di utenti di Adobe IMS](./adobe-ims-user-groups.md)

## Profili di prodotto Adobe IMS

[Profili di prodotto Adobe IMS](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), gestito in [Admin Console di Adobe](https://adminconsole.adobe.com), sono il meccanico che fornisce [Utenti Adobe IMS](#adobe-ims-users) accesso al servizio AEM Author con un livello di accesso base.

+ La __Utenti AEM__ il profilo di prodotto consente agli utenti l’accesso in sola lettura a AEM tramite l’iscrizione al gruppo AEM collaboratori.
+ La __Amministratori AEM__ il profilo di prodotto offre agli utenti un accesso completo e amministrativo alle AEM.

[Informazioni sui profili di prodotto Adobe IMS](./adobe-ims-product-profiles.md)

## AEM gruppi di utenti e autorizzazioni

Adobe Experience Manager si basa sugli utenti, i gruppi di utenti e i profili di prodotto di Adobe IMS per consentire agli utenti di accedere ad AEM con autorizzazioni personalizzabili. Scopri come creare gruppi AEM e autorizzazioni e come interagiscono con le astrazioni Adobe IMS per fornire un accesso semplice e personalizzabile ai AEM.

[Informazioni su AEM utente, gruppi e autorizzazioni](./aem-users-groups-and-permissions.md)

## Procedura dettagliata su accesso e autorizzazioni

Una procedura dettagliata per configurare gli utenti, i gruppi di utenti e i profili di prodotto Adobe IMS in Adobe Admin Console, e come sfruttare queste astrazioni Adobe IMS in AEM Author per definire e gestire autorizzazioni specifiche basate su gruppi.

[Procedura dettagliata sull’accesso AEM e le autorizzazioni](./walk-through.md)

## Risorse Adobe Admin Console aggiuntive

La documentazione seguente tratta [Adobe Admin Console](https://adminconsole.adobe.com)- dettagli e problemi specifici che possono contribuire a una migliore comprensione di Adobe Admin Console e a usarlo per gestire gli utenti e l&#39;accesso tra i prodotti Experience Cloud.

+ [Panoramica dell’identità di Adobe Admin Console](https://helpx.adobe.com/it/enterprise/using/identity.html)
+ [Ruoli amministratore di Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Ruoli sviluppatore di Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)
