---
title: Informazioni sull’autenticazione Adobe IMS con AEM in Adobe Managed Services
description: Adobe Experience Manager introduce il supporto Admin Console per le istanze AEM e l’autenticazione basata su Adobe IMS (Identity Management System) per AEM su Managed Services.   Questa integrazione consente ai clienti AEM Managed Services di gestire tutti gli utenti Experience Cloud in un’unica console web unificata. Gli utenti e i gruppi possono essere assegnati a profili di prodotto associati alle istanze AEM, consentendo l’accesso gestito a livello centrale alle istanze AEM specifiche.
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
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 3%

---

# Informazioni sull’autenticazione Adobe IMS con AEM in Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce il supporto Admin Console per le istanze AEM e l’autenticazione basata su Adobe Identity Management System (IMS) per AEM su Managed Services.   Questa integrazione consente ai clienti AEM Managed Services di gestire tutti gli utenti Experience Cloud in un’unica console web unificata. Gli utenti e i gruppi possono essere assegnati a profili di prodotto associati alle istanze AEM, consentendo l’accesso gestito a livello centrale alle istanze AEM specifiche.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Il supporto per l’autenticazione IMS di Adobe Experience Manager è destinato solo agli utenti &quot;interni&quot; (autori, revisori, amministratori, sviluppatori e così via) e non agli utenti finali esterni come i visitatori del sito web.
* [Admin Console](https://adminconsole.adobe.com/) rappresenta i clienti Managed Services dell’AEM come organizzazioni IMS e le istanze AEM come contesti di prodotto. Gli amministratori di prodotto e sistema di Admin Console possono definire e gestire.
* AEM Managed Services sincronizza la topologia con Admin Console, creando una mappatura da 1 a 1 tra un contesto di prodotto e un’istanza AEM.
* Il profilo di prodotto di questo Admin Console determina le istanze AEM a cui un utente può accedere.
* Il supporto dell’autenticazione include gli IDP del cliente conformi a SAML2 per l’SSO.
* Sono supportati solo Enterprise ID o Federated ID (per l’SSO del cliente) (gli ID Adobe personali non sono supportati).

*&#42;Questa funzione è supportata per AEM 6.4 SP3 e versioni successive per i clienti di Adobe Managed Services.*

## Best practice {#best-practices}

### Applicazione delle autorizzazioni in Admin Console

È necessario evitare di applicare autorizzazioni e accesso a livello di utente sia in Admin Console che in Adobe Experience Manager.

In, agli utenti Admin Console deve essere concesso l’accesso tramite Gruppi di utenti a livello di contesto di prodotto. In genere, i gruppi di utenti si esprimono meglio in base al ruolo logico all’interno dell’organizzazione per promuoverne la riutilizzabilità tra i prodotti Adobe Experience Cloud.

>[!NOTE]
>
> Se utilizzi AEM as a Cloud Service, assegna gli utenti Admin Console direttamente ai profili di prodotto. Le autorizzazioni transitive tra gli utenti Admin Console e i profili di prodotto tramite i gruppi di utenti Admin Console non sono supportate per AEM as a Cloud Service.

### Applicazione delle autorizzazioni in Adobe Experience Manager

In Adobe Experience Manager, i gruppi di utenti sincronizzati da Adobe IMS devono essere aggiunti a termine a [Gruppi di utenti forniti dall’AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=it), preconfigurate con le autorizzazioni appropriate per eseguire set specifici di attività in AEM. Gli utenti sincronizzati da Adobe IMS non devono essere aggiunti direttamente a [Gruppi di utenti forniti dall’AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=it).
