---
title: Informazioni  autenticazione IMS Adobe con AEM sui servizi gestiti Adobe
description: Adobe Experience Manager introduce  Admin Console di supporto per AEM istanze e  Adobe di autenticazione basata su IMS ( Identity Management System) per AEM su Managed Services.   Questa integrazione consente AEM clienti Managed Services di gestire tutti  utenti Experienci Cloud in un'unica console Web unificata. Gli utenti e i gruppi possono essere assegnati ai profili di prodotto associati alle istanze AEM, garantendo l'accesso gestito centralmente alle istanze AEM specifiche.
version: 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---


# Informazioni  autenticazione IMS Adobe con AEM sui servizi gestiti Adobe{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce  Admin Console di supporto per AEM istanze e  Adobe di autenticazione basata su IMS ( Identity Management System) per AEM su Managed Services.   Questa integrazione consente AEM clienti Managed Services di gestire tutti  utenti Experienci Cloud in un&#39;unica console Web unificata. Gli utenti e i gruppi possono essere assegnati ai profili di prodotto associati alle istanze AEM, garantendo l&#39;accesso gestito centralmente alle istanze AEM specifiche.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Il supporto per l&#39;autenticazione Adobe Experience Manager IMS è riservato solo agli utenti &quot;interni&quot; (autori, revisori, amministratori, sviluppatori, ecc.) e non agli utenti finali esterni, ad esempio i visitatori del sito Web.
* [Admin Console](https://adminconsole.adobe.com/) rappresenterà AEM clienti Managed Services come organizzazioni IMS e le istanze AEM come contesti di prodotto.  Admin Console di amministratori di sistema e prodotti possono definire e gestire.
* AEM Managed Services sincronizza la topologia con  Admin Console, creando una mappatura da 1 a 1 tra un contesto prodotto e AEM istanza.
* Profilo di prodotto in  Admin Console determinerà AEM istanze a cui un utente può accedere.
* Il supporto per l&#39;autenticazione include gli IDPs conformi a SAML2 per l&#39;SSO.
* Saranno supportati solo Enterprise ID o Federated ID (per SSO cliente) (Personal  Adobe ID non sono supportati).

** Questa funzione è supportata per AEM 6.4 SP3 e versioni successive per i clienti di Adobe Managed Services.*

## Best practices {#best-practices}

### Applicazione delle autorizzazioni in  Admin Console

È necessario evitare di applicare autorizzazioni e accesso a livello di utente sia nel  Admin Console che in Adobe Experience Manager.

In  Admin Console agli utenti dovrebbe essere consentito l&#39;accesso tramite Gruppi di utenti a livello di contesto del prodotto. I gruppi di utenti sono tipicamente espressi in base al ruolo logico all&#39;interno dell&#39;organizzazione per promuovere la riutilizzabilità dei gruppi nei prodotti Adobe Experience Cloud.

>[!NOTE]
>
> Se utilizzate AEM come Cloud Service, assegnate  utenti Admin Console direttamente ai profili di prodotto. Le autorizzazioni di transizione tra  utenti del Admin Console a Profili di prodotto tramite  gruppi di utenti di Admin Console non sono supportate per AEM come Cloud Service.

### Applicazione delle autorizzazioni in Adobe Experience Manager

In Adobe Experience Manager, i gruppi di utenti sincronizzati da  Adobe IMS devono essere aggiunti in futuro ai gruppi [di utenti forniti da](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)AEM, che vengono preconfigurati con le autorizzazioni appropriate per eseguire specifici set di attività in AEM. Gli utenti sincronizzati da  Adobe IMS non devono essere aggiunti direttamente ai gruppi [di utenti forniti da](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)AEM.
