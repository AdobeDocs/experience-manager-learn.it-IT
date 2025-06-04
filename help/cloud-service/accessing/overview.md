---
title: Configurazione dell’accesso ad AEM as a Cloud Service
description: AEM as a Cloud Service è il modo nativo per il cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso degli utenti, sia amministratori che utenti normali, al servizio AEM Author. Scopri in che modo gli utenti, i gruppi di utenti e i profili di prodotto di Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per fornire l’accesso specifico al servizio Authoring AEM.
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '598'
ht-degree: 100%

---

# Configurazione dell’accesso ad AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introduzione ad Adobe IMS"
>abstract="AEM as a Cloud Service sfrutta Adobe IMS (Identity Management System) per gestire l’accesso al servizio AEM Author da parte dei diversi tipi di utenti, dagli amministratori agli utenti normali. Scopri in che modo gli utenti, i gruppi di utenti e i profili di prodotto di Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per fornire l’accesso specifico al servizio AEM Author."

AEM as a Cloud Service è il modo nativo per il cloud di sfruttare le applicazioni AEM e, come tale, sfrutta Adobe IMS (Identity Management System) per facilitare l’accesso degli utenti, sia amministratori che utenti normali, al servizio AEM Author.

![Adobe Admin Console](./assets/hero.png)

Scopri in che modo gli utenti, i gruppi di utenti e i profili di prodotto di Adobe IMS vengono utilizzati insieme ai gruppi e alle autorizzazioni di AEM per fornire l’accesso specifico al servizio AEM Author.

## Utenti di Adobe IMS

Gli utenti che richiedono l’accesso al servizio AEM Author, vengono gestiti come [utenti di Adobe IMS](https://helpx.adobe.com/it/enterprise/using/set-up-identity.html) in [Adobe Admin Console](https://adminconsole.adobe.com). Scopri cosa si intende per utenti di Adobe IMS, nonché come accedervi e gestirli in Admin Console.

>[!NOTE]
>
>Quando un utente IMS viene eliminato da Admin Console, non viene eliminato automaticamente da AEM, ma una volta scaduta la sessione (token) di AEM NON può accedere ad AEM.


[Scopri gli utenti di Adobe IMS](./adobe-ims-users.md)

## Gruppi di utenti di Adobe IMS

Gli utenti che accedono al servizio AEM Author devono essere organizzati in gruppi logici utilizzando [gruppi di utenti Adobe IMS](https://helpx.adobe.com/it/enterprise/using/user-groups.html) in [Adobe Admin Console](https://adminconsole.adobe.com). I gruppi di utenti Adobe IMS non forniscono autorizzazioni o accesso diretto ad AEM (questo è il processo dei [profili di prodotto Adobe IMS](#adobe-ims-product-profiles)), ma rappresentano un ottimo modo per definire raggruppamenti logici di utenti che possono a loro volta essere tradotti in livelli specifici di accesso nel servizio AEM Author, utilizzando i gruppi e le autorizzazioni di AEM.

[Scopri di più sui gruppi di utenti di Adobe IMS](./adobe-ims-user-groups.md)

## Profili di prodotto di Adobe IMS

I [profili di prodotto di Adobe IMS](https://helpx.adobe.com/it/enterprise/using/manage-permissions-and-roles.html), gestiti in [Adobe Admin Console](https://adminconsole.adobe.com), sono il meccanismo che fornisce agli [utenti di Adobe IMS](#adobe-ims-users) l’accesso al servizio AEM Author con un livello di accesso di base.

+ Il profilo di prodotto __Utenti AEM__ consente agli utenti di accedere in sola lettura ad AEM tramite l’appartenenza al gruppo di collaboratori di AEM.
+ Il profilo di prodotto __Amministratori AEM__ consente agli utenti l’accesso amministrativo completo ad AEM.

[Scopri di più sui profili di prodotto di Adobe IMS.](./adobe-ims-product-profiles.md)

## Gruppi di utenti e autorizzazioni di AEM

Adobe Experience Manager si basa sugli utenti, i gruppi di utenti e i profili di prodotto di Adobe IMS per consentire agli utenti di accedere ad AEM con autorizzazioni personalizzabili. Scopri come creare gruppi e autorizzazioni di AEM e come questi interagiscono con le astrazioni di Adobe IMS per fornire un accesso ad AEM fluido e personalizzabile.

[Informazioni su utenti, gruppi e autorizzazioni di AEM](./aem-users-groups-and-permissions.md)

## Procedura dettagliata per accessi e autorizzazioni

Procedura dettagliata e sintetica sulla configurazione di utenti, gruppi di utenti e profili di prodotto di Adobe IMS in Adobe Admin Console e su come sfruttare queste astrazioni di Adobe IMS nell’authoring AEM per definire e gestire le autorizzazioni specifiche basate su gruppi.

[Procedura dettagliata per accessi e autorizzazioni di AEM](./walk-through.md)

## Risorse di Adobe Admin Console aggiuntive

La seguente documentazione tratta i dettagli e le considerazioni specifici di [Adobe Admin Console](https://adminconsole.adobe.com) che possono aiutare a comprendere meglio Adobe Admin Console e a come utilizzarla per gestire gli utenti e l’accesso tra i prodotti Experience Cloud.

+ [Panoramica dell’identità di Adobe Admin Console](https://helpx.adobe.com/it/enterprise/using/identity.html)
+ [Ruoli amministratore di Adobe Admin Console](https://helpx.adobe.com/it/enterprise/using/admin-roles.html)
+ [Ruoli sviluppatore di Adobe Admin Console](https://helpx.adobe.com/it/enterprise/using/manage-developers.html)
