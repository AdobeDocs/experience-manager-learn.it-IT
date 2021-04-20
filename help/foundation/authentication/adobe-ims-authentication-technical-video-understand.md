---
title: Informazioni sull’autenticazione Adobe IMS con AEM su Adobe Managed Services
description: Adobe Experience Manager introduce il supporto Admin Console per le istanze AEM e l’autenticazione basata su Adobe IMS (Identity Management System) per AEM su Managed Services.   Questa integrazione consente ai clienti di AEM Managed Services di gestire tutti gli utenti di Experience Cloud in un’unica console Web unificata. Gli utenti e i gruppi possono essere assegnati ai profili di prodotto associati alle istanze AEM, consentendo l’accesso gestito centralmente alle istanze AEM specifiche.
version: 6.4, 6.5
feature: Users and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---


# Informazioni sull’autenticazione Adobe IMS con AEM su Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce il supporto Admin Console per le istanze AEM e l’autenticazione basata su Adobe IMS (Identity Management System) per AEM su Managed Services.   Questa integrazione consente ai clienti di AEM Managed Services di gestire tutti gli utenti di Experience Cloud in un’unica console Web unificata. Gli utenti e i gruppi possono essere assegnati ai profili di prodotto associati alle istanze AEM, consentendo l’accesso gestito centralmente alle istanze AEM specifiche.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Il supporto per l’autenticazione IMS di Adobe Experience Manager è riservato solo agli utenti &quot;interni&quot; (autori, revisori, amministratori, sviluppatori, ecc.) e non agli utenti finali esterni, come i visitatori del sito web.
* [Admin ](https://adminconsole.adobe.com/) Console rappresenterà i clienti AEM Managed Services come organizzazioni IMS e le istanze AEM come contesti di prodotto. Gli amministratori di sistema e di prodotto di Admin Console possono definire e gestire.
* AEM Managed Services sincronizza la topologia con Admin Console, creando una mappatura 1-1 tra un contesto di prodotto e un’istanza AEM.
* Il profilo di prodotto in Admin Console determina a quali istanze AEM un utente può accedere.
* Il supporto per l&#39;autenticazione include gli IDP conformi a SAML2 del cliente per SSO.
* Saranno supportati solo Enterprise ID o Federated ID (per l’SSO del cliente) (i Personal Adobe ID non sono supportati).

** Questa funzione è supportata per i clienti di AEM 6.4 SP3 e versioni successive per Adobe Managed Services.*

## Best practice {#best-practices}

### Applicazione delle autorizzazioni in Admin Console

È necessario evitare di applicare autorizzazioni e accesso a livello di utente sia in Admin Console che in Adobe Experience Manager.

In Admin Console è necessario concedere agli utenti l’accesso tramite Gruppi di utenti a livello di contesto del prodotto. I gruppi di utenti sono tipicamente espressi in base al ruolo logico all’interno dell’organizzazione per promuovere la riutilizzabilità dei gruppi nei prodotti Adobe Experience Cloud.

>[!NOTE]
>
> Se utilizzi AEM as a Cloud Service, assegna gli utenti Admin Console direttamente ai profili di prodotto. Le autorizzazioni di transizione tra gli utenti di Admin Console e i profili di prodotto tramite i gruppi di utenti di Admin Console non sono supportate per AEM as a Cloud Service.

### Applicazione delle autorizzazioni in Adobe Experience Manager

In Adobe Experience Manager, i gruppi di utenti sincronizzati da Adobe IMS devono essere aggiunti a [gruppi di utenti forniti da AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), che sono preconfigurati con le autorizzazioni appropriate per eseguire specifici set di attività in AEM. Gli utenti sincronizzati da Adobe IMS non devono essere aggiunti direttamente a [gruppi di utenti forniti da AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
