---
title: Informazioni sull’autenticazione Adobe IMS con AEM su Adobe Managed Services
description: Adobe Experience Manager introduce un Admin Console di supporto per le istanze AEM e l’autenticazione basata su Adobe IMS (Identity Management System) per AEM su Managed Services.   Questa integrazione consente AEM clienti Managed Services di gestire tutti gli utenti di Experience Cloud in un’unica console Web unificata. Gli utenti e i gruppi possono essere assegnati ai profili di prodotto associati alle istanze AEM, consentendo l’accesso gestito centralmente alle istanze AEM specifiche.
version: 6.4, 6.5
feature: Utenti e gruppi
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architettura
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---


# Informazioni sull’autenticazione Adobe IMS con AEM su Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce un Admin Console di supporto per le istanze AEM e l’autenticazione basata su Adobe IMS (Identity Management System) per AEM su Managed Services.   Questa integrazione consente AEM clienti Managed Services di gestire tutti gli utenti di Experience Cloud in un’unica console Web unificata. Gli utenti e i gruppi possono essere assegnati ai profili di prodotto associati alle istanze AEM, consentendo l’accesso gestito centralmente alle istanze AEM specifiche.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Il supporto per l’autenticazione Adobe Experience Manager IMS è riservato solo agli utenti &quot;interni&quot; (autori, revisori, amministratori, sviluppatori, ecc.) e non agli utenti finali esterni, come i visitatori del sito web.
* [Admin ](https://adminconsole.adobe.com/) Console rappresenta AEM clienti Managed Services come organizzazioni IMS e le istanze AEM come contesti di prodotto. Gli amministratori di sistema e di prodotto di Admin Console possono definire e gestire.
* AEM Managed Services sincronizza la topologia con l’Admin Console, creando una mappatura da 1 a 1 tra un contesto di prodotto e un’istanza AEM.
* Il profilo di prodotto in Admin Console determinerà AEM istanze a cui un utente può accedere.
* Il supporto per l&#39;autenticazione include gli IDP conformi a SAML2 del cliente per SSO.
* Saranno supportati solo Enterprise ID o Federated ID (per l’SSO del cliente) (gli ID Personal Adobe non sono supportati).

** Questa funzione è supportata per AEM 6.4 SP3 e versioni successive per i clienti di Adobe Managed Services.*

## Best practice {#best-practices}

### Applicazione delle autorizzazioni in Admin Console

L’applicazione delle autorizzazioni e dell’accesso a livello di utente deve essere evitata sia in Admin Console che in Adobe Experience Manager.

In Admin Console, agli utenti deve essere concesso l’accesso tramite Gruppi di utenti a livello di contesto del prodotto. I gruppi di utenti sono tipicamente espressi in base al ruolo logico all&#39;interno dell&#39;organizzazione per promuovere la riutilizzabilità dei gruppi tra i prodotti Adobe Experience Cloud.

>[!NOTE]
>
> Se utilizzi AEM come Cloud Service, assegna gli utenti di Admin Console direttamente ai profili di prodotto. Le autorizzazioni di transizione tra utenti di Admin Console e profili di prodotto tramite gruppi di utenti di Admin Console non sono supportate per AEM come Cloud Service.

### Applicazione delle autorizzazioni in Adobe Experience Manager

In Adobe Experience Manager, i gruppi di utenti sincronizzati da IMS di Adobe devono essere aggiunti in termini a [gruppi di utenti AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), che sono preconfigurati con le autorizzazioni appropriate per eseguire specifici set di attività in AEM. Gli utenti sincronizzati da Adobe IMS non devono essere aggiunti direttamente ai gruppi di utenti forniti da [AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
