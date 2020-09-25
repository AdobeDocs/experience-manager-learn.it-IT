---
title: Notifica di sicurezza AEM (novembre 2018)
seo-title: Notifica di sicurezza AEM (novembre 2018)
description: AEM  Experience Manager del dispatcher delle notifiche sulla sicurezza
seo-description: AEM  Experience Manager del dispatcher delle notifiche sulla sicurezza
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 5%

---


# Notifica di sicurezza AEM (novembre 2018)

## Riepilogo

Questo articolo risolve alcune vulnerabilità recenti e vecchie che sono state recentemente segnalate in AEM. Notare che la maggior parte delle vulnerabilità identificate erano problemi noti per il prodotto AEM e la mitigazione sono stati precedentemente identificati, una nuova versione del dispatcher è disponibile per le nuove vulnerabilità.  Adobe esorta inoltre i clienti a completare la lista di controllo di sicurezza [AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) e a seguire le linee guida pertinenti.

## Azione richiesta

* AEM le distribuzioni devono iniziare utilizzando la versione più recente del dispatcher.
* Le regole di protezione del dispatcher devono essere applicate in base alla configurazione consigliata.
* È necessario completare l&#39;elenco di controllo [AEM sicurezza](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) per AEM distribuzioni.

## Vulnerabilità e risoluzioni

| Problema | Risoluzione | Collegamenti |
|-------|------------|-------|
| Ignorare AEM regole del dispatcher | Installate la versione più recente del dispatcher (4.3.1) e seguite la configurazione del dispatcher consigliata. | Consultate [AEM Note](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) sulla versione del dispatcher e [Configurazione del dispatcher](https://helpx.adobe.com/it/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilità di bypass del filtro URL che potrebbe essere utilizzata per aggirare le regole del dispatcher - CVE-2016-0957 | Questo problema è stato risolto in una versione precedente del Dispatcher, ma ora si consiglia di installare la versione più recente del Dispatcher (4.3.1) e di seguire la configurazione del Dispatcher consigliata. | Consultate [AEM Note](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) sulla versione del dispatcher e [Configurazione del dispatcher](https://helpx.adobe.com/it/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| vulnerabilità XSS correlata ai file SWF memorizzati | Questo problema è stato risolto con correzioni di sicurezza rilasciate in precedenza. | Consultate [AEM Bollettino sulla sicurezza APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Sfruttamenti relativi alla password | Seguite la raccomandazione nell&#39;elenco di controllo Sicurezza per ottenere password più sicure. | Vedere Elenco di controllo [AEM protezione](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Esposizione dell&#39;utilizzo del disco per gli utenti anonimi | Questo problema è stato risolto per AEM 6.1 e versioni successive, perché AEM 6.0 le autorizzazioni out-of-box possono essere modificate in modo da essere più restrittive. | Consultate [le](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)note sulla versione per AEM 6.1 e versioni precedenti. |
| Esposizione del proxy Open Social per gli utenti anonimi | È stato risolto nelle versioni a partire da 6.0 SP2. | Consultate le note [di](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) rilascio per AEM 6.1 e versioni precedenti. |
| Accesso a CRX Explorer per le istanze di produzione | La gestione dell&#39;accesso a CRX Explorer è già inclusa nell&#39;elenco di controllo della sicurezza. CRX Explorer deve essere rimosso dall&#39;autore della produzione e dalla pubblicazione e il controllo dello stato di protezione lo segnala se non viene rimosso. | Vedere [AEM elenco](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)di controllo della protezione. |
| BGServlets è esposto | Ciò è stato risolto a partire dal AEM 6.2. | See [AEM 6.2 Release Notes](https://helpx.adobe.com/it/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guida utente del dispatcher AEM](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Note sulla versione di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM Bollettini sulla sicurezza](https://helpx.adobe.com/security.html#experience-manager)

