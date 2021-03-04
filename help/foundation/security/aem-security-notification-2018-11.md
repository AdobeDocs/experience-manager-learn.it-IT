---
title: Notifica di sicurezza AEM (novembre 2018)
seo-title: Notifica di sicurezza AEM (novembre 2018)
description: Dispatcher per le notifiche sulla sicurezza di AEM Experience Manager
seo-description: Dispatcher per le notifiche sulla sicurezza di AEM Experience Manager
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 7%

---


# Notifica di sicurezza AEM (novembre 2018)

## Riepilogo

Questo articolo tratta alcune vulnerabilità recenti e vecchie che sono state recentemente segnalate in AEM. Nota che la maggior parte delle vulnerabilità identificate erano problemi noti per il prodotto AEM e che la mitigazione era stata precedentemente identificata, è disponibile una nuova versione del dispatcher per le nuove vulnerabilità. Adobe invita inoltre i clienti a completare la [Lista di controllo AEM per la sicurezza](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) e a seguire le linee guida pertinenti.

## Azione richiesta

* Le implementazioni di AEM devono iniziare a utilizzare la versione più recente di Dispatcher.
* Le regole di sicurezza del dispatcher devono essere applicate in base alla configurazione consigliata.
* La [Lista di controllo della sicurezza AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) deve essere completata per le implementazioni AEM.

## Vulnerabilità e risoluzioni

| Problema | Risoluzione | Collegamenti |
|-------|------------|-------|
| Salto delle regole di AEM Dispatcher | Installa la versione più recente di Dispatcher(4.3.1) e segui la configurazione consigliata del dispatcher. | Consulta [Note sulla versione di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configurazione di Dispatcher](https://helpx.adobe.com/it/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilità del filtro URL che potrebbe essere utilizzata per aggirare le regole del dispatcher - CVE-2016-0957 | Questo problema è stato risolto in una versione precedente di Dispatcher, ma ora si consiglia di installare la versione più recente di Dispatcher (4.3.1) e di seguire la configurazione consigliata di Dispatcher. | Consulta [Note sulla versione di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configurazione di Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilità XSS relativa ai file SWF memorizzati | Questo problema è stato risolto con correzioni di sicurezza rilasciate in precedenza. | Consulta il [Bollettino sulla sicurezza AEM APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Sfruttamenti relativi alle password | Segui le raccomandazioni in Lista di controllo sicurezza per password più forti. | Consulta [Elenco di controllo per la sicurezza AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Esposizione all&#39;utilizzo del disco per utenti anonimi | Questo problema è stato risolto per AEM 6.1 e versioni successive; per AEM 6.0 le autorizzazioni predefinite possono essere modificate in modo da essere più restrittive. | Consulta le [note sulla versione](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it#previous-updates)per AEM 6.1 e versioni precedenti. |
| Esposizione del proxy Open Social per utenti anonimi | Questo problema è stato risolto nelle versioni a partire dalla versione 6.0 SP2. | Consulta le [note sulla versione](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) per AEM 6.1 e versioni precedenti. |
| Accesso a CRX Explorer sulle istanze di produzione | La gestione dell&#39;accesso a CRX Explorer è già inclusa nella Lista di controllo sicurezza, CRX Explorer deve essere rimosso dall&#39;autore di produzione e pubblicato e il controllo dello stato di sicurezza lo segnala se non rimosso. | Consulta [Elenco di controllo per la sicurezza AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets è esposto | Questo problema è stato risolto a partire da AEM 6.2. | Consulta [Note sulla versione di AEM 6.2](https://helpx.adobe.com/it/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guida utente di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Note sulla versione di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Bollettini sulla sicurezza di AEM](https://helpx.adobe.com/security.html#experience-manager)

