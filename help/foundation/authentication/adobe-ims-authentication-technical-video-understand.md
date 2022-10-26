---
title: Informazioni sull’autenticazione Adobe IMS con AEM in Adobe Managed Services
description: Adobe Experience Manager introduce un Admin Console di supporto per le istanze AEM e l’autenticazione basata su Adobe IMS (Identity Management System) per AEM su Managed Services.   Questa integrazione consente AEM clienti Managed Services di gestire tutti gli utenti di Experience Cloud in un’unica console Web unificata. Gli utenti e i gruppi possono essere assegnati ai profili di prodotto associati alle istanze AEM, consentendo l’accesso gestito centralmente alle istanze AEM specifiche.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 3%

---

# Informazioni sull’autenticazione Adobe IMS con AEM in Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce un Admin Console di supporto per le istanze AEM e l’autenticazione basata su Adobe Identity Management System (IMS) per AEM su Managed Services.   Questa integrazione consente AEM clienti Managed Services di gestire tutti gli utenti di Experience Cloud in un’unica console Web unificata. Gli utenti e i gruppi possono essere assegnati ai profili di prodotto associati alle istanze AEM, consentendo l’accesso gestito centralmente alle istanze AEM specifiche.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Il supporto per l’autenticazione Adobe Experience Manager IMS è riservato solo agli utenti &quot;interni&quot; (autori, revisori, amministratori, sviluppatori e così via) e non agli utenti finali esterni, come i visitatori del sito web.
* [Admin Console](https://adminconsole.adobe.com/) rappresenta AEM clienti Managed Services come organizzazioni IMS e le istanze AEM come contesti di prodotto. Gli amministratori di sistema e di prodotto di Admin Console possono definire e gestire.
* AEM Managed Services sincronizza la topologia con l’Admin Console, creando una mappatura da 1 a 1 tra un contesto di prodotto e un’istanza AEM.
* Il profilo di prodotto in Admin Console determina AEM istanze a cui un utente può accedere.
* Il supporto per l&#39;autenticazione include gli IDP conformi a SAML2 del cliente per SSO.
* Sono supportati solo Enterprise ID o Federated ID (per l’SSO del cliente) (gli ID Personal Adobe non sono supportati).

*&#42;Questa funzione è supportata per AEM 6.4 SP3 e versioni successive per i clienti di Adobe Managed Services.*

## Best practice {#best-practices}

### Applicazione delle autorizzazioni in Admin Console

L’applicazione delle autorizzazioni e dell’accesso a livello di utente deve essere evitata sia in Admin Console che in Adobe Experience Manager.

In, agli utenti di Admin Console deve essere concesso l’accesso tramite Gruppi di utenti a livello di contesto del prodotto. I gruppi di utenti sono tipicamente espressi in base al ruolo logico all&#39;interno dell&#39;organizzazione per promuovere la riutilizzabilità dei gruppi tra i prodotti Adobe Experience Cloud.

>[!NOTE]
>
> Se utilizzi AEM as a Cloud Service, assegna gli utenti di Admin Console direttamente ai profili di prodotto. Le autorizzazioni di transizione tra utenti di Admin Console e profili di prodotto tramite gruppi di utenti di Admin Console non sono supportate per AEM as a Cloud Service.

### Applicazione delle autorizzazioni in Adobe Experience Manager

In Adobe Experience Manager, i gruppi di utenti sincronizzati da Adobe IMS devono essere aggiunti in termini a [Gruppi di utenti forniti da AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=it), preconfigurati con le autorizzazioni appropriate per eseguire specifici set di attività in AEM. Gli utenti sincronizzati da Adobe IMS non devono essere aggiunti direttamente a [Gruppi di utenti forniti da AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html).
