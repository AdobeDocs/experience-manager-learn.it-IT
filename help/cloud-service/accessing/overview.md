---
title: Configurazione dell'accesso a AEM come Cloud Service
description: 'AEM come Cloud Service è il modo nativo di sfruttare le applicazioni AEM e, come tale, sfrutta  Adobe IMS ( Identity Management System) per facilitare l''accesso degli utenti, sia amministratori che utenti normali, al servizio AEM Author. Scopri  Adobe di utenti IMS, gruppi di utenti e profili di prodotto vengono utilizzati insieme AEM gruppi e autorizzazioni per fornire accesso specifico a AEM Author.  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---


# Configurazione dell&#39;accesso a AEM come Cloud Service

AEM come Cloud Service è il modo nativo basato sul cloud di sfruttare le applicazioni AEM e, come tale, sfrutta  Adobe IMS ( Identity Management System) per facilitare l&#39;accesso dei suoi utenti, sia amministratori che normali utenti, al servizio AEM Author. Scoprite come  Adobe gli utenti, i gruppi e i profili di prodotto IMS vengono utilizzati di concerto con AEM gruppi e autorizzazioni per fornire accesso dettagliato al servizio AEM Author.

##  utenti IMS Adobe

Gli utenti che richiedono l&#39;accesso al servizio AEM Author vengono gestiti come utenti IMS [ Adobe](https://helpx.adobe.com/it/enterprise/using/set-up-identity.html) in [ Adobe  AdminConsole](https://adminconsole.adobe.com). Scoprite  utenti IMS del Adobe e come accedervi e gestirli in  Admin Console.

[Scopri  Adobe di utenti IMS](./adobe-ims-users.md)

## Gruppi di utenti  Adobe IMS

Gli utenti che accedono al servizio AEM Author devono essere organizzati in gruppi logici utilizzando [ Adobe IMS group](https://helpx.adobe.com/enterprise/using/user-groups.html) in [ Adobe  AdminConsole](https://adminconsole.adobe.com).  Adobe i gruppi di utenti IMS non forniscono autorizzazioni dirette o l&#39;accesso a AEM (si tratta del lavoro di [ profili di prodotto IMS Adobe](#adobe-ims-product-profiles)), tuttavia, sono un ottimo modo per definire raggruppamenti logici di utenti che possono a loro volta essere convertiti a specifici livelli di accesso nel servizio AEM Author, utilizzando AEM gruppi e autorizzazioni.

[Scopri  Adobe di gruppi di utenti IMS](./adobe-ims-user-groups.md)

##  profili di prodotto IMS Adobe

[ profili](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html) di prodotto IMS di Adobe, gestiti in  [ Adobe  AdminConsole](https://adminconsole.adobe.com), sono il meccanismo che fornisce agli utenti  [ ](#adobe-ims-users) utenti IMS Adobe l&#39;accesso al servizio AEM Author con un livello di accesso di base.

+ Il profilo di prodotto __AEM Utenti__ consente agli utenti l&#39;accesso in sola lettura a AEM tramite l&#39;iscrizione AEM gruppo Collaboratori.
+ Il profilo di prodotto __AEM Administrators__ offre agli utenti accesso amministrativo completo alle AEM.

[Scopri i profili di prodotto  Adobe IMS](./adobe-ims-product-profiles.md)

## Gruppi di utenti AEM e autorizzazioni

Adobe Experience Manager si basa  utenti IMS, gruppi di utenti e profili di prodotto di Adobe per fornire agli utenti l&#39;accesso personalizzabile a AEM. Scoprite come creare AEM gruppi e autorizzazioni e come lavorare insieme alle astrazioni IMS  Adobe per fornire un accesso diretto e personalizzabile alle AEM.

[Scopri AEM utenti, gruppi e autorizzazioni](./aem-users-groups-and-permissions.md)

## Procedura dettagliata su accesso e autorizzazioni

Passeggiata abbreviata per configurare  utenti IMS di Adobe, gruppi di utenti e profili di prodotto in  Admin Console di Adobe, e come sfruttare queste Adobe astrazioni IMS di   in AEM Author per definire e gestire autorizzazioni specifiche basate su gruppi.

[Procedura dettagliata sull&#39;accesso AEM e sulle autorizzazioni](./walk-through.md)

## Risorse Adobe Admin Console aggiuntive

La seguente documentazione contiene informazioni specifiche su [Adobe Admin Console](https://adminconsole.adobe.com) e problemi che possono facilitare una migliore comprensione dell&#39;Adobe Admin Console e il suo utilizzo per gestire gli utenti e l&#39;accesso tra  prodotti Experience Cloud.

+ [Panoramica di Adobe Admin Console Identity](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Ruoli di amministrazione di Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Ruoli per sviluppatori Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)