---
title: Notifica di sicurezza AEM (novembre 2018)
seo-title: Notifica di sicurezza AEM (novembre 2018)
description: Dispatcher per le notifiche sulla sicurezza di AEM Experience Manager
seo-description: Dispatcher per le notifiche sulla sicurezza di AEM Experience Manager
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Sicurezza
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 8%

---


# Notifica di sicurezza AEM (novembre 2018)

## Riepilogo

Questo articolo tratta alcune vulnerabilità recenti e vecchie che sono state recentemente segnalate in AEM. Nota che la maggior parte delle vulnerabilità identificate erano problemi noti per il prodotto AEM e l’attenuazione è stata precedentemente identificata, è disponibile una nuova versione del dispatcher per le nuove vulnerabilità. L&#39;Adobe esorta inoltre i clienti a completare la [AEM lista di controllo della sicurezza](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) e a seguire le linee guida pertinenti.

## Azione richiesta

* AEM le distribuzioni devono iniziare a utilizzare la versione più recente di Dispatcher.
* Le regole di sicurezza del dispatcher devono essere applicate in base alla configurazione consigliata.
* La [AEM Lista di controllo protezione](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) deve essere completata per AEM distribuzioni.

## Vulnerabilità e risoluzioni

| Problema | Risoluzione | Collegamenti |
|-------|------------|-------|
| Salto delle regole del Dispatcher AEM | Installa la versione più recente di Dispatcher(4.3.1) e segui la configurazione consigliata del dispatcher. | Consulta [AEM Note sulla versione di Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configurazione di Dispatcher](https://helpx.adobe.com/it/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilità del filtro URL che potrebbe essere utilizzata per aggirare le regole del dispatcher - CVE-2016-0957 | Questo problema è stato risolto in una versione precedente di Dispatcher, ma ora si consiglia di installare la versione più recente di Dispatcher (4.3.1) e di seguire la configurazione consigliata di Dispatcher. | Consulta [AEM Note sulla versione di Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configurazione di Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilità XSS relativa ai file SWF memorizzati | Questo problema è stato risolto con correzioni di sicurezza rilasciate in precedenza. | Consultare il [AEM Bollettino sulla sicurezza APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Sfruttamenti relativi alle password | Segui le raccomandazioni in Lista di controllo sicurezza per password più forti. | Vedere [AEM Lista di controllo protezione](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Esposizione all&#39;utilizzo del disco per utenti anonimi | Questo problema è stato risolto per AEM 6.1 e versioni successive, perché AEM 6.0 le autorizzazioni predefinite possono essere modificate in modo da essere più restrittive. | Consulta le [note sulla versione](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it#previous-updates)per AEM 6.1 e versioni precedenti. |
| Esposizione del proxy Open Social per utenti anonimi | Questo problema è stato risolto nelle versioni a partire dalla versione 6.0 SP2. | Consulta le [note sulla versione](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) per AEM 6.1 e versioni precedenti. |
| Accesso a CRX Explorer sulle istanze di produzione | La gestione dell&#39;accesso a CRX Explorer è già inclusa nella Lista di controllo sicurezza, CRX Explorer deve essere rimosso dall&#39;autore di produzione e pubblicato e il controllo dello stato di sicurezza lo segnala se non rimosso. | Vedere [AEM Lista di controllo protezione](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets è esposto | Questo problema è stato risolto a partire dal AEM 6.2. | Consulta [AEM Note sulla versione 6.2](https://helpx.adobe.com/it/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guida utente di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Note sulla versione di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Bollettini sulla sicurezza AEM](https://helpx.adobe.com/security.html#experience-manager)

