---
title: Utilizzo del reporting delle transazioni in AEM Forms
description: I rapporti sulle transazioni in AEM Forms ti consentono di tenere un conteggio di tutte le transazioni effettuate a partire da una data specificata nella distribuzione AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
duration: 96
source-git-commit: 4b88045a626b5e7bd1386e62ee54ac6fe2ce9282
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---

# Utilizzo del reporting delle transazioni in AEM Forms{#using-transaction-reporting-in-aem-forms}

Con AEM Forms 6.4.1 è stato introdotto il reporting delle transazioni per acquisire il numero di invii di moduli, il rendering dei documenti utilizzando i servizi documentali e il rendering delle comunicazioni interattive (canali Web e di stampa). Questa funzionalità è attualmente disponibile solo nello stack OSGI di AEM Forms.

## Abilitazione del reporting delle transazioni {#enabling-transaction-reporting}

Per impostazione predefinita, la registrazione delle transazioni è disabilitata. Per abilitare la registrazione delle transazioni, procedere come segue:

* [Apri configMgr](http://localhost:4502/system/console/configMgr)
* Cerca &quot;Forms Transaction Reporting&quot; (Generazione rapporti transazioni)
* Selezionare la casella di controllo Registra transazioni
* Salva le modifiche

Una volta abilitata la generazione di rapporti sulle transazioni, puoi inviare Forms adattivo, generare documenti utilizzando servizi documentali o eseguire il rendering di documenti di comunicazione interattiva per vedere la generazione di rapporti sulle transazioni in azione.

## Visualizzazione del rapporto sulle transazioni {#viewing-transaction-report}

Per visualizzare il rapporto sulle transazioni, accedi ad AEM Forms come amministratore. Solo i membri del gruppo fd-Administrator possono visualizzare il report delle transazioni.

Seleziona strumenti | Forms | Visualizza report transazioni

oppure visualizzare il report della transazione facendo clic su [qui](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![Reporting sulle transazioni](assets/transactionreporting.gif)

Nella schermata precedente Documento elaborato indica il numero di documenti generati utilizzando i servizi documentali. Documenti sottoposti a rendering indica il numero di documenti di comunicazione interattiva (Web e stampa) sottoposti a rendering. Forms inviato indica il numero di invii di moduli adattivi.

Una transazione rimane nel buffer per un periodo specificato (tempo buffer di scaricamento + tempo replica inversa). Per impostazione predefinita, occorrono circa 90 secondi prima che il conteggio delle transazioni venga visualizzato nel rapporto sulle transazioni.

Azioni quali l’invio di un modulo PDF, l’utilizzo dell’interfaccia utente dell’agente per visualizzare in anteprima una comunicazione interattiva o l’utilizzo di metodi di invio di moduli non standard non vengono considerate transazioni. AEM Forms fornisce un’API per registrare tali transazioni. Chiama l’API dalle implementazioni personalizzate per registrare una transazione.

Se visualizzi il rapporto sulle transazioni nell’istanza di authoring, assicurati che la replica inversa sia configurata su tutte le istanze di pubblicazione.

Per ulteriori informazioni sulla generazione di rapporti sulle transazioni [fai clic qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
