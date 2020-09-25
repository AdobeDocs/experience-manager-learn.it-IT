---
title: Utilizzo dei rapporti sulle transazioni in  AEM Forms
seo-title: Utilizzo dei rapporti sulle transazioni in  AEM Forms
description: I rapporti sulle transazioni in  AEM Forms consentono di tenere un conteggio di tutte le transazioni effettuate da una data specificata nella distribuzione  AEM Forms.
seo-description: I rapporti sulle transazioni in  AEM Forms consentono di tenere un conteggio di tutte le transazioni effettuate da una data specificata nella distribuzione  AEM Forms.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: adaptive-forms
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---


# Utilizzo dei rapporti sulle transazioni in  AEM Forms{#using-transaction-reporting-in-aem-forms}

Con  AEM Forms 6.4.1 è stata introdotta la generazione di rapporti sulle transazioni per acquisire il numero di invii di moduli, il rendering di documenti tramite document services e il rendering di comunicazioni interattive (canali Web e di stampa). Questa funzionalità è principalmente destinata ai clienti che desiderano ottenere la licenza per il software in base al numero di invii di moduli e/o di documenti sottoposti a rendering. Questa funzionalità è attualmente disponibile solo  stack AEM Forms OSGI.

## Abilitazione del reporting delle transazioni {#enabling-transaction-reporting}

Per impostazione predefinita, la registrazione della transazione è disabilitata. Per abilitare la registrazione delle transazioni, segui i passaggi indicati di seguito:

* [Aprire configMgr](http://localhost:4502/system/console/configMgr)
* Cerca &quot;Report transazione Forms&quot;
* Selezionare la casella di controllo &quot;Registra transazioni&quot;
* Salvare le modifiche

Una volta attivato il reporting delle transazioni, è possibile inviare l&#39;Forms adattivo, generare documenti utilizzando document services o eseguire il rendering dei documenti di comunicazione interattiva per visualizzare il reporting delle transazioni in azione.

## Visualizzazione del rapporto sulle transazioni {#viewing-transaction-report}

Per visualizzare il rapporto sulla transazione, accedi a  AEM Forms come amministratore. Solo i membri del gruppo fd-Administrator possono visualizzare il rapporto sulle transazioni.

Seleziona strumenti | Forms | Visualizza rapporto transazione

o visualizzare il rapporto sulla transazione facendo clic [qui](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransctionReporting](assets/transactionreporting.gif)

Nella schermata precedente a Document Processed è riportato il numero di documenti generati tramite Document Services. Documenti sottoposti a rendering è il numero di documenti di comunicazione interattiva (Web e stampa) sottoposti a rendering. Forms Inviato indica il numero di invii di moduli adattivi.

Una transazione rimane nel buffer per un periodo specificato (tempo buffer di scaricamento + tempo di replica inversa). Per impostazione predefinita, il conteggio delle transazioni impiega circa 90 secondi per riflettere nel rapporto delle transazioni.

Azioni come l&#39;invio di un modulo PDF, l&#39;utilizzo dell&#39;interfaccia utente dell&#39;agente per visualizzare l&#39;anteprima di una comunicazione interattiva o l&#39;utilizzo di metodi di invio di moduli non standard non vengono considerate come transazioni.  AEM Forms fornisce un&#39;API per registrare tali transazioni. Chiama l&#39;API dalle tue implementazioni personalizzate per registrare una transazione.

Se visualizzi il rapporto sulle transazioni nell’istanza di creazione, accertati che la replica inversa sia configurata in tutte le istanze di pubblicazione.

Per saperne di più sul reporting delle transazioni [fare clic qui](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

