---
title: Notifica di sicurezza AEM (novembre 2018)
seo-title: AEM Security Notification (November 2018)
description: Dispatcher di notifica di sicurezza Experience Manager AEM
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 14%

---

# Notifica di sicurezza AEM (novembre 2018)

## Riepilogo

Questo articolo tratta alcune vulnerabilità vecchie e recenti che sono state recentemente segnalate nell’AEM. Tieni presente che la maggior parte delle vulnerabilità identificate erano problemi noti per il prodotto AEM e che la mitigazione è stata identificata in precedenza; per tali vulnerabilità è disponibile una nuova versione di Dispatcher. Adobe invita inoltre i clienti a completare il [Elenco di controllo della sicurezza AEM](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/security-checklist.html) e seguire le linee guida pertinenti.

## Azione richiesta

* Le distribuzioni AEM devono iniziare a utilizzare la versione più recente di Dispatcher.
* Le regole di sicurezza del dispatcher devono essere applicate in base alla configurazione consigliata.
* Il [Elenco di controllo della sicurezza AEM](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/security-checklist.html) devono essere completate per le implementazioni AEM.

## Vulnerabilità e risoluzioni

| Problema   | Risoluzione | Collegamenti |
|-------|------------|-------|
| Ignorare le regole del Dispatcher AEM | Installa la versione più recente di Dispatcher (4.3.1) e segui la configurazione di Dispatcher consigliata. | Consulta [Note sulla versione di Dispatcher per l’AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configurazione di Dispatcher](https://helpx.adobe.com/it/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Il filtro URL ignora la vulnerabilità che potrebbe essere utilizzata per aggirare le regole del dispatcher - CVE-2016-0957 | Questo problema è stato risolto in una versione precedente di Dispatcher, ma ora è consigliabile installare la versione più recente di Dispatcher (4.3.1) e seguire la configurazione di Dispatcher consigliata. | Consulta [Note sulla versione di Dispatcher per l’AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configurazione di Dispatcher](https://helpx.adobe.com/it/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilità XSS relativa ai file SWF archiviati | Questo problema è stato risolto con correzioni di sicurezza rilasciate in precedenza. | Consulta [Bollettino sulla sicurezza dell’AEM APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Esplosioni correlate a password | Seguire i consigli riportati nell&#39;elenco di controllo della protezione per ottenere password più sicure. | Consulta [Elenco di controllo della sicurezza AEM](https://helpx.adobe.com/it/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Esposizione all&#39;utilizzo del disco per gli utenti anonimi | Questo problema è stato risolto per AEM 6.1 e versioni successive. Per AEM 6.0 è possibile modificare le autorizzazioni predefinite per renderlo più restrittivo. | Consulta [note sulla versione](https://helpx.adobe.com/it/experience-manager/aem-previous-versions.html)per AEM 6.1 e versioni successive. |
| Esposizione di Open Social Proxy per utenti anonimi | Questo problema è stato risolto nelle versioni a partire da 6.0 SP2. | Consulta [note sulla versione](https://helpx.adobe.com/it/experience-manager/aem-previous-versions.html) per AEM 6.1 e versioni successive. |
| Accesso a CRX Explorer sulle istanze di produzione | La gestione dell’accesso a CRX Explorer è già inclusa nell’elenco di controllo della sicurezza, CRX Explorer deve essere rimosso dall’istanza di authoring e pubblicazione di produzione e il controllo dello stato della sicurezza segnala tale rimozione se non la rimuove. | Consulta [Elenco di controllo della sicurezza AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security-checklist.html?lang=it). |
| BGServlets è esposto | Questo problema è stato risolto dopo l’AEM 6.2. | Consulta [Note sulla versione di AEM 6.2](https://helpx.adobe.com/it/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guida utente di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Note sulla versione di AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Bollettini sulla sicurezza dell’AEM](https://helpx.adobe.com/security.html#experience-manager)

